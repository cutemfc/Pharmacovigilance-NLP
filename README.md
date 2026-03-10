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

## Could AI help to extract the keywords of Electronic Health Record?

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
 ## How could I avoid Hallucination of PV
**The Workflow**: Cross-Verification Logic
The most effective implementation involves a pipeline that separates Information Extraction from Causal Reasoning. Why: BERT-based models are "discriminative"—they classify existing text rather than generating new text. They are highly stable and rarely "hallucinate" entities that aren't there.

 **Step 1:**  Entity Extraction with ClinicalBERT (Discriminative)
Action: Use a model like ClinicalBERT or BioBERT to perform Named Entity Recognition (NER).
Why: BERT-based models are "discriminative"—they classify existing text rather than generating new text. They are highly stable and rarely "hallucinate" entities that aren't there.

**Step 2:** Relationship Reasoning with Flan-T5 (Generative)
Action: Feed the original text and the entities identified by BERT into Flan-T5. Ask the model to determine the causal relationship.
Why: BERT-based models are "discriminative"—they classify existing text rather than generating new text. They are highly stable and rarely "hallucinate" entities that aren't there.

Instruction: "Based on the text, did the confirmed drug (Lisinopril) cause the confirmed event (Angioedema)? Explain the logic."

Why: T5 excels at understanding the nuances of medical narratives (e.g., temporal relationships or "treatment-emergent" logic).

**Step 3:** : Consistency Check & Flagging
The "Agreement" Rule: If Flan-T5 tries to link a drug or symptom that ClinicalBERT did not find, or if the logic contradicts the extracted entities, the system triggers a Consistency Error.

Action: Any discrepancy between the BERT "extraction" and the T5 "reasoning" is automatically flagged for Manual Intervention.




Output: Confirmed entities like [Drug: Lisinopril], [Dosage: 20mg], and [Adverse Event: Angioedema].
### Recommended Project Architecture
To satisfy both performance and compliance, this repository recommends the following pipeline:
Extraction (BERT): Precisely capture dosage and symptom entities.

Reasoning (Flan-T5): Link the causal relationships between entities and generate summaries.

Human Review Interface: Visualize the AI's reasoning logic, providing experts with a "one-click" audit and modification tool to ensure the Traceability required by EU law.

## Reference
1.EMA & FDA Alignment (January 2026), "Guiding Principles of Good AI Practice in Drug Development."

2.Li, Y., Harrigian, K., Zirikly, A., & Dredze, M. (2024). Are Clinical T5 Models Better for Clinical Text? Proceedings of the 4th Machine Learning for Health (ML4H) Symposium.

3.Alsentzer, E., et al. (2019). "Publicly Available Clinical BERT Embeddings." NAACL-HLT (Clinical NLP Workshop).

4.Leveraging Natural Language Processing and Machine Learning Methods for Adverse Drug Event Detection in Electronic Health/Medical Records: A Scoping Review




