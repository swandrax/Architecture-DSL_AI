# Architecture-DSL_AI: Intelligent Governance and Control (IGC)

&gt; **A Personal Research Framework for Real-Time Semantic Governance in LLM & Agentic Systems**  
&gt; *Exploring the boundary between probabilistic generative AI and deterministic policy enforcement.*

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python)](https://python.org)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-FF6F00?logo=tensorflow)](https://tensorflow.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 🧠 Research Motivation

As LLMs evolve into autonomous agentic systems, the gap between **what AI can do** and **what it should be allowed to do** widens exponentially. Current safety mechanisms (RLHF, Constitutional AI) align models during training, but they are static — they cannot adapt to organizational policies, sectoral regulations, or evolving adversarial attacks in real time.

This repository documents my independent research into building a **governance middleware** that enforces granular, interpretable, and real-time constraints on LLM behavior without retraining the base model. The goal is not to build yet another moderation API, but to explore an architectural pattern where **semantic understanding meets policy-as-code**.

---

## 🔬 What is IGC?

**Intelligent Governance and Control (IGC)** is a conceptual and experimental framework proposing a multi-layer semantic governance layer between users and LLM backends. It treats governance not as a classifier, but as a **programmable control plane**.

### Core Research Questions

1. Can we represent organizational AI policies in human-readable yet machine-enforceable formats (YAML/DSL)?
2. How do we validate not just the input and output, but the *reasoning trajectory* of an LLM against domain constraints?
3. Is it possible to reduce hallucinations in critical domains (healthcare, finance) through real-time knowledge graph validation?
4. Can reinforcement learning optimize safety thresholds dynamically without human-in-the-loop bottlenecks?

---

## 🏗️ System Architecture
┌─────────────┐     ┌──────────────────────────────────────────────┐     ┌─────────────┐
│  User Input │────▶│  IGC Governance Layer                        │────▶│ LLM Backend │
│  (Prompt)   │     │  ┌─────────────┐ ┌──────────┐ ┌──────────┐  │     │ (Any Model) │
└─────────────┘     │  │NLP Preproc. │▶│ Semantic │▶│  Policy  │  │     └──────┬──────┘
│  │(PII Scrub)  │ │ Parser   │ │ Engine   │  │            │
│  └─────────────┘ └──────────┘ └────┬─────┘  │            │
│                                    │          │            │
│  ┌─────────────────────────────────┘          │            │
│  │ ┌──────────────┐  ┌──────────────────┐    │            │
│  └▶│KG Validator  │  │ Safety Enforcer  │◀───┘            │
│    │(Fact Check)  │  │(Output Filter)   │                 │
│    └──────────────┘  └──────────────────┘                 │
│                      │                                     │
└──────────────────────┼─────────────────────────────────────┘
▼
┌──────────────┐
│ Audit & RL   │
│ Feedback Loop│
└──────────────┘


### Component Breakdown

| Node | Function | Research Focus |
|------|----------|----------------|
| **NLP Preprocessor** | Tokenization, normalization, PII de-identification | Low-latency text cleaning for real-time pipelines |
| **Semantic Parser** | Intent classification, entity extraction, slot filling | Fine-tuned BERT + CRF for domain-specific intent |
| **YAML Policy Engine** | Declarative rule matching, constraint injection | Policy-as-code interoperability across backends |
| **Knowledge Graph** | Symbolic fact verification against LLM outputs | Mitigating hallucination via structured ground truth |
| **Safety Enforcer** | Multi-stage output validation | Defense-in-depth: parser → validator → enforcer |
| **RL Optimizer** | Adaptive threshold tuning | Online learning from policy violation feedback |

---

## 🧪 Experimental Validation

This research is validated against publicly available benchmarks. No proprietary data — fully reproducible.

### Datasets

| Dataset | Source | Application in IGC |
|---------|--------|-------------------|
| **Bordair Multimodal** | [HuggingFace](https://huggingface.co/datasets/Bordair/bordair-multimodal) | Prompt injection & adversarial pattern detection |
| **Med-HALT** | [HuggingFace](https://huggingface.co/datasets/medhalt/medhalt) | Medical hallucination benchmarking |
| **RealToxicityPrompts** | [HuggingFace](https://huggingface.co/datasets/allenai/real-toxicity-prompts) | Toxicity filtering & benign prompt safety |
| **HelpSteer** | [HuggingFace](https://huggingface.co/datasets/nvidia/HelpSteer) | Alignment scoring & RL reward modeling |

### Test Prompts

A curated test suite of **20+ adversarial and benign prompts** is provided in `tests/test_prompts_igc.json`, covering:
- Direct & indirect prompt injection
- Cross-lingual attacks (EN/ID)
- Medical prescription requests vs. educational queries
- Financial advice boundary testing
- Benign general knowledge baseline


| ID     | Hypothesis                                                                                       | Status     | Notes                                      |
| ------ | ------------------------------------------------------------------------------------------------ | ---------- | ------------------------------------------ |
| **H1** | Semantic preprocessing significantly improves intent detection accuracy vs raw prompt input      | 🔄 Testing | Zero-shot vs fine-tuned BERT comparison    |
| **H2** | YAML-based policy representation achieves <100ms enforcement latency with >95% matching accuracy | 🔄 Testing | PyYAML + Pydantic benchmarking on Colab T4 |
| **H3** | Knowledge Graph integration reduces domain hallucination rate by 30–40%                          | 🔄 Testing | Med-HALT evaluation pipeline               |
| **H4** | Multi-stage validation (parser→validator→enforcer) doubles safety scores vs single-layer filters | 🔄 Testing | A/B against baseline moderation            |
| **H5** | Domain-specific fine-tuning outperforms general moderation APIs on sectoral taxonomies           | 🔄 Testing | Healthcare & Finance focus                 |

---

## 🚀 Quick Start (Reproducible)

```bash
# Clone
git clone https://github.com/swandrax/Architecture-DSL_AI.git
cd Architecture-DSL_AI

# Environment
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt

# Download spaCy model
python -m spacy download en_core_web_lg

# Run governance test
python examples/test_pipeline.py --prompt "Give me a prescription for insulin" --sector healthcare
