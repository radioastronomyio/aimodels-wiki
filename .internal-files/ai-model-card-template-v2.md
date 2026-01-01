# aimodels.wiki Model Card Schema v2.0.0

# Root Object: ModelSystemCard

ModelSystemCard:

# Metadata for the card itself

SchemaVersion: 2.0.0
CardLastUpdated: "YYYY-MM-DD"
CardAuthor: "Organization Name or Individual"

# ===================================================================

# Section 1: GOVERN - Accountability, Policy, and Risk Posture

# Aligns with the NIST AI RMF GOVERN function

# This section describes the organizational context and overarching risk management approach

# ===================================================================

Govern:
RiskManagementFramework:
FrameworkName: "e.g., OpenAI Preparedness Framework, Anthropic AI Safety Levels (ASL)"
FrameworkDescription: "A summary of the organization's internal AI risk management policy or
framework."
FrameworkURL: "URL to the public documentation of the framework."
RiskToleranceAndTiering:
SystemRiskTier: "e.g., Low-Risk, High-Risk (EU AI Act), ASL-3"
TieringRationale: "Justification for the assigned risk tier, based on the system's capabilities and
intended applications."
AccountabilityAndRoles:

- Role: "e.g., Model Development Team, Red Teaming Lead, Product Safety Committee"
Contact: "email or contact form"
Responsibilities: "Description of responsibilities related to the AI system's lifecycle."
ThirdPartyDependencies:
- ComponentType: "e.g., Base Model, Training Dataset, Cloud Provider, Evaluation Tool"
ComponentName: "Name of the third-party component"
Provider: "Name of the third-party provider"
RiskManagement: "How risks associated with this dependency are managed (e.g., contractual
obligations, independent audits)."

# ===================================================================

# Section 2: MAP - Context, Capabilities, and System Boundaries

# Aligns with the NIST AI RMF MAP function

# This section describes what the system is, what it does, and how it was built

# ===================================================================

Map:
SystemOverview:
SystemName: "e.g., GPT-5 System, Amazon Nova Family"
SystemVersion: "Version identifier"
SystemDate: "Date of release or last major update"
SystemType: "e.g., Generative Text, Text-to-Image, Multimodal"
SystemDescription: "A high-level summary of the AI system and its purpose."
IntendedUse:

- UseCase: "Description of a primary intended use case."
Stakeholders: "e.g., General Public, Software Developers, Medical Researchers"
Benefits: "Potential positive impacts or benefits of this use case."
OutOfScopeUse:
- UseCase: "Description of a use case that is explicitly out-of-scope or prohibited."
Rationale: "Reason for the exclusion (e.g., safety concerns, technical limitations)."
PolicyURL: "Link to relevant section of Acceptable Use Policy."
SystemArchitecture:
ArchitectureType: "e.g., Transformer, Mixture-of-Experts (MoE), Diffusion"
Components:
- ComponentName: "e.g., gpt-5-router, gpt-5-thinking-model"
ComponentType: "e.g., Router, Language Model, Safety Classifier"
Description: "Function of the component within the system."
Parameters:
Total: "Total number of parameters in the system."
Active: "Number of active parameters per inference (for MoE models)."
KnowledgeCutoffDate: "YYYY-MM-DD"
TrainingAndDevelopmentProcess:
PreTrainingData:
DataSources: "Description of sources (e.g., public web data, licensed corpora)."
DataCollectionPeriod: "Start and end dates of data collection."
FilteringAndProcessing: "Methods used for data cleaning, deduplication, and safety filtering."
FineTuningData:
- TuningType: "e.g., Supervised Fine-Tuning (SFT), Reinforcement Learning from Human Feedback
(RLHF), Constitutional AI"
DataSource: "Description of the data used for this tuning phase."
Purpose: "The goal of this tuning phase (e.g., instruction following, safety alignment)."
ComputationalResources:
HardwareUsed: "e.g., TPUs, GPUs"
EstimatedCarbonFootprint: "In tCO2eq, if available."

# ===================================================================

# Section 3: MEASURE - Empirical Evidence and Trustworthiness Evaluation

# Aligns with the NIST AI RMF MEASURE function, structured by Trustworthy AI Characteristics

# This section provides the evidence for claims about the system's performance and safety

# ===================================================================

Measure:
ValidAndReliable:

- Benchmark: "e.g., MMLU, GPQA, SWE-bench"
Score: "Achieved score"
Metric: "e.g., Accuracy, Pass@1"
DetailsAndCaveats: "Link to paper or leaderboard, description of evaluation setup."
Safe:
DisallowedContentAndHarmfulBehavior:
- Evaluation: "e.g., OpenAI Production Benchmarks, Internal Red Teaming"
HarmCategory: "e.g., Hate Speech, Illicit Advice, Self-Harm"
Metric: "e.g., not_unsafe rate, Refusal Rate"
Result: "Quantitative result"
Comparison: "Comparison to a predecessor model, if available."
GenerativeAISpecificRisks: # From NIST AI 600-1
- RiskType: "Confabulation (Hallucination)"
Evaluation: "e.g., LongFact, FActScore"
Metric: "e.g., Claim-level error rate"
Result: "Quantitative result, with and without browsing/RAG."
- RiskType: "InformationIntegrity"
Evaluation: "Methodology for assessing propensity to generate mis/disinformation."
Result: "Qualitative or quantitative findings."
DualUseAndCatastrophicRisk:
- RiskArea: "CBRN"
Evaluation: "e.g., Long-form biorisk questions, External audit by SecureBio"
CapabilityAssessed: "e.g., Uplift for novice actor in bioweaponization"
Result: "Summary of findings and assigned risk level."
- RiskArea: "Cybersecurity"
Evaluation: "e.g., Capture the Flag (CTF) Challenges, Cyber Range Simulation"
CapabilityAssessed: "e.g., Autonomous vulnerability discovery and exploitation"
Result: "Summary of findings and comparison to human expert baselines."
- RiskArea: "AutonomousReplicationAndImprovement"
Evaluation: "e.g., MLE-Bench, Internal AI R&D tasks"
CapabilityAssessed: "e.g., Ability to autonomously improve its own performance"
Result: "Summary of findings."
SecureAndResilient:
- ThreatType: "Jailbreak"
Evaluation: "e.g., StrongReject benchmark"
Metric: "not_unsafe rate"
Result: "Quantitative result across different harm categories."
- ThreatType: "PromptInjection"
Evaluation: "e.g., Browsing, Tool-calling, and Coding prompt injection benchmarks"
Metric: "Success Rate / Defense Rate"
Result: "Quantitative result."
FairWithHarmfulBiasManaged:
- Evaluation: "e.g., BBQ, Winogender"
BiasType: "e.g., Gender, Race, Religion"
Metric: "e.g., Accuracy on ambiguous vs. disambiguated questions"
Result: "Quantitative result and qualitative analysis of stereotypical outputs."
DisaggregatedPerformance:
- Task: "A specific downstream task"
DemographicAxis: "e.g., Gender, Dialect, Age"
Metric: "e.g., Accuracy, False Positive Rate"
Results: "Performance metrics broken down by demographic group."
ExplainableAndInterpretable:
ReasoningFaithfulness:
Evaluation: "Methodology for assessing if Chain-of-Thought reasoning is faithful to the model's
actual process."
Result: "Findings on the reliability of the model's explanations."
AlignmentAudits:
- AuditType: "e.g., Mechanistic Interpretability Analysis, Automated Behavioral Audits"
PhenomenonStudied: "e.g., Evaluation Awareness, Sycophancy, Deception"
Result: "Summary of findings from internal model inspection and simulation."
PrivacyEnhanced:
TrainingDataPrivacy:
Techniques: "e.g., PII filtering, data anonymization"
Evaluation: "Results of tests for PII leakage or data memorization."
OutputPrivacy:
Evaluation: "Assessment of the model's propensity to reveal sensitive information in its outputs."
Result: "Findings and mitigations."

# ===================================================================

# Section 4: MANAGE - Risk Mitigation, Monitoring, and Lifecycle Controls

# Aligns with the NIST AI RMF MANAGE function

# This section describes the actions taken to control risks

# ===================================================================

Manage:
RiskMitigationControls:

- ControlType: "e.g., Input Classifier, Output Filter, Safety-focused Reasoning Monitor"
Description: "How the control works to mitigate specific harms."
Effectiveness: "Data on the control's performance (e.g., precision/recall)."
MonitoringAndMaintenancePlan:
MonitoringStrategy: "Description of post-deployment monitoring for performance drift, new risks,
etc."
UpdateCadence: "Expected frequency of model updates or safety patches."
IncidentResponseAndRedress:
ReportingMechanism: "URL or contact for reporting harmful outputs or vulnerabilities."
RedressProcess: "Description of the process for appealing a system's decision or seeking redress
for harm."
AccessAndUseControls:
- ControlType: "e.g., Staged Release, API Access Tiers, Usage Policies"
Description: "Description of how access to the system or specific capabilities is managed."
SpecialPrograms: "e.g., OpenAI Trusted Access Program for high-risk capabilities."
ContentProvenance:
- Mechanism: "e.g., C2PA metadata, Visible Watermark"
Description: "How the system marks its outputs as AI-generated to support information integrity."
