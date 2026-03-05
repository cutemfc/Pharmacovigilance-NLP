# Pharmacovigilance-NLP
## Goal 
This research explores the application of Natural Language Processing (NLP) to detect Adverse Drug Events (ADEs) within Electronic Health Records (EHR). Specifically, we analyze the causal linkage between Drug Dosage, ADEs, and Hospitalization Outcomes.
## What is Pharmacoviligance
Traditional Approach: Heavily manual, relying on Spontaneous Reporting Systems (SRS). Experts must review thousands of "Yellow Cards" and unstructured physician notes to find safety signals. This process is slow, expensive, and reactive.

The NLP Transformation: By using Transformer-based models, PV transitions to a proactive system capable of scanning millions of records in real-time to identify rare or emerging drug-drug interactions that traditional reporting might miss.
## 2026 Regulatory Compliance (EU AI Act & EMA)
A. EU Regulatory Classification:
According to the EU AI Act (passed in 2024, fully applicable in 2026), AI systems used for pharmacovigilance are generally classified as "High-Risk" systems.

B. Three Pillars of Compliance:

Transparency & Traceability: The law requires the ability to "reconstruct" every decision made by the AI. Improvement: Systems must log model versions, data provenance, and generation logic (e.g., providing Chain-of-Thought explanations).

Data Governance: Clinical data used to fine-tune BERT/T5 must be high-quality, unbiased, and representative.

Accuracy & Robustness: Systems must monitor for "Data Drift" to ensure accuracy remains stable across different hospitals and patient demographics.
## Critical Requirement: Human Oversight
The EU AI Act explicitly mandates that AI cannot replace the final judgment of a pharmacovigilance expert.

Human-in-the-Loop (HITL): AI handles preliminary screening (Triage) and extraction, but the final regulatory reports (e.g., ICSRs) must be reviewed and signed by a qualified professional (e.g., QPPV).

Preventing Automation Bias: Medical professionals must have the authority to "override" AI judgments. If an AI determines "dosage unrelated" but a physician's experience suggests "related," the human judgment prevails.

Decision Support, Not Decision Maker: AI should be positioned as an "Intelligent Scanner," reducing 80% of redundant work so experts can focus on qualitative analysis of high-risk cases.

## Could AI can help to extract the keywords of Electronic Health Record?

Extraction (BERT): Precisely capture dosage and symptom entities.

Reasoning (Flan-T5): Link the causal relationships between entities and generate summaries.

### Model Architectures: BERT vs. T5

BERT (Encoder-Only)
Mechanism: Uses a "Bidirectional" approach to understand the context of a word based on its surrounding text.

PV Utility: Best for Named Entity Recognition (NER). It excels at accurately tagging specific data points in medical text: [Drug: Warfarin], [Dosage: 5mg], [Symptom: Hematuria].

T5 (Encoder-Decoder)
Mechanism: A "Text-to-Text" framework that treats every NLP task as a translation or generation problem.

PV Utility: Best for Causal Reasoning. As Li et al. (2024) highlighted, Flan-T5 is superior at following instructions to link events: "Was this dosage responsible for the patient's admission?"

#### Model Comparison for Pharmacovigilance (PV)
## 📊 Model Comparison: BERT vs. T5 in Pharmacovigilance

According to recent benchmarks (Li et al., 2024), selecting the right architecture is critical for balancing entity extraction with causal reasoning.

| Feature | BioBERT / ClinicalBERT | Clinical-T5 (Pre-trained) | Flan-T5 (Instruction-tuned) |
| :--- | :--- | :--- | :--- |
| **Primary Strength** | **Named Entity Recognition (NER)** | Medical terminology fluency | **Logic & Multi-hop Reasoning** |
| **PV Application** | Extracting Drug names & Dosages | Summarizing clinical text | **Linking Dosage → ADE → Hospitalization** |
| **Data Efficiency** | Requires large labeled datasets | High resource (Pre-training) | **Superior Zero/Few-shot performance** |
| **Generalizability** | Low (Specific to training site) | Moderate (Overfits to MIMIC) | **High (Robust across clinical tasks)** |

> **Note:** While BERT models remain the "gold standard" for precise entity tagging, Instruction-tuned models like **Flan-T5** are increasingly preferred for complex tasks that require linking clinical events and understanding dosage-related causality.



