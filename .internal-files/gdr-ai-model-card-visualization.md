# Designing the AI Model Card Wiki: A Governance-First Visualization Framework

## 1\. Executive Summary: The Architecture of Trust

The contemporary artificial intelligence ecosystem is characterized by a "transparency paradox." While the volume of model releases has exploded—Hugging Face alone hosts over 750,000 models 1—the *utility* of the accompanying documentation for governance professionals has not kept pace. The dominant design paradigm in AI documentation remains the "Leaderboard," a gamified ranking system that reduces complex socio-technical systems to single-number performance metrics (e.g., MMLU scores, win rates).2 While effective for engineers seeking raw capability, this reductionist approach fails the Governance, Risk, and Compliance (GRC) practitioner who must assess safety, legality, and alignment with standards like the NIST AI Risk Management Framework (AI RMF).

This research report outlines a comprehensive visualization and user experience strategy for a static, governance-focused AI Model Card Wiki. Anchored by a dataset of 162 structured YAML model cards and constrained by a static site architecture (Astro \+ Tailwind CSS), this framework rejects the "Leaderboard" in favor of a "Trustboard." The core recommendation transforms the visualization metaphor from "ranking" to "mapping," enabling practitioners to assess the *shape* of a model's risk profile rather than just its performance velocity.

Our analysis, informed by the NIST AI RMF functions (Govern, Map, Measure, Manage) and best practices in qualitative data visualization, identifies the Polar Area (Nightingale Rose) Chart as the primary visualization champion for the seven NIST trustworthiness characteristics. Unlike radar charts, which suffer from perceptual distortion, the Rose chart allows for an equitable, area-based comparison of qualitative evidence levels. Furthermore, we propose a "Cognitive Diff" pattern for textual comparison, leveraging client-side logic to highlight divergences in vendor claims versus verified evidence, directly addressing the "Reality Gap" in current documentation.4

This report delivers a detailed roadmap for implementation, from the schema-to-visualization mapping to the specific Chart.js configurations required for a static environment. It serves as a blueprint for building a tool that does not merely list AI models, but actively facilitates their responsible governance.

## ---

2\. Visualization Recommendations (Ranked)

The selection of visualization types for the Model Card Wiki is driven by the specific nature of the data: categorical risk assessments, hierarchical RMF mappings, and qualitative evidence text. Standard quantitative charts (bars, lines) are insufficient for capturing the nuance of "Trustworthiness." The following ranked recommendations prioritize clarity, accessibility, and the ability to render deterministically in a static environment.

### Rank 1: The "Trustworthiness Rose" (Polar Area Chart)

Concept: The definitive "Trust Fingerprint" of a model.  
Schema Mapping: trustworthiness\_assessment (7 NIST Characteristics: Valid, Safe, Secure, Accountable, Explainable, Private, Fair).  
Data Type: Ordinal (Score 0-3 based on documentation maturity).  
Rationale:  
The standard Radar Chart (or Spider Plot) is frequently used for multivariate data but is mathematically flawed for this use case. As noted in data visualization literature, the area of a polygon in a radar chart is influenced by the arbitrary order of the axes.6 If two high-scoring axes are adjacent, the shape looks massive; if separated by a low-scoring axis, the area collapses. This creates false visual cues.  
The Polar Area Chart (Nightingale Rose) solves this by using equal-angle segments where the *radius* represents the magnitude. This ensures that a model with high scores in "Safety" and "Privacy" occupies the same visual area regardless of where those axes are placed on the circle. For governance professionals comparing the "volume of evidence" across models, this perceptual accuracy is non-negotiable.

Implementation Specification (Chart.js):

* Library: Chart.js polarArea type.  
* Scale: Fixed radial scale (min: 0, max: 3). *Crucial:* Do not let the chart auto-scale, or a model with uniformly low scores (all 1s) will look identical to a model with uniformly high scores (all 3s).  
* Color Coding: Use a semantic palette mapped to the characteristics (e.g., Red for Safety, Blue for Privacy, Green for Fairness) or a monochromatic scale indicating evidence density.  
* Interactivity: Hovering over a segment (e.g., "Fairness") triggers a tooltip or a React Island sidebar update that displays the specific vendor\_claims and assessment\_notes summary.

