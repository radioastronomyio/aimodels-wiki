<!--
---
title: "Phase 01: Ideation and Setup"
description: "Project conception, architecture decisions, and repository scaffolding for aimodels.wiki"
author: "VintageDon - https://github.com/vintagedon"
ai_contributor: "Claude Opus 4.5, Gemini Deep Research"
date: "2025-12-31"
version: "1.1"
phase: phase-01
tags:
  - domain: site
  - type: methodology
  - tech: astro, tailwind, azure, chartjs
related_documents:
  - "[Main README](../../README.md)"
  - "[Documentation Standards](../../docs/documentation-standards/README.md)"
  - "[GDR Visualization Research](../../.internal-files/gdr-ai-model-card-visualization.md)"
---
-->

# Phase 01: Ideation and Setup

> Compiled from: Multiple sessions | December 2025  
> Status: Complete  
> Key Outcome: Architecture decisions, technology stack selection, visualization framework, and repository scaffolding for a NIST AI RMF-aligned model card wiki

---

## 1. Objective

Establish the foundation for aimodels.wiki, a static site presenting AI model cards with NIST AI RMF trustworthiness characteristics, analytics dashboards, and comparison tools. This phase reviewed existing model card data, analyzed competitive landscape, made key architecture decisions, and conducted deep research on visualization approaches.

---

## 2. Context

### The Problem

The AI model ecosystem is flooded with benchmark-centric comparison sites (Artificial Analysis, HuggingFace Open LLM Leaderboard) and vendor-specific model cards (Google Model Cards, HuggingFace repo cards). What's missing is a cross-vendor aggregation of model cards emphasizing governance, risk management, and trustworthiness data aligned with NIST AI RMF.

Practitioners evaluating models for enterprise deployment need more than benchmark scores — they need to understand safety posture, bias mitigation approaches, transparency levels, and intended use boundaries. This information exists but is scattered across vendor documentation, system cards, and research papers.

### The Opportunity

The repository already contains 162 YAML model cards following a schema derived from NIST AI RMF. Each card includes:

- Model identity (vendor, family, version, license)
- Technical specifications (architecture, parameters, context window)
- Capabilities and limitations (vendor-claimed strengths, known failure modes)
- Trustworthiness assessment (7 NIST characteristics with vendor claims and evidence)
- RMF function mapping (Govern/Map/Measure/Manage considerations)
- Evaluation guidance (recommended tests, comparison considerations)

No existing site aggregates this level of detail across vendors. The gap is clear: governance-focused model documentation as a searchable, browsable wiki.

### Target Audience

- AI practitioners evaluating models for deployment
- Governance and compliance professionals assessing AI risks
- Researchers comparing model documentation practices
- Policy professionals seeking transparency examples

---

## 3. Competitive Analysis

### Benchmark-Centric Sites

| Site | Focus | Gap for Our Use Case |
|------|-------|----------------------|
| Artificial Analysis | Pricing, latency, throughput | No governance/trustworthiness data |
| HuggingFace Open LLM Leaderboard | Benchmark scores | No risk management context |
| llm-stats.com | Aggregate stats | Performance-only |

### Model Card Resources

| Resource | Focus | Gap for Our Use Case |
|----------|-------|----------------------|
| Google Model Cards | Google's own models | Vendor-specific |
| HuggingFace Model Cards | Per-repo documentation | Inconsistent structure across vendors |
| OECD.AI Catalog | Policy frameworks | Tools/frameworks, not model data |

### The Gap

No site provides cross-vendor model cards with NIST AI RMF trustworthiness characteristics in a searchable, browsable format. This is the underserved niche.

---

## 4. Key Architecture Decisions

### Decision 1: Pure Static Site

**Decision:** Build a fully static site with client-side filtering rather than a database-backed API.

**Rationale:** With 162 models and limited growth trajectory (95% completeness bar for inclusion), client-side JSON filtering is simpler and sufficient. CosmosDB adds complexity without proportional benefit.

