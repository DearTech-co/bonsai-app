# Bonsai Marketplace - Context OS Navigation Guide

**Purpose:** Single source of truth for building the Bonsai marketplace application for South African enthusiasts.

---

## What Is This?

This is the **Context Operating System** for the Bonsai marketplace project:
- All product decisions, requirements, and architecture live here
- AI compounds knowledge across sessions
- Front-end and back-end repos reference this for context

**Philosophy:** This repo is the product brain. Code repos are execution.

---

## Directory Structure

```
bonsai-app/
├── CLAUDE.md                   # ← YOU ARE HERE (start every session here)
├── knowledge_base/             # Layer 1: Atomic Knowledge
│   ├── product/                # Features, user stories, UX decisions
│   ├── technical/              # Architecture, integrations, tech decisions
│   ├── business/               # Market, competitors, pricing, legal
│   └── user-research/          # Personas, feedback, user insights
├── 00_foundation/              # Layer 2: Operational Docs
│   ├── vision/                 # Product vision, roadmap
│   ├── requirements/           # Feature specs, PRDs
│   ├── architecture/           # System design, API contracts
│   └── _synthesis/             # Summary documents
├── _system/
│   └── knowledge_graph/
│       ├── taxonomy.yaml       # Blessed tags for this domain
│       └── ontology.yaml       # How concepts relate
└── raw-context/                # Unprocessed notes, transcripts, research
```

---

## Project Overview

### The Application

**Bonsai** (working name) - A marketplace for Bonsai enthusiasts in South Africa

### Core Features (MVP)

1. **Marketplace**
   - Individual sellers list collections
   - Business sellers (nurseries, garden shops)
   - Buyer/seller dual-sided experience

2. **Trust System**
   - User ratings for buyers AND sellers
   - Overall trust score

3. **Community Forum**
   - Q&A for expert advice
   - Moderation system

4. **Media**
   - Photo uploads
   - Carousel for multiple images

### Technical Repos (External)

- `bonsai-frontend/` - Front-end application (TBD stack)
- `bonsai-backend/` - Back-end API (TBD stack)

---

## How to Use This System

### Starting a Session

Always start with: "Read CLAUDE.md for context"

### Finding Information

1. **Product decisions** → `00_foundation/requirements/`
2. **Technical architecture** → `00_foundation/architecture/`
3. **Specific features** → `knowledge_base/product/`
4. **Tech choices** → `knowledge_base/technical/`

### Adding Information

Process raw content (meeting notes, research, decisions):
"Process [file] into my knowledge base"

Or create directly:
"Add a knowledge node about [topic]"

### Checking Health

Run: `/graph-health`

---

## Current Status

**Phase:** Foundation / Requirements Gathering
**Next Milestone:** Define MVP scope and tech stack

---

## Quality Standards

**Every concept should have:**
- Structured frontmatter (tags, status, relationships)
- Links to related concepts
- Clear decision rationale (why, not just what)

**Decision format:**
- [DECIDED: rationale] - Final decision made
- [PROPOSED: reasoning] - Under consideration
- [OPEN] - Needs discussion

---

**Created:** 2026-01-12
**Status:** Active