### Rank 2: The "RMF Process" Matrix (Interactive Heatmap)

Concept: A "Governance Contribution Graph" showing process coverage.  
Schema Mapping: rmf\_function\_mapping (Govern, Map, Measure, Manage).  
Data Type: Hierarchical / Categorical (Presence of mapping).  
Rationale:  
The NIST AI RMF is a process framework, not just a checklist. Users need to see where the effort has been applied. A model might be heavily "Measured" (technical benchmarks) but poorly "Governed" (no policies). A Matrix Heatmap visualizes this density instantly. It mirrors the familiar mental model of a GitHub contribution graph or a disk usage map, indicating "activity" rather than value.7  
Implementation Specification:

* Library: chartjs-chart-matrix plugin.  
* Axes:  
  * Y-Axis: The 4 Functions (Govern, Map, Measure, Manage).  
  * X-Axis: The Sub-Categories (e.g., 1.1, 1.2, 2.1...).  
* Cell Logic:  
  * *Empty (Gray):* No mapping found in schema.  
  * *Light Shade:* Mapped to high-level vendor\_claims.  
  * *Dark Shade:* Mapped to specific public\_evidence (URL present).  
* Interaction: Clicking a cell locks the "RMF Detail View" below the chart to that specific function, allowing the user to read the relevant YAML snippet.

### Rank 3: The "Reality Gap" Diff Table

Concept: Side-by-side verification of claims vs. reality.  
Schema Mapping: trustworthiness\_assessment \-\> (vendor\_claims vs. public\_evidence vs. assessment\_notes).  
Data Type: Unstructured Text / Boolean verification.  
Rationale:  
The most critical task for a risk assessor is validating vendor marketing. A standard table hides discrepancies in walls of text. We propose a "Diff" pattern adapted from code repositories (like GitHub PRs), but applied to semantic claims.4 This highlights the absence of evidence as a visual signal.  
Implementation Specification:

* Library: Pure HTML/CSS Grid with diff class utilities.  
* Visual Pattern:  
  * *Column 1 (Claim):* "Vendor states model is bias-free." (Neutral background).  
  * *Column 2 (Evidence):* "No external audit provided." (Yellow/Red background warning).  
  * *Column 3 (Notes):* "Assessment: Claims unsubstantiated."  
* Harvey Balls: Use SVG Harvey Balls 9 in the header of each row to summarize the status (Full \= Verified, Half \= Self-Reported, Empty \= Unverified).

### Rank 4: The Modality Intersect (Bubble Matrix)

Concept: Rapid discovery of input/output capabilities.  
Schema Mapping: technical\_specifications.modalities (Inputs, Outputs).  
Data Type: Categorical Sets.  
Rationale:  
As models become multimodal (e.g., GPT-4o), simple lists like "Text, Image" are insufficient. Users need to know combinations. Can it take Audio and output Code? A Bubble Matrix visualizes these intersections clearly.  
Implementation Specification:

* Library: Chart.js Bubble Chart.  
* Axes: X \= Input Modalities, Y \= Output Modalities.  
* Bubble Size: Number of models in the dataset matching this pair.  
* Interaction: Clicking a bubble (e.g., intersection of "Image Input" and "Text Output") acts as a filter, redirecting the user to a filtered list of those specific models.

### Rank 5: The "Power vs. Trust" Scatter Plot

Concept: Correlating resource cost with governance maturity.  
Schema Mapping: technical\_specifications.parameter\_count vs. trustworthiness\_assessment (Aggregate Score).  
Data Type: Quantitative vs. Ordinal.  
Rationale:  
A common question in governance is, "Does a larger model imply better safety mechanisms?" or "Are small models less transparent?" A scatter plot allows researchers to visualize the ecosystem's landscape.  
Implementation Specification:

