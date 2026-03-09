# Theory Chapters Plan

## Length Estimation Basis
- All section targets are given in pages first, with approximate words in brackets.
- Conversion used: **1 page ≈ 300–350 words** (depends on figures, equations, and spacing).

## Purpose
This document defines the final plan for the theory/background chapter(s) before writing begins. The goal is to ensure:
- complete coverage of required background,
- a clear argument chain,
- explicit section size targets,
- clean separation from Methods.

## Final Section Structure (and Why)

### 0) Short Introduction to Theory Chapters
**Overall Purpose:** Outline the structure of these chapters and the purpose for each. 


### 1) In-Vehicle Systems and Event-Triggered Logging
**Overall Purpose:** Establish the technical domain and neutral baseline.

**Must cover:**
- What an in-vehicle embedded system is; role of ECUs.
- What in-vehicle networks are and why they are needed.
- What event-triggered logging is and why it is used in practice.

**Tone guidance:**
- Keep this section mostly descriptive and neutral.
- Mention constraints/limitations only lightly; reserve stronger problem framing for Section 2.

**Target size:** 1.2–1.6 pages (**~360–560 words**).
 
#### Paragraph 1.1 - In-vehicle Embedded System
**Purpose:** Define the in-vehicle embedded system as a distributed, real-time automotive architecture and introduce its core components.

**Add:**
- One short sentence clarifying ECU role: local control logic + signal processing + network communication.
- Short sentence explaining how this leads to a coordinated and connected system which produces telemetry data that will be the focus of this thesis. 

**Remove:**
- No removals needed from the current Paragraph 1 text.

#### Paragraph 1.2 — In-vehicle Networks
**Purpose:** Explain why in-vehicle networks are required to coordinate distributed ECUs and enable reliable signal exchange across vehicle functions.

**Add:**
- Go into more detail on the connection of ECUs and the networks. 
- Go into more detail on the key network families and the purpose of the functional role of the in-vehicle networks. 

**Remove:**
- Remove anything on event-triggered logging.

#### Paragraph 1.3 - Event-triggered Logging 
**Purpose:** Introduction to event-triggered logging, its purpose, and its traditional shortcomings. 

**Add:**
- Go into more detail on how it is implemented. 
- Go into more detail on the advantages and the reasons it works. 
- Keep shortcomings here focused on traditional operational limits (e.g., threshold tuning burden, missed low-amplitude events, maintenance overhead), while reserving modern ML/data-scale pressure for Section 2.

---

### 2) Downstream ML Tasks and Data Quantity
**Overall Purpose:** Introduce the modern pressures that make existing telemetry/logging strategies insufficient.

**Must cover:**
- What downstream ML tasks are in this context (e.g., predictive maintenance, anomaly detection, fleet analytics).
- Why these tasks matter for modern vehicle systems.
- The data quantity problem in modern in-vehicle systems.
- Why in-vehicle networks/logging setups struggle with this combination (utility needs + data scale).

**Output of section:**
- A clear statement of the core tension: data reduction vs. utility for ML.

**Target size:** 1.5–2.0 pages (**~450–700 words**).
 
#### Paragraph 2.1 - Intro
**Purpose:** Introduce the two developments challenging the status quo.

**Add:**
- Add a 2–3 sentence framing contrast between the historical objective (traffic/storage reduction) and the modern objective (preserving downstream ML utility).

**Remove:**
- Not needed.

#### Paragraph 2.2 - Data Quantity Issue
**Purpose:** Explain the data quantity issue, where it comes from, and what challenges it brings with it. 

**Add:**
- Add a short source breakdown of growth drivers: more sensors, higher sampling rates, richer modalities, and connected-fleet scale.
- Add 2–3 concrete consequences for the pipeline: storage burden, transmission bottlenecks, and preprocessing/compute overhead.
- Add one sentence clarifying why naive full-resolution logging is operationally difficult at scale.

**Remove:**
- Not needed.

#### Paragraph 2.3 - Downstream ML Importance
**Purpose:** Introduce the importance of downstream ML models in modern vehicles and vehicle companies. 

**Add:**
- Add one sentence defining downstream ML tasks in the thesis context (post-collection analysis for maintenance, anomaly detection, and fleet optimization).
- Add one sentence linking business/engineering relevance to why utility loss is costly.

**Remove:**
- Not needed.

#### Paragraph 2.4 - Shortcomings of Event-Triggered Logging
**Purpose:** Explain how event-triggered logging struggles with these developments.

**Add:**
- Add an explicit mechanism-level mismatch statement: threshold/event-only capture can miss weak precursors and surrounding temporal context.
- Add one sentence on why fragmented sampling degrades model robustness and generalization for downstream tasks.
- Add a strong bridge sentence to Section 3: compression is needed, but should be evaluated under a rate-utility lens.
- Add a closing emphasis sentence (comment): in modern telemetry pipelines, compression quality should be judged by both bitrate reduction and downstream-task utility retention, not bitrate alone.

**Remove:**
- Not needed.

---

### 3) Compression Theory and Traditional Methods
**Purpose:** provide the conceptual tools needed to evaluate compression approaches.

**Must cover:**
- Compression from an information-theory perspective (high-level, thesis-appropriate depth).
- Core idea of lossy compression pipeline.
- Rate-utility (or rate-distortion-utility) framing used later in experiments.
- Traditional methods relevant to telemetry/time-series and their practical shortcomings for your problem.

