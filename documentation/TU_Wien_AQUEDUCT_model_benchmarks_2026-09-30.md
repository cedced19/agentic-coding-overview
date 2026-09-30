---
title: "TU Wien / AQUEDUCT model benchmark comparison"
date: "2026-09-30"
research_cutoff: "2026-09-30"
purpose: "Agent-readable comparison of TU Wien/AQUEDUCT-side models against current proprietary frontier models"
higher_is_better: true
notes:
  - "N/A means no exact-model public result was located; it does not mean zero."
  - "Do not directly compare different benchmark versions, harnesses, or reasoning-effort settings."
  - "Grok 4.8 was requested, but no released xAI model with that name was found as of 2026-09-30. Grok 4.7 is used instead."
---

# TU Wien / AQUEDUCT model benchmark comparison

Research cutoff: **30 September 2026**

## Model-name normalization

| Requested name | Model used | Note |
|---|---|---|
| DeepSeek V4 Flash | **DeepSeek V4 Flash 0731** | July 31 post-trained revision when revision-specific data are available |
| Qwen 3.6 35B | **Qwen3.6-35B-A3B** | Exact 35B-class model |
| DeepSeek V4 Vision | **DeepSeek-V4-Flash-Vision-Exp** | Experimental multimodal V4 Flash |
| GLM-5.3 Flash | **GLM-5.3-Flash** | Flash model, not full GLM-5.3 |
| GPT Sol 5.6 | **GPT-5.6 Sol** | Prefer max/highest public reasoning result |
| Claude Opus 5.5 | **Claude Opus 5.5** | Prefer max/xhigh when available |
| Grock 4.8 | **Grok 4.7** | **Substitution:** no released Grok 4.8 found; Grok 4.7 released 2026-09-21 |
| Gemini 3.8 Flash | **Gemini 3.8 Flash** | Prefer high reasoning when available |

> **AQUEDUCT note:** the first four are the TU Wien-side models requested for comparison. This document does not claim that all four are currently deployed on AQUEDUCT. Live availability should be checked from AQUEDUCT `/v1/models`.

---

# Benchmark selection

| Requested category | Benchmark selected | Rationale |
|---|---|---|
| Academic Writing | **GDPval-AA v2.1** | Direct academic-writing benchmarks such as WritingBench do not have clean public coverage for all eight exact models. GDPval-AA is used as a **written knowledge-work / deliverable proxy**. |
| DeepSWE | **DeepSWE v1.1** | Direct requested benchmark |
| Agentic Coding | **Terminal-Bench 4.0** + **Terminal-Bench 2.1 cross-check** | v4.0 is newer/harder; v2.1 has stronger coverage for several open models |
| Scientific benchmark | **Humanity's Last Exam (HLE)** + **GPQA Diamond cross-check** | HLE gives broad expert academic coverage; GPQA is specifically graduate-level physics/biology/chemistry |

---

# 1. Academic-writing proxy — GDPval-AA v2.1

**Metric:** Elo, higher is better.  
**Evaluator:** Artificial Analysis.

GDPval-AA is not an academic-paper-writing benchmark. It evaluates complex professional tasks that often require finished documents and knowledge-work artifacts, so it is used here as the closest current **cross-model writing/knowledge-work proxy** with coverage of all eight exact models.

| Model | GDPval-AA v2.1 Elo | Configuration / provenance |
|---|---:|---|
| **DeepSeek V4 Flash 0731** | **1427** | Artificial Analysis, reasoning, max |
| **Qwen3.6-35B-A3B** | **879** | Artificial Analysis, reasoning |
| **DeepSeek V4 Flash Vision** | **1534** | Artificial Analysis, reasoning, max |
| **GLM-5.3-Flash** | **1641** | Artificial Analysis |
| **GPT-5.6 Sol** | **1588** | Artificial Analysis, max |
| **Claude Opus 5.5** | **1846** | Artificial Analysis, adaptive reasoning, max, default fallback |
| **Grok 4.7** | **1695** | Artificial Analysis, xhigh |
| **Gemini 3.8 Flash** | **1412** | Artificial Analysis, high |