* Library: Chart.js Scatter.  
* X-Axis: Parameter Count (Logarithmic Scale). *Note: Log scale is essential as parameters range from 7B to 1T+.*  
* Y-Axis: Trust Score (0-21 aggregate of the Rose Chart).  
* Color: Vendor or License Type (Open vs. Closed).  
* Insight: Clusters in the top-right indicate "High Power, High Trust." Clusters in the bottom-right indicate "High Power, Low Trust" (the danger zone).

### Rank 6: The Deployment Timeline with Policy Overlays

Concept: Historical context of model releases.  
Schema Mapping: model\_identity.release\_date.  
Data Type: Time-series.  
Rationale:  
Governance is temporal. A model released before the EU AI Act might document privacy differently than one released after. Plotting models on a timeline, overlaid with vertical annotations for major regulatory milestones (NIST RMF release, ChatGPT launch, EU AI Act), contextualizes the documentation quality.11  
Implementation Specification:

* Library: Chart.js Line/Scatter with Annotation Plugin.  
* Data: Models plotted by date (X) and Trust Score (Y).  
* Annotations: Vertical lines for regulatory events. This helps identify if governance interventions are actually moving the needle on model documentation quality.

## ---

3\. Comparison Tool Specification

Research Question 2: What interaction patterns would let users compare 2-4 models on trustworthiness characteristics side-by-side?

Comparing qualitative governance documentation is fundamentally different from comparing quantitative benchmarks. In a benchmark comparison, the user looks for the *highest number*. In a governance comparison, the user looks for *divergence in policy* and *gaps in evidence*. The interaction design must support this "investigative" mode.

### 3.1 Interaction Pattern: The "Persisted Basket"

Since the site is static (Astro) and lacks a backend database for sessions, the "Comparison Basket" must be managed entirely client-side using localStorage and URL query parameters.

Workflow:

1. Selection (The Add Action):  
   * On the Model Card or Listing Page, a "Compare" checkbox (or \+ icon) is available.  
   * State Management: Toggling this updates a reactive store (using Nano Stores or simple React state) that persists to localStorage.  
   * Feedback: A "floating dock" or sticky footer appears: *"Comparing 2 of 4 models: Llama 3, Claude 3... \[Compare Now\]"*.  
2. Navigation (The Handoff):  
   * Clicking "Compare Now" navigates to /compare.  
   * URL State: The URL is constructed as /compare?ids=model\_a,model\_b,model\_c. This ensures the comparison view is shareable (a critical governance requirement for emailing findings to stakeholders).  
3. Hydration (The Load):  
   * The /compare page is a static shell.  
   * On load, a client-side script reads the URL parameters.  
   * It fetches the full models.json index (generated at build time) or individual JSON chunks for the requested IDs to minimize bandwidth.

### 3.2 The "Cognitive Diff" Interface

Standard comparison tables (like Amazon product specs) fail for long text. They force the user to ping-pong their eyes between columns to find differences. We propose a "Cognitive Diff" pattern that computationally highlights divergence.

Feature: Keyword Divergence Highlighting  
Since we cannot run a semantic LLM comparison in the browser efficiently, we use a deterministic "Keyword Bag" approach:

1. Dictionary: Define a client-side dictionary of "Risk Keywords" (e.g., *hallucination, bias, red-team, internal, external, audit, third-party, indemnification*).  
2. Processing: When rendering the text fields side-by-side:  
   * Highlight "Positive Signal" words (e.g., *audit, external*) in Green.  
   * Highlight "Negative Signal" words (e.g., *internal-only, limited*) in Yellow.  
   * Bold any keywords that appear in Model A but are absent in Model B.  
3. Result: The user can scan the table and instantly see that Model A discusses "indemnification" while Model B does not, without reading every word.

### 3.3 Handling Sparse Data ("Not Assessed")

