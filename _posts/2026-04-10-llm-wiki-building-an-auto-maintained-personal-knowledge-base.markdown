---
layout: single
title: "LLM Wiki: Building an Auto-Maintained Personal Knowledge Base"
date: 2026-04-10 23:00:00 +0800
categories: AI
tags: [LLM, Knowledge-Base, RAG, AI, Tutorial]
read_time: true
---

> Inspired by Andrej Karpathy's approach — let LLMs do the "nerd work," you just think.

We've all faced this dilemma: reading great articles, papers, and notes, but they're scattered everywhere, unable to form a cohesive system. Tried using Obsidian or Notion to maintain a knowledge base, but maintenance costs quickly exhaust motivation. Using ChatGPT or NotebookLM for RAG (Retrieval-Augmented Generation) means the model has to rediscover knowledge with every query — no accumulation.

What if there was a system where:

- You drop in an article, and the LLM **automatically extracts key information and updates all related pages**;
- It automatically maintains **bidirectional links, indexes, and change logs**, never getting bored;
- You end up with a **continuously growing, interconnected knowledge wiki** — your personal Wikipedia.

This is **LLM Wiki**. This article provides a complete, copy-paste-ready guide that gets you started in 30 minutes.

---

## Core Idea: From RAG to Incremental Knowledge Compilation

Traditional RAG works like this: upload document → ask question → retrieve relevant fragments → generate answer. Each query is a fresh start, knowledge is rediscovered every time.

LLM Wiki takes a completely different approach:

> The LLM **incrementally builds and maintains a persistent wiki** — a set of structured, interlinked Markdown files sitting between you and your raw materials.

When you add new material, the LLM doesn't just index it. It:

- Reads and extracts key information
- Updates existing entity/concept pages
- Marks contradictions between new information and old viewpoints
- Updates indexes and logs

The result: **knowledge is compiled once, then kept continuously up-to-date**. Your wiki becomes an asset that grows through "compound interest."

---

## Three-Layer Architecture

```
raw/               → Immutable raw materials (articles, PDFs, clips)
wiki/              → LLM-generated Markdown files with [[bidirectional links]]
CLAUDE.md          → "Constitution," telling the LLM how to work
```

