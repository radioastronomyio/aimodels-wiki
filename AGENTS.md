# AGENTS.md

Entry point for AI coding agents working on this repository.

## Quick Start

1. **Load Memory Bank:** Read files in `.kilocode/rules/memory-bank/` for project context
   - `brief.md` — What is this project?
   - `context.md` — Current state and next steps
   - `architecture.md` — How it's built
   - `tech.md` — Technology stack and setup

2. **Understand the Structure:**
   ```
   ai-models-wiki/
   ├── model-cards/          # 162 YAML source files (the data)
   ├── site/                  # Astro project (the presentation)
   ├── docs/                  # Documentation and standards
   └── work-logs/             # Development phases
   ```

3. **Follow Documentation Standards:** Templates in `docs/documentation-standards/`

## Project Identity

**Domain:** aimodels.wiki  
**Repository:** https://github.com/radioastronomyio/aimodels-wiki  
**Purpose:** Static site presenting AI model cards with NIST AI RMF trustworthiness characteristics

## Current Phase

Check `.kilocode/rules/memory-bank/context.md` for current phase and next steps.

## Key Files

| Path | Purpose |
|------|---------|
| `model-cards/*.yaml` | Source model card data (162 files) |
| `site/` | Astro static site project |
| `site/src/pages/` | Route pages |
| `site/src/components/` | Reusable components |
| `docs/documentation-standards/` | Templates for all documentation |
| `work-logs/` | Phase-based development logs |

## Conventions

- **Documentation:** Use templates from `docs/documentation-standards/`
- **Commits:** Conventional commits (`feat:`, `fix:`, `docs:`)
- **Code Style:** TypeScript with strict mode, Prettier formatting
- **YAML:** Validate syntax before committing model cards

## Before You Start

1. Read the memory bank files (especially `context.md`)
2. Check `work-logs/` for what's been done
3. Follow existing patterns in the codebase
4. Update `context.md` when session ends

## Session End Checklist

- [ ] Updated `.kilocode/rules/memory-bank/context.md` with accomplishments
- [ ] Updated next steps for future sessions
- [ ] Documented any decisions made
- [ ] Committed changes with descriptive message