In the 162-card dataset, missing data is a signal, not a bug.

* Anti-Pattern: Leaving the cell blank. This looks like a loading error.  
* Governance Pattern: The "Null State" visualization.  
  * Visual: A cell with a gray diagonal hatch pattern.  
  * Icon: A "Ghost" or "Unknown" icon (e.g., QuestionMarkCircle from Heroicons).  
  * Text: Explicitly state "Not Disclosed" or "Not Assessed."  
  * Comparison Logic: If Model A has data and Model B is "Null," the comparison row should visually penalize Model B (e.g., a red border on the empty cell), drawing attention to the lack of transparency.

### 3.4 Mobile Considerations: The "Pivot" View

Side-by-side columns do not work on mobile.

* Desktop: 3-4 Columns.  
* Mobile: Pivot Interaction.  
  * User selects a "Primary Model" (pinned to top).  
  * User selects a "Comparison Target" (dropdown).  
  * The view renders as an Interleaved List:  
    * *Row Header: Fairness*  
    * *Model A (Primary):*  
    * *Model B (Target):* (Indented, distinct background color).  
  * This vertical stacking allows for comparing text blocks on narrow screens without horizontal scrolling.

## ---

4\. Aggregate Dashboard Components

Research Question 3: What cross-model aggregate views would be most valuable?

The Aggregate Dashboard moves beyond individual model assessment to "Ecosystem Assessment." This serves the Researcher and Policy Maker personas. These components are powered by an analytics.json file generated during the Astro build process, ensuring instant loading without database queries.

### Component 1: The Ecosystem Treemap (Organization vs. Risk)

User Question: "Which vendors are producing the most documented models?"  
Visualization: Zoomable Treemap.12

* Hierarchy: Organization (e.g., Meta, Google, Mistral) \-\> Model Family (e.g., Llama, Gemma) \-\> Model.  
* Tile Size: Parameter Count (Proxy for model complexity/resource cost).  
* Tile Color: Aggregate Trust Score (0-21 scale derived from the Rose Chart).  
  * *Green:* Highly Documented.  
  * *Red:* Undocumented/Opaque.  
* Insight: A large red tile (e.g., a massive model with no documentation) stands out immediately as a systemic risk.

### Component 2: The "Trust Gap" Histogram

User Question: "Is the industry mostly compliant or mostly opaque?"  
Visualization: Bar Histogram.

* X-Axis: Trust Score Range (0-5, 6-10, 11-15, 16-21).  
* Y-Axis: Number of Models.  
* Overlay: Segment bars by License Type (Open Source vs. Proprietary).  
* Insight: This reveals if open-weights models are systematically more (or less) transparent than closed API models, a key debate in AI governance.13

### Component 3: The Licensing Sunburst

User Question: "How many models are truly open source vs. 'source available'?"  
Visualization: Sunburst Chart (Multi-level Pie).

* Inner Ring: General License Category (Open Source, Proprietary, Source Available).  
* Outer Ring: Specific License (Apache 2.0, MIT, Llama Community, CreativeML).  
* Interaction: Clicking a segment acts as a filter, redirecting to the discovery page with that license pre-selected.

### Component 4: The Modality Support Matrix

User Question: "What is the standard capability set today?"  
Visualization: Heatmap/Matrix (as described in Visualizations Rank 4).

* Data: Aggregated counts of I/O modalities.  
* Insight: Identifies gaps in the market (e.g., "Why are there 50 Text-to-Text models but only 2 Audio-to-Code models?").

## ---

5\. Discovery UX Recommendation

Research Question 4: Discovery Patterns.

The audience for this Wiki is professional. They are not "browsing for fun"; they are "hunting for a solution" that meets strict criteria. Therefore, we reject the "Netflix-style" browse-first pattern in favor of a Filter-First (Faceted Search) approach, augmented by Question-First shortcuts.

### 5.1 The "Governance Console" (Faceted Search)