**Implications:** Pre-compute all JSON at build time. Filtering, search, and comparison all run in the browser.

### Decision 2: Astro Framework

**Decision:** Use Astro as the static site framework.

**Rationale:**

- Content-first design aligns with model card wiki use case
- Island architecture allows interactive React components where needed (charts, comparison tools)
- Well-supported by AI coding assistants (Claude Code, Cursor)
- Static output deploys cleanly to Azure Static Web Apps

**Alternatives Considered:**

- Next.js: Heavier, SSR complexity not needed
- Hugo: Pure static, but limited interactivity
- SvelteKit: Good option, but Astro's island model better fits mixed static/interactive needs

### Decision 3: Tailwind CSS

**Decision:** Use Tailwind CSS for styling.

**Rationale:** Fast iteration, AI-friendly (well-represented in training data), utility-first approach reduces CSS complexity.

### Decision 4: Chart.js for Visualizations

**Decision:** Use Chart.js with plugins for analytics dashboards.

**Rationale:** Well-documented, AI assistants handle it well, extensible via plugins for specialized chart types (matrix, treemap).

### Decision 5: Azure Static Web Apps

**Decision:** Deploy to Azure Static Web Apps free tier.

**Rationale:** Generous free tier, GitHub CI/CD integration, aligns with existing Azure familiarity.

### Decision 6: Pagefind for Search

**Decision:** Use Pagefind for client-side faceted search.

**Rationale:** Binary-based static search, indexes rendered HTML, supports complex filtering without backend.

---

## 5. Visualization Research (GDR)

A Gemini Deep Research session using Negative Space Bounding methodology produced a comprehensive visualization framework. Full output saved to `.internal-files/gdr-ai-model-card-visualization.md`.

### Core Concept: "Trustboard" Not "Leaderboard"

The research reframes the site from "ranking" to "mapping" — users assess the *shape* of a model's risk profile rather than its position in a hierarchy. This differentiation is critical for governance audiences.

### Visualization Rankings

| Rank | Visualization | Schema Mapping | Purpose |
|------|---------------|----------------|---------|
| 1 | **Nightingale Rose Chart** | trustworthiness_assessment (7 characteristics) | "Trust Fingerprint" — area-based comparison without radar chart distortion |
| 2 | **RMF Process Heatmap** | rmf_function_mapping (Govern/Map/Measure/Manage) | "Governance Contribution Graph" showing process coverage density |
| 3 | **Reality Gap Diff Table** | vendor_claims vs public_evidence | Side-by-side verification with keyword divergence highlighting |
| 4 | **Modality Intersect Matrix** | technical_specifications.modalities | Bubble matrix for input/output capability discovery |
| 5 | **Power vs Trust Scatter** | parameter_count vs trust_score | Ecosystem landscape correlating size with governance maturity |
| 6 | **Timeline with Policy Overlays** | release_date | Historical context with regulatory milestone annotations |

### Key Visualization Decisions

**Nightingale Rose over Radar Chart:** Radar charts have perceptual distortion — area changes based on axis ordering. Rose charts use equal-angle segments where radius represents magnitude, ensuring honest visual comparison.

**Harvey Balls for Status:** Three-state indicators (Full = Verified, Half = Self-Reported, Empty = Unverified) provide instant visual vocabulary for evidence quality.

**"Cognitive Diff" Pattern:** Client-side keyword highlighting for claims vs. evidence comparison. Highlight "positive signal" words (audit, external) in green, "negative signal" words (internal-only, limited) in yellow.

### Comparison Tool Specification

- **Persisted Basket:** localStorage + URL query parameters (`/compare?ids=a,b,c`) for shareable comparisons
- **Sparse Data Handling:** Gray hatch pattern with "Not Disclosed" text — missing data is a signal, not a bug
- **Mobile Pivot View:** Interleaved list with primary model pinned, comparison target in dropdown

### Discovery UX: Filter-First

