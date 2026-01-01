<!--
---
title: "Tagging Strategy"
description: "Controlled vocabulary for document classification and RAG retrieval in ai-models-wiki"
author: "VintageDon - https://github.com/vintagedon"
ai_contributor: "Claude Opus 4.5 (Anthropic)"
date: "2025-12-31"
version: "1.0"
phase: [phase-01, phase-02, phase-03]
tags:
  - domain: documentation
  - type: specification
related_documents:
  - "[Interior README Template](interior-readme-template.md)"
  - "[General KB Template](general-kb-template.md)"
---
-->

# Tagging Strategy

## 1. Purpose

This document defines the controlled tag vocabulary for all documentation in ai-models-wiki, enabling consistent classification for human navigation and RAG system retrieval.

---

## 2. Scope

Covers all tag categories, valid values, and usage guidance. Does not cover front-matter field structure—see individual templates for field requirements.

---

## 3. Tag Categories

### Phase Tags

Development phases. Documents may belong to multiple phases.

| Tag | Description |
|-----|-------------|
| `phase-01` | Ideation and setup (architecture, scaffolding) |
| `phase-02` | Site development (Astro, components, routing) |
| `phase-03` | Analytics and comparison (charts, tools) |
| `phase-04` | Production (deployment, CI/CD, monitoring) |

**Usage**: Tag with all phases a document supports.

---

### Domain Tags

Primary functional area. Usually one per document.

| Tag | Description |
|-----|-------------|
| `model-cards` | Model card data, schema, YAML processing |
| `site` | Astro framework, pages, components |
| `analytics` | Charts, visualizations, aggregate data |
| `comparison` | Side-by-side comparison tools |
| `documentation` | Methodology, specifications, standards |
| `infrastructure` | Azure Static Web Apps, CI/CD, hosting |

**Usage**: Choose the primary domain.

---

### Type Tags

Document purpose and structure.

| Tag | Description |
|-----|-------------|
| `methodology` | How we do something |
| `reference` | Lookup information (schema, data dictionary) |
| `guide` | Step-by-step procedures |
| `decision-record` | Why we chose X over Y |
| `specification` | Formal requirements |
| `source-code` | Code files and scripts |
| `configuration` | Config files, parameters |

**Usage**: One type per document.

---

### Tech Tags

Technologies and frameworks used.

| Tag | Description |
|-----|-------------|
| `astro` | Astro static site framework |
| `typescript` | TypeScript code |
| `tailwind` | Tailwind CSS styling |
| `chartjs` | Chart.js visualizations |
| `recharts` | Recharts React components |
| `yaml` | YAML data processing |
| `azure` | Azure Static Web Apps |

**Usage**: Tag when the document is specific to that technology.

---

### Model Card Schema Tags

For model card data documentation.

| Tag | Description |
|-----|-------------|
| `schema-identity` | Model identity section (name, vendor, version) |
| `schema-technical` | Technical specifications section |
| `schema-capabilities` | Capabilities and limitations section |
| `schema-trustworthiness` | NIST AI RMF trustworthiness assessment |
| `schema-rmf` | RMF function mapping (Govern/Map/Measure/Manage) |

**Usage**: Tag model card documentation with relevant schema sections.

---

### Trustworthiness Characteristic Tags

NIST AI RMF trustworthiness characteristics.

| Tag | Description |
|-----|-------------|
| `trust-valid-reliable` | Valid and reliable characteristic |
| `trust-safe` | Safe characteristic |
| `trust-secure-resilient` | Secure and resilient characteristic |
| `trust-accountable-transparent` | Accountable and transparent characteristic |
| `trust-explainable-interpretable` | Explainable and interpretable characteristic |
| `trust-privacy-enhanced` | Privacy enhanced characteristic |
| `trust-fair-bias-managed` | Fair with harmful bias managed characteristic |

**Usage**: Tag documents focused on specific trustworthiness characteristics.

---

## 4. References

| Reference | Link |
|-----------|------|
| Main README | [../../README.md](../../README.md) |
| Interior README Template | [interior-readme-template.md](interior-readme-template.md) |
| General KB Template | [general-kb-template.md](general-kb-template.md) |
| NIST AI RMF | [https://www.nist.gov/itl/ai-risk-management-framework](https://www.nist.gov/itl/ai-risk-management-framework) |

---