We utilize Pagefind 15 to implement a powerful, client-side faceted search engine. Pagefind indexes the *rendered HTML*, meaning it indexes exactly what the user sees, including the "Trust Scores" we inject into the DOM.

The Filter Architecture:

* Facet 1: NIST Maturity: Checkboxes for "Verified Evidence," "Claims Only," "Unassessed."  
* Facet 2: RMF Coverage: A slider or set of ranges for "RMF Completeness %."  
* Facet 3: Technical Constraints: Sliders for "Parameters (B)" and "Context Window (k tokens)."  
* Facet 4: Legal: Checkboxes for "Commercial Use Allowed," "Weights Available."

Technical Implementation:  
In the Astro template, we inject hidden data attributes:

HTML

\<div data-pagefind-body\>  
  \<h1 data-pagefind-meta\="title"\>Llama 3\</h1\>  
  \<span data-pagefind-filter\="license"\>Open Weights\</span\>  
  \<span data-pagefind-filter\="trust\_level"\>High\</span\>  
  \<span data-pagefind-filter\="modality"\>Text-to-Text\</span\>  
\</div\>

Pagefind automatically generates the index from these attributes, allowing for sub-millisecond filtering on the client side.

### 5.2 The "Question-First" Accelerators

To reduce the cognitive load of configuring complex filters, the landing page should feature "Intent Buttons"—natural language sentence builders that pre-configure the search state.

* *"I need a model for \[Healthcare\] applications."* \-\> (Sets Trust\>15, Domain=Medical).  
* *"Show me models that run on \[Consumer Hardware\]."* \-\> (Sets License=OSI, Params\<14B).  
* *"Find models with aligned with \[EU AI Act\]."* \-\> (Sets Safety=Verified, Region=EU).

This "Mad Libs" style interface guides the user into the faceted search results with a high-relevance starting state.

## ---

6\. Model Card Page Layout

Research Question 7: Ideal Information Hierarchy.

The layout must invert the typical "Hugging Face" pattern (which prioritizes download counts and file trees). The "Inverted Pyramid of Trust" places governance at the top.

### Zone 1: The Trust Header (Above the Fold)

* Identity: Model Name, Version, Organization (Logo).  
* The "Trustworthiness Rose": A prominent, interactive Rose chart giving the immediate "Risk Profile."  
* The "Traffic Light" Badges: 3-4 critical binary signals using Harvey Balls or Icons:  
  * *License:* Green (Open) / Yellow (Restricted) / Red (Closed).  
  * *Audit Status:* Checkmark (External) / Exclamation (Internal).  
  * *Training Data:* Database Icon (Disclosed) / Crossed Eye (Undisclosed).  
* Primary Action: "Compare" (Toggle) and "View RMF Mapping" (Anchor Link).

### Zone 2: The Evidence Locker (Tabbed Interface)

* Tabs: The 7 NIST Characteristics.  
* Content: The "Diff-Ready" Claims vs. Evidence tables.  
* Why Tabs? Keeping the 7 sections visible prevents "Scroll Fatigue." Users can jump specifically to "Privacy" if that is their audit focus.

### Zone 3: The Technical Sidebar (Right Rail on Desktop)

* Specs: Parameters, Context Window, Architecture (Transformer/Mamba/MoE).  
* Compute: Training FLOPs, Carbon Emission estimates (if available).  
* Resources: Links to papers, GitHub, HF Repo.  
* *Why Sidebar?* This is reference data. It should be available but not block the flow of governance reading.

### Zone 4: The Process Footer (The RMF Heatmap)

* Visualization: The RMF Matrix Heatmap.  
* Content: Collapsible accordions for the detailed "Map, Measure, Manage" entries.  
* *Why Footer?* This is the "Deep Audit" trail. It is necessary for compliance reporting but dense.

## ---

7\. Anti-Patterns (The "Avoid List")

Research Question 6: What to avoid.

### 1\. The "Single Safety Score" Fallacy