**Important boundary:**
- Keep this section conceptual + comparative.
- Do not drift into implementation-level details that belong in Methods.

**Target size:** 1.5–2.0 pages (**~450–700 words**).

#### Paragraph 3.1 - Information Theory 
**Purpose:** Introduce relevant information theory concepts and how compression uses them. 

**Add:**
- Introduction to information theory principles.

**Remove:**
- Not needed.

#### Paragraph 3.2 - Compression
**Purpose:** Show how the information theory concepts work together to enable compression. 

**Add:**
- Introduction to compression terms and concepts such as lossy compression. 

**Remove:**
- Not needed.

#### Paragraph 3.3 - The Rate-Utility Framework
**Purpose:** Show the importance of the rate-utility concept in the context of compression. 

**Add:**
- Introduction to the rate-utility concept. 
- Add concrete utility metrics to be referenced later (e.g., downstream task performance such as F1/AUROC for anomaly tasks, MAE/RMSE for regression-style tasks).
- Add concrete rate proxies to be referenced later (e.g., compression ratio, bits-per-sample, and storage/transmission reduction).
- Add one sentence clarifying that exact metric definitions and protocol belong to Methods, while Theory only motivates why these metrics are appropriate.

**Remove:**
- Not needed.

#### Paragraph 3.4 - Traditional Compression Methods
**Purpose:** Introduce traditional methods relevant to telemetry/time-series and their practical shortcomings for this problem.

**Add:**
- Expand on the current sections in more detail.

**Remove:**
- Not needed. 

---

### 4) Neural Compression (with Focused Related Work)
**Purpose:** motivate and position the method family your thesis builds on.

**Must cover:**
- What neural compression is.
- How neural compression can be implemented for time series:
  - temporal sequence modeling,
  - continuous vs. discrete latent spaces,
  - vector quantization/tokenization idea.
- How neural compression methods are benchmarked under rate-utility trade-offs.
- Related work focused on neural compression only (your chosen scope).

**Explicitly exclude from deep related-work treatment:**
- utility-aware adaptive telemetry,
- task-aware compression techniques.

**Target size:** 1.5–2.0 pages (**~450–700 words**).

#### Paragraph 4.1 - Neural Compression Idea
**Purpose:** Introduce the idea of neural compression and how it relates to traditional compression methods.

**Add:**
- Introduction to neural compression.

**Remove:**
- Not needed.

#### Paragraph 4.2 - Deep Learning Basics 
**Purpose:** Introduce core machine and deep learning concepts necessary to understand and implement neural compression. 

**Add:**
- Introduction to temporal sequence modeling, continuous vs. discrete latent spaces, vector quantization/tokenization idea.
- Keep scope strict: include only concepts directly needed to understand neural compression choices; omit generic deep-learning background.

**Remove:**
- Not needed.

#### Paragraph 4.3 - Benchmarking
**Purpose:** Introduce how neural compression methods are benchmarked. 

**Add:**
- Introduction to how neural compression models work within the rate-utility trade-off framework. 

**Remove:**
- Not needed.

#### Paragraph 4.4 - Related Work
**Purpose:** Introduce relevant related work in the field of neural compression.

**Add:**
- Introduction to related work. 
- Add one explicit scope-justification sentence: utility-aware adaptive telemetry and task-aware compression are acknowledged but not reviewed in depth because this thesis focuses on representation-level neural compression/tokenization methods.
- Add one thesis-positioning bridge sentence at the end: this motivates selecting tokenization-based neural compression as the most relevant subset for the methodology developed in this thesis.

**Remove:**
- Not needed.

---

### 5) Transition to Methods (short closing subsection/paragraph)
**Purpose:** close Theory with positioning and hand-off, without formal hypotheses.

**Must do:**
- Summarize why tokenization-based neural compression is the logical next step.
- State scope boundaries briefly (what is and is not covered).
- Point directly to Methods for implementation and experimental setup.

**Target size:** 0.4–0.8 pages (**~120–280 words**).

## Argument Chain (Golden Thread)
Use this logic sequence throughout the chapter:
1. Vehicles are complex embedded networked systems with practical telemetry constraints.
2. Modern downstream ML tasks increase utility requirements for collected data.
3. Data quantity is rapidly increasing, making naive collection/transmission infeasible.
4. Therefore, compression is necessary—but must be judged beyond pure reconstruction.
5. Traditional methods are useful but limited for this specific telemetry + ML setting.
6. Neural compression (especially discrete/tokenized variants) is a promising direction.
7. This motivates the concrete methodological choices presented in Methods.

## Suggested Internal Length Split (quick reference)
- Section 1: 1.2–1.6 pages (**~360–560 words**)
- Section 2: 1.5–2.0 pages (**~450–700 words**)
- Section 3: 1.5–2.0 pages (**~450–700 words**)
- Section 4: 1.5–2.0 pages (**~450–700 words**)
- Section 5: 0.4–0.8 pages (**~120–280 words**)

## Writing Guardrails (to avoid overlap with Methods)
- **Theory answers:** *why this problem exists* and *why this approach class is justified*.
- **Methods answers:** *exactly how the model/pipeline is implemented and evaluated*.
- Keep equations/framework definitions in Theory only when they are needed to understand comparisons and metrics.
- Keep architecture, hyperparameters, data splits, and experiment protocol in Methods.

