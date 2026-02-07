# Multi-Agent Chain-of-Thought Reasoning for Verifiable Medical Report Generation

---

[![Status](https://img.shields.io/badge/Manuscript-IEEE%20TMI%20Style-orange)]()
[![Framework](https://img.shields.io/badge/PyTorch-2.x-ee4c2c)](https://pytorch.org/)
[![Backbone](https://img.shields.io/badge/VLM-Qwen2.5--VL--3B-6a5acd)]()
[![Language](https://img.shields.io/badge/Python-3.8%2B-3776ab)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)

**MACoTR** is a multi-agent chain-of-thought framework for **verifiable medical report generation**.  
It first builds high-quality verifiable reasoning data via **TRAC** (Ternary Reasoning Agent Checker), then transfers this supervision into an image-grounded medical reasoning agent through **DAL** (Dual-Alignment Learning), enabling reports with traceable evidence chains.

> 📌 Paper repository (as provided by the manuscript): `https://github.com/meijkang/MACoTR`

---

## ✨ Highlights

> **MACoTR** unifies verifiable reasoning construction and reasoning-aware optimization in one pipeline, improving both traceability and clinical consistency.

- 🔁 **TRAC closed-loop reasoning mining**: Reasoning Agent → Deduction Agent → Verification Agent  
- ✅ **Consistency-based filtering**: Retains high-quality reasoning-report pairs by semantic consistency score  
- 🧠 **Two-stage DAL optimization**: Domain Alignment + Causal Alignment  
- 🩺 **Higher clinical reliability**: Reduces hallucinations and logical drift, improves clinical entity consistency  

---

## 🧭 1. Motivation: From Black-Box Generation to Verifiable Generation

Conventional medical report generation often maps images directly to text, with limited explicit intermediate reasoning, making conclusions hard to trace or verify.  
MACoTR aims to make each diagnostic statement supported by an inspectable reasoning chain, enabling **auditable and verifiable** report generation.

<div align="center">
  <img src="assets/fig1_paradigm_comparison.png" width="92%"/>
  <br>
  <em>Medical report generation paradigm comparison: non-verifiable vs verifiable.</em>
</div>

---

## 🌀 2. TRAC: Expert-Free CoT Mining

TRAC automatically constructs verifiable CoT data without extra expert annotation:

1. **Reasoning Phase**  
   Given a GT report, sample multiple candidate reasoning chains.
2. **Deduction Phase**  
   Reconstruct reports from candidate chains under clinical writing conventions.
3. **Verification Phase**  
   Evaluate entity-level consistency between reconstructed reports and GT reports.
4. **Retention**  
   Keep candidates above a threshold (with iterative re-sampling when needed).

<div align="center">
  <img src="assets/fig3_framework_overview.png" width="92%"/>
  <br>
  <em>TRAC closed-loop workflow for verifiable CoT construction.</em>
</div>

---

## 🔥 3. DAL: Grounded Causality Alignment

On top of TRAC-generated data, DAL optimizes the model in two stages:

### 3.1 Domain Alignment (D-A)
- Supervised learning on `(Image, Reasoning, Report)` triplets  
- Aligns medical terminology, reporting style, and visual semantics  
- Provides a robust in-domain initialization

### 3.2 Causal Alignment (C-A)
- Group-relative advantage style optimization (GRPO-like)  
- Reward combines format validity + BLEU-4 + ROUGE-L  
- Strengthens causal consistency from intermediate reasoning to final report

---

## 🧩 4. Agent Inference

At inference time, the agent outputs separable components:
- `<think>`: intermediate reasoning chain (traceable)
- `<answer>`: final report (verifiable)

This upgrades the system from “answer-only” generation to “answer + auditable reasoning evidence”.

---

## 📈 5. Experimental Snapshot

### MIMIC-CXR
- **BLEU-1 / BLEU-4 / METEOR / ROUGE-L**: `0.434 / 0.145 / 0.177 / 0.335`
- **Clinical P / R / F1**: `0.532 / 0.495 / 0.513`

### IU X-Ray
- **BLEU-1 / BLEU-4 / METEOR / ROUGE-L**: `0.510 / 0.189 / 0.216 / 0.399`

> The paper reports consistent improvements over diverse baselines in both textual quality and clinical effectiveness.

---

## 🛠️ 6. Suggested Project Structure

```text
MACoTR/
├── assets/
│   ├── fig1_paradigm_comparison.png
│   └── fig3_framework_overview.png
├── configs/
│   ├── trac.yaml
│   ├── domain_alignment.yaml
│   └── causal_alignment.yaml
├── data/
│   ├── mimic_cxr/
│   └── iu_xray/
├── src/
│   ├── trac/
│   ├── dal/
│   ├── models/
│   └── train.py
├── scripts/
│   ├── build_trac_data.sh
│   ├── train_domain_alignment.sh
│   └── train_causal_alignment.sh
├── requirements.txt
└── README.md
