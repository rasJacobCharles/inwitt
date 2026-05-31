# Inwitt

> **Inwitt** *(Old English: inner knowledge, internal wisdom)* is an open-source, neural-inspired Long-Term Memory (LTM) layer for AI harnesses and personal usage. It allows you or an artificial agents to securely read, navigate, and curate a your knowledge base structured like the human brain.

---

## 🌌 The Concept

AI agents suffer from transient memory, restricted by rigid context windows. Modern vector databases treat all information equally, missing the organic way humans recall thoughts. 

**Inwitt** acts as an extended, shared memory palace between you and your AI. Built entirely on top of plain-text Markdown (`.md`) files enriched with `[[wikilinks]]` and `#tags`, Inwitt introduces **Synaptic Weighting**. 

Instead of relying solely on raw text matches, Inwitt builds an internal graph of your data. The more a note is linked to, or the more frequently it is recalled, the stronger its "synaptic path" becomes. This ensures highly important or deeply integrated concepts naturally resurface faster—mimicking biological memory.

---

## 🚀 Core Capabilities

- **🧠 Synaptic Graph Indexing:** Organizes knowledge via node connectivity (link density) and temporal decay (recency), matching the mechanics of human cognitive recall.
- **📂 Zero Vendor Lock-In:** Your data remains your data. Inwitt works directly with standard, human-readable Markdown files (perfect for Obsidian or Logseq users).
- **🛡️ Hybrid Privacy Architecture:** Simple frontmatter metadata allows you to designate notes as `public` or `private`, letting you selectively share knowledge with external agents.
- **✍️ Bi-Directional Curation:** Your AI harness isn't just a passive reader. Through Inwitt, agents can organically forge new links, update concepts, and synthesize new notes into your vault.