- **raw/** – Read-only. Your true sources.
- **wiki/** – Fully owned by the LLM. You read it; the LLM writes it.
- **CLAUDE.md** – An instruction file you co-evolve with the LLM.

---

## Three Core Operations

### 1. Ingest

You say: `ingest raw/articles/transformer-paper.md`

The LLM automatically:
- Reads the article, extracts concepts (self-attention, positional encoding, etc.)
- Creates or updates `[[self-attention]]`, `[[transformer]]` and other pages
- Updates `wiki/index.md` (adds/updates entries)
- Appends to `wiki/log.md`
- If contradictions are found, adds a "Contradictions/To Resolve" section at the bottom of relevant pages

### 2. Query

You ask: `Based on the wiki, what is self-attention?`

The LLM:
- Reads `index.md` to find `[[self-attention]]`
- Reads that page and answers with citations
- Asks if this answer is valuable and whether it should be saved as a new wiki page (e.g., `wiki/answers/self-attention-explained.md`)

### 3. Lint – Health Check

You say: `lint`

The LLM scans the entire wiki and reports:
- **Orphaned pages** (no incoming links)
- **Stale information** (not updated in 30 days, and the original file has changed)
- **Contradictions** (two pages describe the same concept but differ in details)
- **Missing links** (plain text matches existing page titles but lacks `[[]]`)

Then you can ask the LLM to automatically fix most issues.

---

## Division of Labor Between You and the LLM

| Human | LLM |
|-------|-----|
| Curate materials | Read and extract |
| Ask good questions | Create/update pages |
| Think about meaning | Maintain indexes and logs |
| Evolve rules (CLAUDE.md) | Mark contradictions |
| | Execute health checks (lint) |

> People don't abandon wikis because of reading or thinking, but because of maintenance costs. LLMs never get bored. Maintenance cost → near zero.

---

## Copy-Paste-Ready Configuration: CLAUDE.md

Save the following content as `CLAUDE.md` in your project root (works with Claude Code, Codex, OpenCode, etc.). This is your wiki's "constitution."

```markdown
# LLM Wiki – Project Constitution

> You (Claude) are the automated maintainer of this knowledge base. All rules must be strictly followed.

## I. Directories and Permissions

- `raw/` – Raw materials (articles, PDFs, clips). **Read-only, absolutely no modifications.**
- `wiki/` – All generated markdown pages. You may **create, update, delete** (deletion requires user confirmation).
- `wiki/index.md` – Content index, automatically maintained by you.
- `wiki/log.md` – Append-only log, recording all operations.
- `raw/assets/` – Image attachments, can be read but not modified.

## II. Page Format Standards

### File Names
- Use lowercase English + hyphens, e.g., `attention-mechanism.md`
- Concept pages: `concept-name.md`; Entity pages: `person-name-work.md`; Source pages: `article-title.md`

### Page Content Structure
Each wiki page must contain the following parts (in order):

1. **YAML frontmatter** (optional but highly recommended):
   ```yaml
   ---
   type: concept          # Options: concept, entity, source, summary, answer
   tags: [tag1, tag2]
   created: YYYY-MM-DD
   updated: YYYY-MM-DD
   source: raw/articles/xxx.md   # If from a specific raw file
   ---
   ```

2. **Main content**: Use markdown, at least one level-1 heading.
3. **Bidirectional links**: Any mention of existing concepts in the wiki must use `[[concept-name]]`.
4. **Source citations**: Use `(source: [[page-name]])` or footnotes at the end of paragraphs.

### Example Page
```markdown
---
type: concept
tags: [llm, rag]
created: 2026-04-10
updated: 2026-04-10
---

# LLM Wiki Pattern

LLM Wiki is a [[knowledge-management]] pattern where the core idea is to let LLMs automatically maintain a wiki with [[bidirectional-links]].

Unlike traditional RAG, it doesn't rely on retrieval during each query but instead [[incrementally-updates]] the knowledge base.

Source: [[hello-llm-wiki]]
```

## III. Operation Workflow (Must execute in strict order)

### 3.1 Ingest

**Trigger word**: `ingest <file-path>` or `add new material <file-path>`

**Steps**:
1. Read the complete content of `<file-path>` (if it's an image, skip it and focus on text).
2. Extract key information: main concepts, entities, data, contradictions.
3. **Briefly confirm with user** (unless user says "auto mode"):
   - "This article's core concepts are X, Y, Z. I will create/update these pages in the wiki. Is that okay?"
4. For each key concept/entity:
   - If a corresponding page exists in `wiki/`, update it (supplement new information, update `updated` field).
   - If not, create a new page (following "page format standards").
5. Update `wiki/index.md`:
   - List each page by category (concept/entity/source/answer), format: `- [[page-name]] – one-sentence summary (updated on YYYY-MM-DD)`
   - If page exists, update summary and date; if new page, append.
6. Append to `wiki/log.md`:
   - Format: `## [YYYY-MM-DD HH:MM] ingest | filename | number of affected pages`
   - Example: `## [2026-04-10 14:30] ingest | hello-llm-wiki.md | 3 pages updated`
7. If new information contradicts existing pages:
   - Add a `## Contradictions/To Resolve` section at the bottom of relevant pages, detailing the contradiction and source.

### 3.2 Query

**Trigger word**: Any direct question, such as `What is X?` or `Explain Y based on the wiki`

**Steps**:
1. First read the full `wiki/index.md` (if wiki page count > 100, use search script or grep).
2. Based on index, find 3-5 potentially relevant pages.
3. Read the full text of these pages.
4. Generate answer, **must cite sources with `[[page-name]]` after each key assertion**.
5. If the answer involves new synthesized insights from multiple pages, ask user: "This insight is valuable, should it be saved as `wiki/answers/question-summary.md`?"
6. If user agrees, create answer page following page standards, and update index.md and log.md.

### 3.3 Lint

**Trigger word**: `lint` or `check wiki health`

**Steps**:
1. Scan all `.md` files under `wiki/` (except index.md and log.md).
2. Find:
   - **Orphaned pages**: No other pages link to it via `[[page-name]]`.
   - **Stale information**: Page `updated` field is older than 30 days, and the referenced `source` file has been updated (compare file modification times).
   - **Contradictions**: Two pages describe the same concept inconsistently (based on keyword matching, heuristic).
   - **Missing links**: Plain text vocabulary in pages that exactly matches other page titles but lacks `[[]]`.
3. Output report, format:
   ```markdown
   ## Lint Report (YYYY-MM-DD)
   - Orphaned: [[page1]], [[page2]] – suggest deletion or adding backlinks
   - Stale: [[page3]] – last updated 2026-03-01, corresponding raw file has changed
   - Contradictions: [[pageA]] and [[pageB]] have different definitions of "XX"
   - Suggested new pages: "YY" concept mentioned 5 times but has no independent page
   ```
4. Execute fixes after waiting for user instructions (only when user says "fix orphaned pages").

## IV. Index and Log Maintenance Details

### `wiki/index.md` Format
```markdown
# Wiki Index

## Concepts
- [[llm-wiki]] – LLM-automated knowledge base pattern (2026-04-10)

## Entities
- [[john-doe]] – Researcher, proposed XX theory (2026-04-09)

## Sources
- [[hello-llm-wiki]] – Example article (2026-04-10)

## Answers
- [[rag-vs-wiki]] – Comparison of RAG and LLM Wiki (2026-04-10)
```

**Rules**:
- Must synchronously update index.md when ingesting, creating new answer pages, or deleting pages.
- If a page is deleted, remove corresponding entry from index.

### `wiki/log.md` Format
- Append only, never modify historical lines.
- Each line strictly follows: `## [YYYY-MM-DD HH:MM] operation-type | brief-description`
- Operation types: `ingest`, `query`, `lint`, `manual-edit`, `delete`.
- Examples:
  ```
  ## [2026-04-10 10:00] ingest | hello-llm-wiki.md | created 1 page, updated 2 pages
  ## [2026-04-10 10:15] query | "What is LLM Wiki" | generated answer, saved as [[rag-vs-wiki]]
  ```

## V. Constraints and Tools

### Absolutely Forbidden
- Modifying any files under the `raw/` directory.
- Deleting any wiki page without explicit user confirmation.
- Modifying historical entries in `wiki/log.md`.

### Allowed Shell Commands
- `ls`, `cat`, `grep`, `find` (read-only)
- `git status`, `git diff` (view changes)
- `git add . && git commit -m "auto-update"` (**only when user enables auto-commit**, off by default)

### Image Handling
- If a page references images under `raw/assets/`, reference them with `![description](raw/assets/image-name)`.
- When user asks to "view image," you can describe image content in words (if binary data is visible), otherwise skip.

### Multi-Session Memory
- At the start of each conversation, first read the last 5 lines of `wiki/log.md` to understand recent operations.
- If user asks "what was done before," directly reference the log.

## VI. Example Conversations (for your reference)

**User**: ingest raw/articles/attention-is-all-you-need.md
**Claude**: Read the article. Core concepts: [[self-attention]], [[transformer]], [[positional-encoding]]. I will create these three pages and update [[attention-mechanism]]. Is that okay?
**User**: Yes
**Claude**: ✅ Done. Created 3 pages, updated 1 page. Log recorded, index updated.

**User**: Explain self-attention
**Claude**: According to the [[self-attention]] page, self-attention is a mechanism that allows each position in a sequence to directly calculate its relevance to other positions. The specific formula is... (source: [[self-attention]]). I also found a comparison table in the [[cross-attention]] page, do you want to see it?
**User**: Yes
**Claude**: [shows table] This comparison is useful, should it be saved as a new page?
**User**: Save as wiki/answers/self-vs-cross-attention.md
**Claude**: Created and updated index.
```

---

## How to Get Started (5-minute Quick Start)

### 1. Preparation
- [Obsidian](https://obsidian.md/) (free) – For browsing the wiki
- Claude Code: `npm install -g @anthropic/claude-code`
  (or use Cursor / VS Code + Continue)

### 2. Create Directory Structure
```bash
mkdir ~/my-llm-wiki
cd ~/my-llm-wiki
mkdir -p raw/articles raw/assets wiki
touch wiki/index.md wiki/log.md
```

### 3. Save Configuration File
Copy the complete content of `CLAUDE.md` above to `~/my-llm-wiki/CLAUDE.md`.

### 4. Open the Folder in Obsidian

### 5. First Ingest
- Find an article and save it as `raw/articles/my-first.md`
- In Claude Code, type: `ingest raw/articles/my-first.md`

Observe the wiki pages, index, and log automatically created by the LLM.

### 6. Browse and Query
- Click `[[...]]` links in Obsidian to browse the auto-generated wiki.
- Ask Claude: `Based on the wiki, what are the main points of this article?`

---

## Why This Pattern Works

1. **Maintenance cost → zero** – LLMs never tire of updating links, indexes, and logs.
2. **Knowledge compounding** – Each ingestion builds on the existing wiki, no need to re-retrieve.
3. **You retain control** – You choose materials, ask questions, evolve rules. The LLM handles execution.
4. **Excellent toolchain** – Obsidian provides graphs, backlinks, and plugins; Git provides version history.

> This idea traces back to Vannevar Bush's 1945 Memex — personal, accumulative, associative path knowledge base. What Bush couldn't solve back then was "who would maintain it." Today, LLMs perfectly fill this gap.

---

## Advanced Tips

- **Images**: Use Obsidian Web Clipper, then press `Ctrl+Shift+D` to download all images to `raw/assets/`. The LLM can reference them with relative paths.
- **Search**: When the wiki exceeds 200 pages, you can add a simple `grep -r "keyword" wiki/` script, or integrate `qmd` (local hybrid search engine).
- **Slides**: Use Marp format, the LLM can directly generate presentations from wiki content.
- **Dynamic tables**: Add YAML tags in frontmatter and use Obsidian's Dataview plugin to automatically generate tables.

You can modify `CLAUDE.md` at any time to adjust rules — it's a "living protocol" between you and the LLM.

---

**Start building your LLM Wiki today.**
You just curate and ask. AI handles everything else.

> Inspiration: Andrej Karpathy's [LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). This article concretizes that abstract idea into a copy-paste-ready implementation.