### Interpretation

This is the **most internally comparable table in this note**, because all eight values come from the same current benchmark version and evaluator.

It should still be treated as evidence for **professional written/knowledge work**, not specifically for:
- scientific-paper structure,
- citation accuracy,
- literature review quality,
- LaTeX output,
- academic style.

Direct writing benchmark worth monitoring:
- WritingBench: https://github.com/X-PLUG/WritingBench

Primary source:
- https://artificialanalysis.ai/evaluations/gdpval-aa

---

# 2. DeepSWE v1.1 — long-horizon software engineering

**Metric:** percent resolved, higher is better.

| Model | DeepSWE v1.1 | Provenance / harness note |
|---|---:|---|
| **DeepSeek V4 Flash 0731** | **54.4%** | DeepSeek self-report; DeepSeek Harness minimal mode, max effort |
| **Qwen3.6-35B-A3B** | **N/A** | No exact DeepSWE v1.1 public result located |
| **DeepSeek V4 Flash Vision** | **59.3%** | DeepSeek model card |
| **GLM-5.3-Flash** | **63.4%** | Model-card / DeepSWE evaluation metadata |
| **GPT-5.6 Sol** | **72.7%** | Public benchmark result |
| **Claude Opus 5.5** | **74.2%** | Anthropic system-card result; five-trial average |
| **Grok 4.7** | **71.0%** | xAI self-report, high effort |
| **Gemini 3.8 Flash** | **73.8%** | DataCurve / mini-swe-agent public result; Google reports 73.7% |

### Qwen fallback evidence — not the same benchmark

The Qwen model card reports:

| Alternative coding benchmark | Qwen3.6-35B-A3B |
|---|---:|
| SWE-bench Verified | **73.4%** |
| SWE-bench Pro | **49.5%** |
| Terminal-Bench 2.0 | **51.5%** |

These must **not** be substituted for a DeepSWE score.

### Comparability warning

DeepSWE performance depends on the coding harness and reasoning budget. Some entries use `mini-swe-agent`, others use vendor harnesses. Small differences should not be treated as pure model-quality differences.

Sources:
- DeepSWE leaderboard: https://deepswe.datacurve.ai/
- DeepSeek V4 Flash update: https://api-docs.deepseek.com/updates/
- DeepSeek V4 Vision: https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-Vision-Exp
- GLM-5.3-Flash: https://huggingface.co/zai-org/GLM-5.3-Flash
- Qwen3.6-35B-A3B: https://huggingface.co/Qwen/Qwen3.6-35B-A3B
- Gemini 3.8 Flash: https://deepmind.google/models/model-cards/gemini-3-8-flash/
- Grok 4.7: https://x.ai/news/grok-4-7
- Claude Opus: https://www.anthropic.com/claude/opus
- GPT-5.6: https://openai.com/index/gpt-5-6/

---

# 3. Agentic coding — Terminal-Bench 4.0

**Metric:** task pass rate, higher is better.  
**Primary cross-model evaluator:** Artificial Analysis.

Terminal-Bench 4.0 is a harder 66-task benchmark covering software, ML, systems, science, operations, security, hardware, and media.

| Model | Terminal-Bench 4.0 | Configuration / caveat |
|---|---:|---|
| **DeepSeek V4 Flash 0731** | **~12%** | Artificial Analysis, max reasoning |
| **Qwen3.6-35B-A3B** | **N/A** | No exact v4.0 result located; Qwen publishes TB **2.0 = 51.5%**, not comparable |
| **DeepSeek V4 Flash Vision** | **~12%** | Artificial Analysis, max reasoning |
| **GLM-5.3-Flash** | **~33%** | Artificial Analysis; current value about 32.8–33% |
| **GPT-5.6 Sol** | **~40%** | Artificial Analysis, max; current value about 39.9% |
| **Claude Opus 5.5** | **59.6%** | Artificial Analysis with fallback. Anthropic reports **66.4%** in its own xhigh setup |
| **Grok 4.7** | **~26%** | Artificial Analysis xhigh. xAI reports **37.6–38.0%** with Grok Build |
| **Gemini 3.8 Flash** | **~20%** | Artificial Analysis high; public benchmark material reports about 19.1% |

