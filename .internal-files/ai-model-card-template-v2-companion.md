# Model Card Schema v2.0.0 - Quick Reference Guide

## What Changed: v1.0.0 → v2.0.0

v1.0.0 Problem: Simple "nutrition labels" couldn't document modern AI systems—routers, multi-model architectures, advanced safety testing, governance frameworks.

v2.0.0 Solution: NIST AI RMF-aligned structure transforms model cards into dynamic risk management artifacts.

---

## Four-Section Architecture (NIST AI RMF Functions)

### 1. GOVERN - Who's Responsible & What Are The Rules?

Purpose: Organizational accountability & risk posture

Key Fields:

- `RiskManagementFramework`: Internal governance (e.g., OpenAI Preparedness Framework, Anthropic ASL)
- `RiskToleranceAndTiering`: Risk classification (High-Risk/EU AI Act, ASL-3, etc.)
- `AccountabilityAndRoles`: Who owns what (teams, contacts, responsibilities)
- `ThirdPartyDependencies`: Supply chain risks (base models, datasets, cloud providers)

Why It Matters: Makes organizational risk culture explicit—missing from v1.0.0 entirely.

---

### 2. MAP - What Is This System & What Could Go Wrong?

Purpose: System context, capabilities, boundaries

Key Fields:

- `SystemArchitecture`: Multi-component systems (routers, MoE, sub-models)
- `IntendedUse` / `OutOfScopeUse`: Explicit boundaries & prohibited applications
- `TrainingAndDevelopmentProcess`: Data sources, filtering, tuning phases (SFT, RLHF, Constitutional AI)

Why It Matters: Captures complex system interactions (e.g., GPT-5's router + reasoning model) that v1.0.0 couldn't handle.

---

### 3. MEASURE - What's The Evidence For Your Claims?

Purpose: Empirical validation of performance & safety

Organized by NIST Trustworthy AI Characteristics:

#### Valid & Reliable

- Standard benchmarks (MMLU, SWE-bench, etc.)

#### Safe

- Disallowed Content: Refusal rates for hate speech, violence, etc.
- Generative AI Risks (NIST AI 600-1):
  - Confabulation (hallucination): LongFact, FActScore
  - Information Integrity: Misinformation propensity
- Dual-Use & Catastrophic Risk:
  - CBRN: Bioweapon/chem weapon uplift tests
  - Cybersecurity: CTF challenges, autonomous exploitation
  - AI Self-Improvement: MLE-Bench, autonomous R&D

#### Secure & Resilient

- Jailbreak robustness (StrongReject)
- Prompt injection defense rates

#### Fair

- Bias benchmarks (BBQ, Winogender)
- Disaggregated performance by demographic

#### Explainable & Interpretable

- Reasoning faithfulness (CoT accuracy)
- Alignment audits: Deception, sycophancy, evaluation awareness

#### Privacy Enhanced

- PII filtering, memorization tests

Why It Matters: Systematizes frontier practices (external red teams, mechanistic interpretability) into standard structure.

---

### 4. MANAGE - What Have You Done About The Risks?

Purpose: Active risk mitigation & lifecycle controls

Key Fields:

- `RiskMitigationControls`: Technical safeguards (input filters, output classifiers, safety monitors)
- `MonitoringAndMaintenancePlan`: Post-deployment tracking, update cadence
- `IncidentResponseAndRedress`: Reporting mechanisms, appeal processes
- `AccessAndUseControls`: Staged releases, API tiers, Trusted Access Programs
- `ContentProvenance`: C2PA metadata, watermarking

Why It Matters: Documents proactive safeguards—shifts from passive reporting to active risk management.

---

## Critical Innovations

### System-Centric Documentation

Handles multi-component architectures:

- GPT-5's unified system (router + gpt-5-main + gpt-5-thinking)
- AWS Nova's teacher-student distillation hierarchy
- Claude's hybrid reasoning modes (standard vs. extended thinking)

### Current vs. Target Profiles

Operationalizes NIST RMF Profile concept:

- `currentValue`: Today's measured performance
- `targetValue`: Commitment to future improvement
- Transforms card from snapshot → living risk profile

### Regulatory Alignment

Maps directly to:

- EU AI Act technical documentation requirements (Annex IV)
- ISO/IEC 42001 AI management system standards
- NIST AI 600-1 Generative AI Risk Profile

---

## Key Disclosure Patterns From Frontier Models

| Pattern | Example | v2.0.0 Field |
|---------|---------|--------------|
| Internal governance frameworks | OpenAI Preparedness Framework, Anthropic ASL | `Govern.RiskManagementFramework` |
| Dual-use capability testing | CBRN uplift, cybersecurity CTF | `Measure.Safe.DualUseAndCatastrophicRisk` |
| Advanced alignment research | Evaluation awareness, mechanistic interpretability | `Measure.ExplainableAndInterpretable.AlignmentAudits` |
| System-level architecture | GPT-5 router + sub-models | `Map.SystemArchitecture.Components` |
| Operational safeguards | Trusted Access Programs, staged deployment | `Manage.AccessAndUseControls` |

---

## Implementation Considerations

### For Platform (aimodels.wiki)

1. Two-Tiered Display:
   - Summary Card (general audience): Name, version, developer, key risks
   - Full Risk Profile (technical audience): Complete v2.0.0 schema
2. Interactive Tools: Model comparison, field filtering, collapsible sections

### For Developers

1. Model Card Builder Tool: Guided step-by-step completion with examples
2. Field Guidance: Links to NIST AI RMF Playbook for each field
3. Progressive Disclosure: Start with required fields, expand to comprehensive documentation

### Schema Governance

- Public working group (developers, academia, civil society, standards bodies)
- Formal review cycle aligned with NIST AI RMF updates (2028)
- Version control for schema evolution

---

## Why This Matters Now

Regulatory Pressure: EU AI Act, emerging US frameworks require detailed technical documentation.

Market Differentiation: Comprehensive disclosure signals maturity, builds trust.

Risk Management Culture: Systematic framework for continuous improvement, not just release-time compliance.

Interoperability: NIST alignment enables cross-organizational comparison, benchmarking, regulatory mapping.

---

## Quick Decision Tree: Which Section For What?

- Organizational policy/who's accountable? → GOVERN
- System description/what it does/training details? → MAP
- Test results/benchmark scores/safety evals? → MEASURE
- Safeguards/monitoring/access controls? → MANAGE

---

## Schema Format

YAML-based, machine-parseable, human-readable. Designed for:

- Automated compliance checking
- Programmatic model comparison
- Integration with ML platforms (Hugging Face Hub, aimodels.wiki)

---

*This guide accompanies the full v2.0.0 schema specification. For field-by-field implementation guidance, see the complete technical report.*
