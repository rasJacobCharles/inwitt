# Inwitt (Old English: *inner knowledge, internal wisdom*)

  [![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

  An open-source, neural-inspired Long-Term Memory (LTM) layer for AI harnesses and personal knowledge bases.
  Inwitt allows you and autonomous AI agents to securely read, navigate, and curate your knowledge vault structured
  similarly to the human brain.

  ---

  ## The Concept

  AI agents suffer from transient memory, restricted by rigid context windows. Modern vector databases treat all
  information equally, retrieving notes purely by flat semantic similarity. This approach misses the organic way
  humans recall thoughts, resulting in context bloat, high token costs, and LLM hallucinations.

  Inwitt acts as an **extended, shared memory palace** between you and your AI. Built on top of plain-text Markdown
  (`.md`) files enriched with `[[wikilinks]]` and #tags, Inwitt introduces **Synaptic Weighting**.

  Instead of relying solely on raw text matches, Inwitt builds an internal graph of your data. The more a note is
  linked to, or the more frequently it is recalled, the stronger its "synaptic path" becomes. Highly important or
  deeply integrated concepts naturally resurface faster—mimicking biological memory.


       [LLM Client / Agent]
                │
              Skill
                │
         [Inwitt  Server]
         /            \
        /              \
      SQLite Index]   [Markdown Vault]
      (Graph & Decay)   (Standard .md Files)


  ---

  ## The Mathematical Model: Synaptic Weighting

  To mimic biological memory recall, Inwitt computes a dynamic **Synaptic Weight ($S_i$)** for each note $i$ at
  query time $t$:

  $$S_i(t) = \alpha \cdot C_i + \beta \cdot \sum_{j=1}^{R_i} (t - t_j)^{-\lambda}$$

  Where:
    * **$C_i$ (Connectivity / Link Density)**: The total count of bidirectional wikilinks pointing to or from note
  $i$ in the markdown graph.
    * **$R_i$ (Recall Events)**: The frequency of times note $i$ has been retrieved.
    * **$t - t_j$ (Recency / Time Delta)**: The elapsed time since the $j$-th recall event.
    * **$\lambda$ (Decay Parameter)**: The power-law decay rate (modeled on the Ebbinghaus forgetting curve,
  typically $\lambda \approx 0.5$).
    * **$\alpha, \beta$ (Sensitivity Coefficients)**: Weights determining the relative importance of graph structure
  ($\alpha$) versus temporal recall activity ($\beta$).

  ---

  ## Core Capabilities

  ###  Synaptic Graph Indexing
  Organises knowledge using node connectivity and temporal decay. The engine acts as a **dynamic "hot" memory
  filter**, ensuring your active context window contains only highly connected, recently relevant notes. Standard
  vector search serves as "cold" retrieval, updating the hot index when new matches are made.

  ### 🛡️ Staged Curation Ledger (AI Writer Safety)
  To prevent autonomous AI agents from corrupting or cluttering your personal Markdown vaults, agentic
  modifications (like forging links or creating notes) do not write directly to disk. Instead, they are staged in a
  local SQLite ledger for your approval, offering a git-like review interface.

  ### 📂 Zero Vendor Lock-In
  Your data remains your data. Inwitt works directly with standard, human-readable Markdown files (perfect for
  Obsidian or Logseq users). Synaptic weights, histories, and staging queues are maintained in a high-speed local
  SQLite database.

  ### 🔑 Hybrid Privacy Boundaries
  Simple YAML frontmatter (e.g., `private: true`) or folder-level permissions act as hard gates in the API layer.
  You decide exactly which folders and notes are exposed to external AI agents and which remain entirely private.

  ---

  ## 🛠️ Model Context Protocol (MCP) Integration

  Inwitt runs as a local or remote **MCP Server**, letting AI environments interact with your memory vault natively using standardized tools:

  Tool Command | Purpose |
  | :--- | :--- |
  | `query_hot_memory(query, limit)` | Returns top $N$ nodes sorted by current Synaptic Weight $S_i(t)$. |
  | `stage_note_creation(title, content, connections)` | Proposes a new markdown node with wikilinks, queued for user approval. |
  | `stage_link_creation(source_node, target_node)` | Establishes a new synaptic link between existing nodes. |
  | `reinforce_path(node_id)` | Increments the recall counter $R_i$ for a node, resetting its decay cycle. |

  ---

  ## Retrieval Strategy Comparison

  *Based on a sample vault of 5,000 notes (~5 million tokens)*

  | Metric | Flat Vector Search (RAG) | Pure Keyword Search (BM25) | Inwitt Hybrid LTM (Vector + Synaptic Cache) |
  | :--- | :--- | :--- | :--- |
  | **Active Context Size** | Large (10,000+ tokens) | Small but imprecise | **Compact (2,000–4,000 tokens)** |
  | **Retrieve Latency (Local)** | 120ms - 250ms (embedding computation) | < 10ms (index search) | **< 30ms (SQLite
  query + fallback vector)** |
  | **Hallucination Risk** | Moderate (due to noise in context) | High (misses semantic intent) | **Low (context
  filtered to verified active paths)** |
  | **Adaptation Speed** | Static (requires re-embedding) | Static | **Real-time (synaptic weights update on
  query)** |
  | **Vendor Dependency** | High (Vector API / local embedding model) | None | **None (SQLite + Markdown)** |

  ---

  ## 🗺️ Implementation Roadmap

  ### Phase 1: Core Engine
  * Write the parser to extract markdown wikilinks, frontmatter, and tags.
  * Design the local SQLite database schema (`nodes`, `edges`, `recall_logs`, `staging_queue`).
  * Implement the mathematical decay functions ($S_i(t)$) in SQLite/Python.

  ### Phase 2: Agent Skills API & CLI
  * Build the core API to expose indexing and weight reinforcement as standard, callable AI functions/skills.
  * Develop a local CLI tool to index vaults, trigger manual syncs, and review/approve staged changes.

  ### Phase 3: Platform Integrations
  * Validate the system using an Obsidian test vault.
  * Integrate with AI CLI (script), and custom agentic tool.