### Why the harness matters

The gap between common-evaluator and vendor/system scores is evidence that an agentic-coding result measures a **model + harness + tools + reasoning budget** system, not the base model in isolation.

Examples:
- Grok 4.7: roughly **26%** in an Artificial Analysis setup vs **~38%** in xAI's Grok Build setup.
- Claude Opus 5.5: **59.6%** in Artificial Analysis vs **66.4%** in Anthropic's xhigh setup.

Sources:
- Artificial Analysis Terminal-Bench 4.0: https://artificialanalysis.ai/evaluations/terminalbench-4-0
- Terminal-Bench: https://www.tbench.ai/
- Anthropic Opus: https://www.anthropic.com/claude/opus
- xAI Grok 4.7: https://x.ai/news/grok-4-7
- Google Gemini 3.8 Flash: https://deepmind.google/models/model-cards/gemini-3-8-flash/

---

# 3b. Agentic-coding cross-check — Terminal-Bench 2.1

Terminal-Bench 2.1 is included because it has better direct published coverage for several TU Wien-side models.

| Model | Terminal-Bench 2.1 | Source / note |
|---|---:|---|
| **DeepSeek V4 Flash 0731** | **82.7%** | DeepSeek, DeepSeek Harness minimal mode |
| **Qwen3.6-35B-A3B** | **N/A** | Qwen publishes TB 2.0 = 51.5%, not 2.1 |
| **DeepSeek V4 Flash Vision** | **83.9%** | DeepSeek model card |
| **GLM-5.3-Flash** | **84.3%** | Public model-card/evaluation result |
| **GPT-5.6 Sol** | **88.8%** | Public frontier comparison |
| **Claude Opus 5.5** | **N/A exact 2.1 result located** | Current Anthropic material focuses on v4.0 |
| **Grok 4.7** | **N/A** | xAI publishes v4.0 for Grok 4.7 |
| **Gemini 3.8 Flash** | **89.4%** | Google model card |

**Important:** Terminal-Bench 2.0, 2.1 and 4.0 are not interchangeable benchmark versions.

Sources:
- https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-Vision-Exp
- https://huggingface.co/zai-org/GLM-5.3-Flash
- https://deepmind.google/models/model-cards/gemini-3-8-flash/
- https://huggingface.co/Qwen/Qwen3.6-35B-A3B

---

# 4. Scientific / expert academic reasoning — Humanity's Last Exam

**Metric:** pass@1 accuracy, higher is better.  
**Primary evaluator:** Artificial Analysis, using a **2,158-question text-only subset** for comparability.

HLE is not science-only. It spans mathematics, natural sciences and humanities, so it is a broad **expert academic reasoning** benchmark.

| Model | HLE | Provenance / configuration |
|---|---:|---|
| **DeepSeek V4 Flash 0731** | **~39%** | Artificial Analysis, max |
| **Qwen3.6-35B-A3B** | **21.4%** | Qwen model-card result |
| **DeepSeek V4 Flash Vision** | **~34%** | Artificial Analysis, max |
| **GLM-5.3-Flash** | **~40%** | Artificial Analysis |
| **GPT-5.6 Sol** | **~49%** | Artificial Analysis, max |
| **Claude Opus 5.5** | **61.4%** | Artificial Analysis, adaptive reasoning, max, default fallback |
| **Grok 4.7** | **~43%** | Artificial Analysis, xhigh |
| **Gemini 3.8 Flash** | **~48%** | Artificial Analysis, high |

Sources:
- Artificial Analysis HLE: https://artificialanalysis.ai/evaluations/humanitys-last-exam
- Qwen model card: https://huggingface.co/Qwen/Qwen3.6-35B-A3B

---

# 4b. Science-specific cross-check — GPQA Diamond

GPQA Diamond contains **198 graduate-level questions in physics, biology and chemistry**. It is more specifically scientific than HLE, but exact coverage is incomplete for the newest models.