Professional audience hunting for solutions, not browsing. Faceted search via Pagefind with "Intent Button" accelerators:
- "I need a model for Healthcare applications" → pre-configured filters
- "Show me models that run on Consumer Hardware" → Params<14B, OSI license

### Model Card Page Layout

| Zone | Content | Rationale |
|------|---------|-----------|
| **Trust Header** (above fold) | Rose chart, traffic light badges, identity | Immediate risk profile |
| **Evidence Locker** (tabs) | 7 NIST characteristics with diff tables | Jump to specific audit focus |
| **Technical Sidebar** | Specs, compute, resources | Reference data, doesn't block governance flow |
| **Process Footer** | RMF heatmap, accordions | Deep audit trail for compliance |

### Anti-Patterns to Avoid

1. **No single aggregated "Safety Score"** — hides critical failure modes
2. **No gamified leaderboards** — invites gaming, encourages boilerplate
3. **No 3D charts** — distortion without information gain
4. **No force-directed network graphs** — unusable on mobile, accessibility nightmare
5. **No infinite scroll** — governance users need to "finish" a search

### Implementation Stack (from GDR)

| Component | Technology |
|-----------|------------|
| Rose Chart | Chart.js `polarArea` with fixed scale (0-3) |
| RMF Heatmap | `chartjs-chart-matrix` plugin |
| Ecosystem Treemap | `chartjs-chart-treemap` plugin |
| Search | Pagefind with `data-pagefind-filter` attributes |
| State Management | Nano Stores (lightweight) |
| Icons | Phosphor Icons or Heroicons |

---

## 6. Site Architecture

### Route Structure

```
/                       → Landing with aggregate analytics dashboards
/models                 → Filterable card grid (vendor, type, modality, license)
/models/[slug]          → Individual model card page
/compare                → Side-by-side comparison tool (2-4 models)
/analytics              → Deep-dive charts (timeline, capability matrices, trustworthiness)
/about                  → Schema explanation, methodology, NIST RMF context
```

### Data Flow

```
model-cards/*.yaml
    ↓ (Astro Content Collections + Zod validation)
Normalized data + computed trust scores
    ↓ (Build-time generation)
Static HTML pages + analytics.json + chunked model JSON
    ↓ (Azure Static Web Apps CDN)
https://aimodels.wiki
    ↓ (Client browser)
Pagefind search + Chart.js islands + comparison state
```

---

## 7. Repository Structure

```
ai-models-wiki/
├── .internal-files/              # GDR outputs, reference docs (gitignored)
│   └── gdr-ai-model-card-visualization.md
├── .kilocode/                    # Agent configuration
│   └── rules/memory-bank/        # Persistent context (7 files)
├── assets/                       # Repository-level assets
├── docs/                         # Documentation
│   ├── documentation-standards/  # Templates (8 files)
│   └── data-science-infrastructure.md
├── model-cards/                  # Source YAML (162 files)
├── scratch/                      # Working files (gitignored)
├── site/                         # Astro project
│   ├── src/
│   ├── public/
│   └── package.json
├── work-logs/                    # Milestone development
│   └── 01-ideation-and-setup/    # This phase
├── AGENTS.md                     # AI agent entry point
├── LICENSE
└── README.md
```

---

## 8. Model Card Schema Summary

The existing YAML schema (schema_version: "1.0.0") covers:

| Section | Contents |
|---------|----------|
| `model_identity` | name, vendor, family, version, release_date, type, license, deprecation_status |
| `technical_specifications` | architecture, modalities, performance_characteristics |
| `capabilities` | vendor_claimed_strengths, benchmark_performance, special_capabilities, known_limitations |
| `training_information` | training_data_description, training_methodology, data_privacy_considerations |
| `intended_use` | vendor_intended_use, suitable_domains, out_of_scope_use |
| `trustworthiness_assessment` | 7 NIST characteristics (valid_and_reliable, safe, secure_and_resilient, accountable_and_transparent, explainable_and_interpretable, privacy_enhanced, fair_with_harmful_bias_managed) |
| `evaluation_guidance` | recommended_tests, key_evaluation_questions, comparison_considerations |
| `rmf_function_mapping` | govern, map (context_considerations, risk_categories), measure, manage |
| `references` | vendor_documentation, benchmarks, third_party_evaluations, news_coverage |
| `metadata` | card_version, card_author, card_creation_date, last_updated, completeness_assessment, change_log |

