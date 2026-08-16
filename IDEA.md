# Second Brain / Knowledge OS

## Concept

A local-first knowledge management system that continuously scans your documents and business apps (Slack, Gmail, etc.), provides a human-in-the-loop ingestion flow for categorization, stores everything in a central indexed knowledge base with RAG capabilities, and surfaces patterns like repetitive tasks and frequently asked questions.

## Core Features

### Phase 1: Local Document Scanning
- Scan local filesystem for docs (markdown, PDF, text, code comments)
- Auto-categorize by topic/project
- Human review flow: approve, reject, re-categorize, edit
- Central knowledge base with full-text + semantic search

### Phase 2: RAG & Query
- Embeddings-based retrieval (local via Ollama or cloud)
- Ask questions across your entire knowledge base
- Cross-reference related items automatically
- Generate summaries and reports

### Phase 3: Business App Connectors
- **Slack:** Scan channels/DMs, extract decisions, action items, knowledge
- **Gmail:** Scan threads, extract commitments, reference material
- **Calendar:** Meeting notes, recurring topics
- **GitHub:** Issues, PR discussions, decision records
- Continuous background scanning (not one-shot import)

### Phase 4: Pattern Detection
- Identify repetitive TODOs (things you keep putting off)
- Surface FAQs (questions you get asked repeatedly)
- Detect knowledge gaps (topics referenced but never documented)
- Track decisions and their rationale over time

## Example Usage

```bash
# Install
npm install -g brain

# Initialize with local docs
brain init --scan ~/Documents ~/Projects/*/docs

# Start ingestion review (TUI)
brain review           # interactive categorization of new items

# Query your brain
brain ask "What did we decide about the auth migration?"
brain ask "What tasks keep coming up that I haven't done?"

# Connect business apps
brain connect slack --token xoxb-...
brain connect gmail --credentials ./oauth.json

# Dashboard
brain status           # show stats, patterns, recent items
brain patterns         # show repetitive tasks, FAQs, gaps
```

## Architecture

```
┌──────────────────────────────────────────┐
│              User Interface               │
│         (CLI / TUI / Web dashboard)       │
└──────────────────┬───────────────────────┘
                   │
┌──────────────────┴───────────────────────┐
│            Orchestrator                    │
│   (scheduling, dedup, categorization)     │
└──────────────────┬───────────────────────┘
                   │
        ┌──────────┼──────────┐
        │          │          │
        ▼          ▼          ▼
┌──────────┐ ┌──────────┐ ┌──────────┐
│  Local   │ │  Slack   │ │  Gmail   │  ← Connectors
│  Files   │ │Connector │ │Connector │
└──────────┘ └──────────┘ └──────────┘
                   │
┌──────────────────┴───────────────────────┐
│           Knowledge Store                 │
│  (SQLite + Vector DB + Full-text index)   │
│  Central indices, categories, relations   │
└──────────────────┬───────────────────────┘
                   │
┌──────────────────┴───────────────────────┐
│          LLM Layer (Local/Cloud)          │
│  Embeddings, categorization, summaries,   │
│  pattern detection, Q&A                   │
└───────────────────────────────────────────┘
```

## Tech Stack

- TypeScript + Node.js
- Storage: SQLite (structured) + ChromaDB/LanceDB (vectors)
- LLM: Ollama (local default) + OpenAI/Anthropic
- Embeddings: Local (nomic-embed) or cloud
- TUI: Ink (React for terminals)
- Connectors: Slack API, Gmail API, GitHub API
- Scheduling: node-cron for background scanning

## Key Differentiators

1. **Continuous scanning** — Not one-shot import, ongoing background collection
2. **Human review flow** — You approve what goes into your brain (not fully autonomous)
3. **Pattern detection** — Surfaces repetitive tasks, FAQs, knowledge gaps
4. **Local-first** — Data never leaves your machine (Ollama + SQLite)
5. **Business app connectors** — Slack, Gmail, Calendar (not just files)
6. **Open source** — No vendor lock-in, no subscription

## Target Users

- Knowledge workers drowning in Slack/email/docs
- Developers who want to capture decisions and patterns
- Anyone building a personal knowledge management system
- Teams that want shared knowledge bases without expensive SaaS

## Effort Estimate

This is a **large project** (3-6 months for full vision):
- Phase 1 (local files + review): 2-3 weeks
- Phase 2 (RAG + query): 1-2 weeks  
- Phase 3 (connectors): 2-4 weeks per connector
- Phase 4 (patterns): 2-3 weeks

## Status

📋 **Idea** — Not yet implemented. Long-term project.