| Model | GPQA Diamond | Evidence |
|---|---:|---|
| **DeepSeek V4 Flash 0731** | **90.8%** | Artificial Analysis |
| **Qwen3.6-35B-A3B** | **86.0%** | Qwen / Hugging Face evaluation |
| **DeepSeek V4 Flash Vision** | **91.3%** | Artificial Analysis |
| **GLM-5.3-Flash** | **91.2%** | Artificial Analysis |
| **GPT-5.6 Sol** | **94.1% AA / 94.6% OpenAI** | Different evaluator setups retained explicitly |
| **Claude Opus 5.5** | **N/A clean AA value located** | A third-party OpenRouter run is around 90.6%, but not used as the primary comparable value |
| **Grok 4.7** | **N/A** | No exact current GPQA Diamond result located at cutoff |
| **Gemini 3.8 Flash** | **95.3%** | Artificial Analysis, high |

Sources:
- Artificial Analysis GPQA: https://artificialanalysis.ai/evaluations/gpqa-diamond
- Qwen3.6: https://huggingface.co/Qwen/Qwen3.6-35B-A3B
- OpenAI GPT-5.6: https://openai.com/index/gpt-5-6/

---

# Compact benchmark matrix

| Model | GDPval-AA v2.1 | DeepSWE v1.1 | Terminal-Bench 4.0 | HLE | GPQA Diamond |
|---|---:|---:|---:|---:|---:|
| **DeepSeek V4 Flash 0731** | 1427 | 54.4% | ~12% | ~39% | 90.8% |
| **Qwen3.6-35B-A3B** | 879 | N/A | N/A | 21.4% | 86.0% |
| **DeepSeek V4 Flash Vision** | 1534 | 59.3% | ~12% | ~34% | 91.3% |
| **GLM-5.3-Flash** | 1641 | 63.4% | ~33% | ~40% | 91.2% |
| **GPT-5.6 Sol** | 1588 | 72.7% | ~40% | ~49% | 94.1% AA / 94.6% OpenAI |
| **Claude Opus 5.5** | 1846 | 74.2% | 59.6% AA | 61.4% | N/A clean AA |
| **Grok 4.7** | 1695 | 71.0% | ~26% AA | ~43% | N/A |
| **Gemini 3.8 Flash** | 1412 | 73.8% | ~20% | ~48% | 95.3% |

---

# Interpretation rules for an agent

## Safe conclusions

- **GDPval-AA v2.1** is the cleanest common-evaluator comparison in this file.
- Agentic-coding scores are strongly dependent on the **harness**, tools and reasoning budget.
- `DeepSeek-V4-Flash-Vision-Exp` improves over `DeepSeek V4 Flash 0731` on vendor-reported DeepSWE and Terminal-Bench 2.1, but not on every benchmark.
- `GLM-5.3-Flash` has substantially stronger current public agentic-coding evidence than `Qwen3.6-35B-A3B`.
- Missing Qwen results on newer benchmarks are **coverage gaps**, not zero scores.

## Do not do this

An agent should not:
1. average Elo and percentages into a single composite score;
2. compare Terminal-Bench 2.0, 2.1 and 4.0 as if they were the same test;
3. treat vendor-reported and independent-evaluator runs as identical;
4. infer a missing score from another model in the same family;
5. replace `GLM-5.3-Flash` with full `GLM-5.3`;
6. replace `DeepSeek V4 Flash 0731` with `DeepSeek V4.1 Flash`;
7. assume `Grok 4.8` exists unless xAI releases it after this research cutoff.

---

# Source index

## Benchmark sources

- Artificial Analysis — GDPval-AA v2.1  
  https://artificialanalysis.ai/evaluations/gdpval-aa

- Artificial Analysis — Terminal-Bench 4.0  
  https://artificialanalysis.ai/evaluations/terminalbench-4-0

- Artificial Analysis — Humanity's Last Exam  
  https://artificialanalysis.ai/evaluations/humanitys-last-exam

- Artificial Analysis — GPQA Diamond  
  https://artificialanalysis.ai/evaluations/gpqa-diamond

- DeepSWE / DataCurve  
  https://deepswe.datacurve.ai/

