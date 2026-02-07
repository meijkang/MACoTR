# Multi-Agent Chain-of-Thought Reasoning for Verifiable Medical Report Generation

---

[![Status](https://img.shields.io/badge/Manuscript-IEEE%20TMI%20Style-orange)]()
[![Framework](https://img.shields.io/badge/PyTorch-2.x-ee4c2c)](https://pytorch.org/)
[![Backbone](https://img.shields.io/badge/VLM-Qwen2.5--VL--3B-6a5acd)]()
[![Language](https://img.shields.io/badge/Python-3.8%2B-3776ab)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)

**MACoTR** is a unified multi-agent chain-of-thought framework for **verifiable medical report generation**.  
It integrates verifiable reasoning construction and reasoning-aware optimization into a single pipeline, enabling reports whose diagnostic statements are supported by traceable reasoning evidence.

> 📌 Paper repository: `https://github.com/meijkang/MACoTR`

---

## ✨ Highlights (Contributions)

- 🔁 **Unified MACoTR framework** that combines:
  - **TRAC** for verifiable reasoning data construction
  - **DAL** for progressive model optimization
- ✅ **TRAC (Ternary Reasoning Agent Checker)** enables expert-free CoT mining via a closed-loop multi-agent workflow with semantic/entity consistency filtering.
- 🧠 **DAL (Dual-Alignment Learning)** improves reasoning quality through:
  - **Domain Alignment** for medical-domain adaptation
  - **Causal Alignment** for reasoning–report causal consistency
- 🩺 **Consistent performance gains** on both language quality and clinical efficacy metrics on MIMIC-CXR and IU X-Ray.

---

## 🧭 1. Motivation

Conventional medical report generation often maps images directly to reports without explicit intermediate reasoning, making diagnostic conclusions difficult to verify.  
MACoTR addresses this by explicitly modeling intermediate reasoning and enforcing consistency between reasoning traces and final report statements.

<div align="center">
  <a href="assets/challenge.pdf"><strong>📄 Figure 1 (PDF)</strong></a>
  <br>
  <em>Medical report generation paradigm comparison: non-verifiable vs verifiable.</em>
</div>

---

## 🌀 2. MACoTR Method Overview

As described in the paper, MACoTR consists of two core parts:

### 2.1 TRAC for Expert-Free CoT Mining

TRAC builds high-quality verifiable reasoning-report supervision through a closed-loop process:
1. **Reasoning Phase**: sample candidate reasoning chains from a ground-truth report.
2. **Deduction Phase**: reconstruct reports conditioned on candidate chains.
3. **Verification Phase**: evaluate semantic/entity-level consistency with the ground-truth report.
4. **Retention**: keep high-consistency pairs as training supervision.

### 2.2 DAL for Grounded Causality

Built on TRAC data, DAL progressively optimizes the target model with two complementary stages:

- **Domain Alignment**  
  Aligns visual semantics, medical terminology, and clinical reporting conventions using `(Image, Reasoning, Report)` supervision.

- **Causal Alignment**  
  Optimizes the causal linkage between intermediate reasoning and final report (GRPO-style reward optimization with structure and text-quality rewards).

<div align="center">
  <a href="assets/framework.pdf"><strong>📄 Figure 3 (PDF)</strong></a>
  <br>
  <em>Overview of MACoTR: TRAC for verifiable CoT mining and DAL for grounded causal optimization.</em>
</div>

---

## 🧩 3. Inference

At inference time, the trained medical reasoning agent generates:
- `<think>`: explicit intermediate reasoning trace
- `<answer>`: final medical report

This provides verifiable evidence paths from observations to conclusions.

---

## 📈 4. Experimental Snapshot

### MIMIC-CXR
- **BLEU-1 / BLEU-4 / METEOR / ROUGE-L**: `0.434 / 0.145 / 0.177 / 0.335`
- **Clinical P / R / F1**: `0.532 / 0.495 / 0.513`

### IU X-Ray
- **BLEU-1 / BLEU-4 / METEOR / ROUGE-L**: `0.510 / 0.189 / 0.216 / 0.399`

> The paper reports consistent improvements over diverse baselines in both textual quality and clinical effectiveness.

---