Avoid: Aggregating the 7 NIST scores into a single "Safety Rating: 8.5/10."  
Reasoning: This hides critical failure modes. A model can be scored 10/10 on "Fairness" (perfect demographic parity) but 0/10 on "Safety" (generates malware). Averaging them to a "5/10" helps no one. Governance requires granularity, not smoothing. Use the Rose Chart profile, not a single number.

### 2\. Gamified Leaderboards

Avoid: "Gold/Silver/Bronze" badges or numbered rankings (1st, 2nd, 3rd).  
Reasoning: Gamification encourages "Overfitting to the Test." Vendors will copy-paste generic safety boilerplate to get the "Gold Badge".18 Governance tools should remain neutral and descriptive ("Audit Verified"), not judgmental ("Best").

### 3\. 3D Visualizations

Avoid: 3D Pie Charts, 3D Bar Charts.  
Reasoning: In data visualization, 3D adds distortion and occlusion while adding zero information density.20 For a professional tool, flat, high-contrast, accessible design is mandatory.

### 4\. "Network Graph" Lineage

Avoid: Complex force-directed graphs to show Model Lineage (Model A \-\> Finetuned \-\> Model B).  
Reasoning: While visually impressive, these are unusable on mobile, inaccessible to screen readers, and often become "hairballs" with \>100 nodes. Use simple Tree Lists or Breadcrumbs for lineage.

### 5\. Infinite Scroll

Avoid: Infinite scroll for the model list.  
Reasoning: Governance users need to "finish" a search. Pagination allows for referencing ("See Page 3 of the results"). Infinite scroll breaks the "Ctrl+F" search capability and footer access.

## ---

8\. Inspiration Sources

Research Question 7: Adaptable examples.

1. Stanford CRFM Ecosystem Graphs 21:  
   * *Adapt:* Their structured schema for "Dependencies" (Data \-\> Model \-\> App).  
   * *Improve:* Their visualization is often too dense (network graph). We will adapt their *data structure* but visualize it using the simpler RMF Matrix.  
2. Credo AI Governance Dashboards 23:  
   * *Adapt:* Their use of "Policy Packs" and alignment checklists.  
   * *Improve:* Credo is an enterprise SaaS. We can adapt their "Compliance Gap" visualization (Bar charts showing met vs. unmet requirements) for our static RMF Heatmap.  
3. Hugging Face Model Cards 25:  
   * *Adapt:* The YAML header metadata standard.  
   * *Improve:* HF cards are inconsistent free-text. We will enforce the *structure* that HF suggests but rarely enforces, visualizing the *completeness* of that metadata.  
4. Open LLM Leaderboard (Hugging Face) 2:  
   * *Avoid:* Their focus on single-number metrics.  
   * *Adapt:* Their concept of "selectable columns" in the table view. We can allow users to toggle which NIST columns are visible in the comparison view.

## ---

9\. Implementation Guide: The Static Stack

To achieve this functionality within the technology constraints:

### Technology Stack

* Core: Astro (Zero-JS by default, highly performant static HTML generation).  
* Styling: Tailwind CSS (Utility-first, ensures consistent spacing/typography for complex grids).  
* Visualizations:  
  * Chart.js: Robust, canvas-based, tree-shakeable. Excellent for Rose Charts (polarArea).  
  * chartjs-chart-matrix: For the RMF Heatmap.  
  * chartjs-chart-treemap: For the Ecosystem view.  
* Search: Pagefind (CloudCannon). Binary-based static search, supports complex filtering logic client-side.  
* Icons: Phosphor Icons or Heroicons (SVG).

### Data Pipeline (Build Time)

1. Ingestion: Astro's getCollection('models') loads the 162 YAML files.  
2. Normalization: A Zod schema validates the data types.  
3. Scoring: A build script iterates through the collection:  
   * Calculates the "Trust Score" (0-3) for each NIST characteristic based on field presence and regex keyword matching (e.g., presence of "external audit" url \= Score 3).  
   * Aggregates the analytics.json for the dashboard.  