- Terminal-Bench  
  https://www.tbench.ai/

- WritingBench  
  https://github.com/X-PLUG/WritingBench

## Model-primary sources

- TU Wien AQUEDUCT  
  https://datalab.tuwien.ac.at/aiml/aqueduct/

- TU Wien AQUEDUCT release notes  
  https://datalab.tuwien.ac.at/aiml/aqueduct/release-notes/

- DeepSeek V4 Flash API updates  
  https://api-docs.deepseek.com/updates/

- DeepSeek V4 Flash Vision  
  https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-Vision-Exp

- Qwen3.6-35B-A3B  
  https://huggingface.co/Qwen/Qwen3.6-35B-A3B

- GLM-5.3-Flash  
  https://huggingface.co/zai-org/GLM-5.3-Flash

- GPT-5.6  
  https://openai.com/index/gpt-5-6/

- Claude Opus  
  https://www.anthropic.com/claude/opus

- Grok 4.7  
  https://x.ai/news/grok-4-7

- Gemini 3.8 Flash  
  https://deepmind.google/models/model-cards/gemini-3-8-flash/

---

# Machine-readable summary

```yaml
research_cutoff: "2026-09-30"

aliases:
  deepseek_v4_flash: "DeepSeek V4 Flash 0731"
  qwen_3_6_35b: "Qwen3.6-35B-A3B"
  deepseek_v4_vision: "DeepSeek-V4-Flash-Vision-Exp"
  glm_5_3_flash: "GLM-5.3-Flash"
  gpt_sol_5_6: "GPT-5.6 Sol"
  claude_opus_5_5: "Claude Opus 5.5"
  requested_grock_4_8:
    normalized_to: "Grok 4.7"
    reason: "No released Grok 4.8 found at cutoff"
  gemini_3_8_flash: "Gemini 3.8 Flash"

scores:
  gdpval_aa_v2_1:
    deepseek_v4_flash: 1427
    qwen_3_6_35b: 879
    deepseek_v4_vision: 1534
    glm_5_3_flash: 1641
    gpt_5_6_sol: 1588
    claude_opus_5_5: 1846
    grok_4_7: 1695
    gemini_3_8_flash: 1412

  deepswe_v1_1_percent:
    deepseek_v4_flash: 54.4
    qwen_3_6_35b: null
    deepseek_v4_vision: 59.3
    glm_5_3_flash: 63.4
    gpt_5_6_sol: 72.7
    claude_opus_5_5: 74.2
    grok_4_7: 71.0
    gemini_3_8_flash: 73.8

  terminal_bench_4_0_percent_approx:
    deepseek_v4_flash: 12
    qwen_3_6_35b: null
    deepseek_v4_vision: 12
    glm_5_3_flash: 33
    gpt_5_6_sol: 40
    claude_opus_5_5: 59.6
    grok_4_7: 26
    gemini_3_8_flash: 20

  hle_percent_approx:
    deepseek_v4_flash: 39
    qwen_3_6_35b: 21.4
    deepseek_v4_vision: 34
    glm_5_3_flash: 40
    gpt_5_6_sol: 49
    claude_opus_5_5: 61.4
    grok_4_7: 43
    gemini_3_8_flash: 48

  gpqa_diamond_percent:
    deepseek_v4_flash: 90.8
    qwen_3_6_35b: 86.0
    deepseek_v4_vision: 91.3
    glm_5_3_flash: 91.2
    gpt_5_6_sol:
      artificial_analysis: 94.1
      openai: 94.6
    claude_opus_5_5: null
    grok_4_7: null
    gemini_3_8_flash: 95.3
```

---

# Refresh policy

Before using this file after September 2026:

1. check whether `Grok 4.8` has been released;
2. refresh the DeepSWE leaderboard;
3. refresh Artificial Analysis GDPval-AA, Terminal-Bench, HLE and GPQA;
4. preserve exact benchmark versions and reasoning settings;
5. preserve exact model revisions such as `0731`, `Vision-Exp`, and `Flash`;
6. prefer benchmark-author or independent-evaluator results over family-level substitutions.