---

## 9. Technology Stack

| Category | Technology | Purpose |
|----------|------------|---------|
| Framework | Astro | Static site generation with islands |
| Styling | Tailwind CSS | Utility-first CSS |
| Charts | Chart.js + plugins | Rose charts, heatmaps, treemaps, scatter |
| Search | Pagefind | Client-side faceted search |
| Interactivity | React (islands) | Comparison tools, filters |
| State | Nano Stores | Lightweight reactive state |
| Language | TypeScript | Type-safe scripting |
| Validation | Zod | Schema validation at build time |
| Hosting | Azure Static Web Apps | Free tier, GitHub CI/CD |

---

## 10. Implementation Phases

| Phase | Name | Status | Description |
|-------|------|--------|-------------|
| 01 | Ideation and Setup | ✅ Complete | Architecture, scaffolding, visualization research |
| 02 | Site Development | ⬜ Next | Astro project, components, routing |
| 03 | Analytics and Comparison | ⬜ Planned | Charts, comparison tools |
| 04 | Production Deployment | ⬜ Planned | Azure Static Web Apps, CI/CD |

---

## 11. Artifacts Produced

| Artifact | Purpose | Location |
|----------|---------|----------|
| Primary README | Project overview, architecture | Repository root |
| AGENTS.md | AI agent entry point | Repository root |
| Interior READMEs | Directory navigation | docs/, work-logs/, site/, model-cards/ |
| Documentation standards | Templates and tagging (8 files) | docs/documentation-standards/ |
| Memory bank | Persistent AI context (7 files) | .kilocode/rules/memory-bank/ |
| GDR visualization research | Comprehensive viz framework | .internal-files/gdr-ai-model-card-visualization.md |
| Phase worklog | This document | work-logs/01-ideation-and-setup/ |

---

## 12. Lessons Learned

| Challenge | Resolution |
|-----------|------------|
| CosmosDB vs static | Simplified to static JSON; 162 models don't need a database |
| Framework selection | Astro wins for content-first + islands; AI assistants handle it well |
| Competitive gap | NIST RMF trustworthiness focus is genuinely underserved |
| Radar chart limitations | GDR research identified Nightingale Rose as superior for governance data |
| Search approach | Pagefind solves static-site faceted search without backend |

**Key Insight:** The "Trustboard not Leaderboard" framing from GDR research crystallizes the differentiation. Most comparison sites rank models by capability; this site maps models by risk profile. The visualization choices (Rose chart, heatmaps, diff tables) directly support this governance-first approach.

---

## 13. Next Phase

**Enables:** Phase 02 (Site Development) can now scaffold the Astro project with clear visualization specifications.

**Dependencies resolved:**

- Architecture decisions established
- Technology stack selected
- Visualization framework researched and documented
- Repository structure complete
- Memory bank populated for AI agents

**Open items for Phase 02:**

- Initialize Astro project in `site/` directory
- Configure Tailwind CSS and TypeScript
- Create Content Collection for model cards with Zod schema
- Build model card page template with Trust Header layout
- Implement basic routing (`/`, `/models`, `/models/[slug]`)
- Set up Pagefind indexing

---

## 14. Provenance

| Item | Value |
|------|-------|
| Repository URL | https://github.com/radioastronomyio/aimodels-wiki |
| Domain | aimodels.wiki |
| Model cards | 162 YAML files |
| Schema version | 1.0.0 |
| Hosting | Azure Static Web Apps (free tier) |
| GDR Source | Gemini Deep Research with NSB methodology |

---

Next: [Phase 02: Site Development](../02-site-development/README.md)