4. Injection: The computed scores are injected as data-pagefind-filter attributes in the HTML for search, and as JSON props for the Chart.js islands.

### Comparison Logic (Client Side)

1. State: nano-stores (lightweight) manages the "Basket" of selected IDs.  
2. Routing: Navigation to /compare triggers a client-side fetch of the specific model data (pre-chunked JSON to avoid loading the whole DB).  
3. Rendering: A Preact or React island renders the "Diff Table," performing the keyword highlighting logic in the browser.

This architecture ensures the site is fast, secure, and capable of handling complex "App-like" interactions (comparison, filtering) without a single server-side API call.

## ---

10\. Conclusion

The transition from "Model Cards as READMEs" to "Model Cards as Governance Dashboards" requires a shift in visual language. By moving away from linear leaderboards and embracing multidimensional visualizations like the Nightingale Rose Chart and Process Heatmaps, this Wiki can provide the nuance required for high-stakes AI adoption. The proposed "Trustboard" does not just display data; it contextualizes it, highlighting the gap between vendor claims and verifiable reality. This is the essential function of modern AI governance.

### Works cited

1. An Empirical Analysis of Machine Learning Model and Dataset Documentation, Supply Chain, and Licensing Challenges on Hugging Face \- arXiv, accessed December 31, 2025, [https://arxiv.org/html/2502.04484v2](https://arxiv.org/html/2502.04484v2)  
2. Demystifying LLM Leaderboards: What You Need to Know \- Shakudo, accessed December 31, 2025, [https://www.shakudo.io/blog/demystifying-llm-leaderboards-what-you-need-to-know](https://www.shakudo.io/blog/demystifying-llm-leaderboards-what-you-need-to-know)  
3. Why AI leaderboards are inaccurate and how to fix them | University of Michigan News, accessed December 31, 2025, [https://news.umich.edu/why-ai-leaderboards-are-inaccurate-and-how-to-fix-them/](https://news.umich.edu/why-ai-leaderboards-are-inaccurate-and-how-to-fix-them/)  
4. A comprehensive guide to diff tools \- Graphite, accessed December 31, 2025, [https://graphite.com/guides/comprehensive-guide-to-diff-tools](https://graphite.com/guides/comprehensive-guide-to-diff-tools)  
5. Compare proofs in the proofing viewer | Adobe Workfront, accessed December 31, 2025, [https://experienceleague.adobe.com/en/docs/workfront/using/workfront-proof/work-with-proofs-in-wf-proof/review-proofs-web-proofing-viewer/compare-proofs](https://experienceleague.adobe.com/en/docs/workfront/using/workfront-proof/work-with-proofs-in-wf-proof/review-proofs-web-proofing-viewer/compare-proofs)  
6. Why you should avoid radar charts in data visualization \- Observable, accessed December 31, 2025, [https://observablehq.com/blog/avoid-radar-charts](https://observablehq.com/blog/avoid-radar-charts)  
7. Usage | chartjs-chart-matrix, accessed December 31, 2025, [https://chartjs-chart-matrix.pages.dev/usage](https://chartjs-chart-matrix.pages.dev/usage)  
8. Heatmaps in Data Visualization: A Comprehensive Introduction \- Inforiver, accessed December 31, 2025, [https://inforiver.com/insights/heatmaps-in-data-visualization-a-comprehensive-introduction/](https://inforiver.com/insights/heatmaps-in-data-visualization-a-comprehensive-introduction/)  
9. Harvey balls and checkboxes | think-cell, accessed December 31, 2025, [https://www.think-cell.com/en/resources/manual/harvey-balls-and-checkboxes](https://www.think-cell.com/en/resources/manual/harvey-balls-and-checkboxes)  
10. Build a Customizable Progress Indicator with Pure CSS: Harvey Ball Example \- BAVAGA, accessed December 31, 2025, [https://www.bavaga.com/blog/2023/01/06/customizable-progress-indicator-pure-css-harvey-ball/](https://www.bavaga.com/blog/2023/01/06/customizable-progress-indicator-pure-css-harvey-ball/)  
11. fanthos/chartjs-chart-timeline \- GitHub, accessed December 31, 2025, [https://github.com/fanthos/chartjs-chart-timeline](https://github.com/fanthos/chartjs-chart-timeline)  
12. Usage | chartjs-chart-treemap, accessed December 31, 2025, [https://chartjs-chart-treemap.pages.dev/usage](https://chartjs-chart-treemap.pages.dev/usage)  
13. The Leaderboard Illusion \- arXiv, accessed December 31, 2025, [https://arxiv.org/html/2504.20879v1](https://arxiv.org/html/2504.20879v1)  
14. The Leaderboard Illusion arXiv:2504.20879v2 \[cs.AI\] 12 May 2025, accessed December 31, 2025, [https://arxiv.org/pdf/2504.20879](https://arxiv.org/pdf/2504.20879)  
15. How to add fast, client-side search to Astro static sites \- Evil Martians, accessed December 31, 2025, [https://evilmartians.com/chronicles/how-to-add-fast-client-side-search-to-astro-static-sites](https://evilmartians.com/chronicles/how-to-add-fast-client-side-search-to-astro-static-sites)  
16. Filtering using the Pagefind JavaScript API, accessed December 31, 2025, [https://pagefind.app/docs/js-api-filtering/](https://pagefind.app/docs/js-api-filtering/)  
17. Implementing Pagefind Search in Astro, accessed December 31, 2025, [https://www.interviewhelper.in/guides/pagefind-search](https://www.interviewhelper.in/guides/pagefind-search)  
18. Gamification Grew Up \- David R. Longnecker, accessed December 31, 2025, [https://drlongnecker.com/blog/2025/10/gamification-evolution-ai-personalized-customer-retention/](https://drlongnecker.com/blog/2025/10/gamification-evolution-ai-personalized-customer-retention/)  
19. The Dark Side of Gamification: When Points, Badges, & Leaderboards Go Wrong, accessed December 31, 2025, [https://www.growthengineering.co.uk/dark-side-of-gamification/](https://www.growthengineering.co.uk/dark-side-of-gamification/)  
20. Dashboard Design: 7 Best Practices & Examples \- Qlik, accessed December 31, 2025, [https://www.qlik.com/us/dashboard-examples/dashboard-design](https://www.qlik.com/us/dashboard-examples/dashboard-design)  
21. Ecosystem Graphs for Foundation Models \- Stanford CRFM, accessed December 31, 2025, [https://crfm.stanford.edu/ecosystem-graphs/](https://crfm.stanford.edu/ecosystem-graphs/)  
22. Ecosystem Graphs: Documenting the Foundation Model Supply Chain \- AAAI Publications, accessed December 31, 2025, [https://ojs.aaai.org/index.php/AIES/article/download/31629/33796/35693](https://ojs.aaai.org/index.php/AIES/article/download/31629/33796/35693)  
23. Credo AI \- The Trusted Leader in AI Governance, accessed December 31, 2025, [https://www.credo.ai/](https://www.credo.ai/)  
24. Gen AI \- Credo AI, accessed December 31, 2025, [https://www.credo.ai/solutions/gen-ai](https://www.credo.ai/solutions/gen-ai)  
25. User Studies \- Hugging Face, accessed December 31, 2025, [https://huggingface.co/docs/hub/model-cards-user-studies](https://huggingface.co/docs/hub/model-cards-user-studies)  
26. Model Cards \- Hugging Face, accessed December 31, 2025, [https://huggingface.co/docs/hub/model-cards](https://huggingface.co/docs/hub/model-cards)  
27. Open LLM Leaderboard: Benchmarks, Model Types & Filters Explained \- Obot AI, accessed December 31, 2025, [https://obot.ai/resources/learning-center/open-llm-leaderboard/](https://obot.ai/resources/learning-center/open-llm-leaderboard/)
