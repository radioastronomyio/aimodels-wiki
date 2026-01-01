<!--
---
title: "AI Models Wiki"
description: "NIST AI RMF-aligned model card wiki with analytics and comparison tools"
author: "VintageDon"
date: "2025-12-31"
version: "1.0"
status: "Active"
tags:
  - type: project-root
  - domain: [model-cards, site, analytics]
  - tech: [astro, typescript, tailwind, azure]
related_documents:
  - "[NIST AI RMF](https://www.nist.gov/itl/ai-risk-management-framework)"
  - "[RadioAstronomy.io](https://github.com/radioastronomyio)"
---
-->

# 🤖 AI Models Wiki

[![Astro](https://img.shields.io/badge/Framework-Astro-FF5D01?logo=astro)](https://astro.build/)
[![Tailwind CSS](https://img.shields.io/badge/Styling-Tailwind%20CSS-06B6D4?logo=tailwindcss)](https://tailwindcss.com/)
[![TypeScript](https://img.shields.io/badge/Language-TypeScript-3178C6?logo=typescript)](https://www.typescriptlang.org/)
[![Azure Static Web Apps](https://img.shields.io/badge/Hosting-Azure%20Static%20Web%20Apps-0078D4?logo=microsoftazure)](https://azure.microsoft.com/en-us/products/app-service/static)
[![License](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

> Cross-vendor AI model cards with NIST AI RMF trustworthiness characteristics, analytics dashboards, and comparison tools.

This repository powers [aimodels.wiki](https://aimodels.wiki), a static site presenting structured model card data for 160+ AI models. Unlike benchmark-focused comparison sites, this wiki emphasizes governance, risk management, and trustworthiness — helping practitioners evaluate models for enterprise deployment.

---

## 🔭 Background

The AI model ecosystem has abundant benchmark comparison sites (Artificial Analysis, HuggingFace Leaderboard) but limited cross-vendor aggregation of governance and trustworthiness data. Model cards exist, but they're scattered across vendor documentation, system cards, and research papers.

This wiki aggregates model cards following a schema derived from [NIST AI RMF](https://www.nist.gov/itl/ai-risk-management-framework) and [NIST AI 600-1](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf) (Generative AI Profile). Each card covers:

- Model Identity — vendor, family, version, license, deprecation status
- Technical Specifications — architecture, parameters, context window, modalities
- Capabilities & Limitations — vendor claims, known failure modes, unsuitable use cases
- Trustworthiness Assessment — 7 NIST characteristics with vendor claims and public evidence
- RMF Function Mapping — Govern/Map/Measure/Manage considerations for practitioners
- Evaluation Guidance — recommended tests, comparison considerations

---

## 🎯 Target Audience

| Audience | Use Case |
|----------|----------|
| AI Practitioners | Evaluate models for deployment beyond benchmarks |
| Governance/Compliance | Assess AI risks, document due diligence |
| Researchers | Compare model documentation practices across vendors |
| Policy Professionals | Find transparency and disclosure examples |

---

## 🏗️ Architecture

### Static Site with Client-Side Interactivity

```
model-cards/*.yaml  →  Build-time parsing  →  Static JSON  →  Astro pages + React islands
```

- 162 model cards in structured YAML
- Astro static site with island architecture for interactive components
- Client-side filtering — no database backend
- Azure Static Web Apps deployment via GitHub Actions

### Site Structure

| Route | Content |
|-------|---------|
| `/` | Landing with aggregate analytics dashboards |
| `/models` | Filterable card grid (vendor, type, modality, license) |
| `/models/[slug]` | Individual model card page |
| `/compare` | Side-by-side comparison tool (2-4 models) |
| `/analytics` | Deep-dive charts (timelines, matrices, distributions) |
| `/about` | Schema explanation, methodology, NIST RMF context |

### Analytics Views

- Vendor distribution (treemap/bar)
- Modality matrix (input/output heatmap)
- Capability radar (tools/vision/reasoning support rates)
- Release timeline (scatter by date)
- License breakdown (commercial vs open-weight)
- Trustworthiness coverage (documentation completeness)

---

## 📋 Implementation Phases

| Phase | Name | Status | Description |
|-------|------|--------|-------------|
| 01 | [Ideation and Setup](work-logs/01-ideation-and-setup/README.md) | ✅ Complete | Architecture, scaffolding |
| 02 | Site Development | ⬜ Next | Astro project, components, routing |
| 03 | Analytics and Comparison | ⬜ Planned | Charts, comparison tools |
| 04 | Production Deployment | ⬜ Planned | Azure Static Web Apps, CI/CD |

---

## 📁 Repository Structure

```
ai-models-wiki/
├── .internal-files/              # Templates, reference docs (gitignored)
├── assets/                       # Repository-level assets
├── docs/                         # Documentation
│   ├── data-science-infrastructure.md
│   └── documentation-standards/  # Templates and tagging
├── model-cards/                  # Source YAML (162 files)
├── scratch/                      # Working files (gitignored)
├── site/                         # Astro project
│   ├── src/
│   ├── public/
│   └── package.json
├── work-logs/                    # Milestone development
├── LICENSE
└── README.md                     # This file
```

---

## 🔧 Technology Stack

| Category | Technology | Purpose |
|----------|------------|---------|
| Framework | Astro | Static site generation with islands |
| Styling | Tailwind CSS | Utility-first CSS |
| Charts | Chart.js | Analytics visualizations |
| Interactivity | React (islands) | Comparison tools, filters |
| Language | TypeScript | Type-safe scripting |
| Hosting | Azure Static Web Apps | Free tier, GitHub CI/CD |

---

## 📊 Model Card Schema

Each YAML model card follows schema version 1.0.0:

| Section | Contents |
|---------|----------|
| `model_identity` | name, vendor, family, version, release_date, type, license |
| `technical_specifications` | architecture, modalities, performance_characteristics |
| `capabilities` | vendor_claimed_strengths, known_limitations, special_capabilities |
| `trustworthiness_assessment` | 7 NIST characteristics with claims and evidence |
| `rmf_function_mapping` | govern, map, measure, manage considerations |
| `evaluation_guidance` | recommended_tests, comparison_considerations |
| `references` | vendor_documentation, benchmarks, third_party_evaluations |

### NIST Trustworthiness Characteristics

| Characteristic | Description |
|----------------|-------------|
| Valid and Reliable | Accuracy, consistency, reproducibility |
| Safe | Harm avoidance, safety measures |
| Secure and Resilient | Jailbreak resistance, prompt injection defense |
| Accountable and Transparent | Documentation quality, auditability |
| Explainable and Interpretable | Reasoning transparency, output understanding |
| Privacy Enhanced | Data handling, PII protection |
| Fair with Harmful Bias Managed | Bias mitigation, equity considerations |

---

## 🚀 Development

```bash
# Navigate to site directory
cd site

# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

---

## 📄 License

[MIT](LICENSE) © 2025 VintageDon

---

## 🙏 Acknowledgments

- [NIST](https://www.nist.gov/) — AI Risk Management Framework
- [RadioAstronomy.io](https://github.com/radioastronomyio) — Research organization
- Model vendors — for publishing system cards and documentation

---

Last Updated: December 31, 2025 | Current Phase: 01 Ideation and Setup Complete
