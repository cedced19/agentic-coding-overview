---
title: "Live check of sources for the refined deck"
date: "2026-09-30"
scope: "Facts added to 260930_presentation/ that are not in the other notes in this folder"
notes:
  - "Pages were read on 30 September 2026 through an automated fetch and summary. Recheck a figure at its source before quoting it elsewhere."
  - "Where this note and an older note disagree, this note reflects the live page on the day."
---

# Live check, 30 September 2026

## 1. AQUEDUCT overview page

Source: https://datalab.tuwien.ac.at/aiml/aqueduct/

The overview still lists four LLMs. It has not yet been updated for the migration described in the 17 September release note.

| Model id | Context | Hardware as listed | Replicas | Quantization |
|---|---:|---|---:|---|
| `deepseek-v4-flash-284b` | 393,216 | NVIDIA H200 | 1 | none, supports `high` and `max` reasoning effort |
| `qwen-3.6-35b` | 262,144 | NVIDIA A40, one per replica | 4 | AWQ |
| `qwen-3.5-397b` | 200,000 | 4x RTX PRO 6000 Blackwell per replica | 2 | GPTQ-Int4 |
| `glm-5.2-744b-preview` | 262,144 | 8x AMD Instinct MI300X | 1 | FP8 |

Differences from `TU_Wien_AQUEDUCT_models_hardware_2026-09-30.md`:

- That note treats DeepSeek V4 Flash as already on RTX PRO 6000 Blackwell. The live page still shows H200. The deck says "on H200, moving to Blackwell".
- That note treats Qwen 3.5 as retired. The live page still lists it on its retirement day. The deck shows it with "Retires today".
- Quantization per model was not in the older note.

Release notes (https://datalab.tuwien.ac.at/aiml/aqueduct/release-notes/), 7 August 2026: DeepSeek V4 Flash has no image input, unlike the Qwen models.

## 2. Access

Source: https://datalab.tuwien.ac.at/aiml/aqueduct/ and https://datalab.tuwien.ac.at/aiml/

- The service is described as available to "all researchers, lecturers and university staff".
- Login is through TU Wien SSO at https://aqueduct.ai.datalab.tuwien.ac.at/ where a personal API key is generated.
- API base: `https://aqueduct.ai.datalab.tuwien.ac.at/v1`, OpenAI-compatible.
- Contact: Matrix channel `#llm-service:tuwien.ac.at`.
- The public pages say nothing about students. The statement that students must be allowlisted comes from `plan.md`, not from the public page. **Who a student should ask is not documented. The deck says "your supervisor or the dataLAB team on Matrix", which should be confirmed.**

Integrations with setup guides (https://datalab.tuwien.ac.at/aiml/aqueduct/integrations/): Pi, OpenCode, Charmbracelet Crush, Zed.

Model compatibility report (https://datalab.tuwien.ac.at/aiml/aqueduct/model-compatibility/), dated 30 September 2026: tool calling passes on the Qwen models. DeepSeek fails the image-input tests and has trouble with streaming and structured output.

## 3. New frontier models

| Model | Released | API price per 1M tokens (input / output) |
|---|---|---|
| Claude Fable 5.1 | 1 September 2026 | $10 / $50 |
| GPT-6 Astra | 3 September 2026 | $10 / $50 |

Sources: https://artificialanalysis.ai/models/gpt-6-astra, https://artificialanalysis.ai/models/claude-fable-5-1, https://www.anthropic.com/claude/fable

## 4. Benchmark figures added to the charts

| Benchmark | GPT-6 Astra | Claude Fable 5.1 | Provenance |
|---|---:|---:|---|
| GDPval-AA v2.1 (Elo) | 1542 | 1735 | Artificial Analysis leaderboard, max effort |
| DeepSWE v1.1 | 74.1% | 67.4% | Both as published by OpenAI. The DataCurve leaderboard shows 74% for Astra (xhigh). Artificial Analysis measures 68% for Astra in its own setup. |
| Terminal-Bench 4.0 | ~59% | ~52% | Artificial Analysis setup. Vendors report 57.7% and 55.8%. |
| Terminal-Bench 2.1 | N/A | N/A | No result located |
| Humanity's Last Exam | N/A | 59.1% | Artificial Analysis text-only subset, max effort. OpenAI reports 57.2% for Astra with tools, which is a different setup. |
| GPQA Diamond | 96.1% | 93.7% | Astra from Artificial Analysis (max). Fable 5.1 as published by OpenAI. |

Other observations used in chart notes:

- Terminal-Bench 4.0 on Artificial Analysis: 66 tasks, mini-swe-agent harness, pass@1 over three repeats. Claude Sonnet 5.5 leads at 63.6%.
- DeepSWE leaderboard (updated 22 September 2026) reports error bars of roughly 3 to 4 points per model.

Sources:

- https://artificialanalysis.ai/evaluations/gdpval-aa
- https://artificialanalysis.ai/evaluations/terminalbench-4-0
- https://artificialanalysis.ai/evaluations/humanitys-last-exam
- https://artificialanalysis.ai/evaluations/gpqa-diamond
- https://artificialanalysis.ai/articles/benchmarking-gpt-6-astra
- https://deepswe.datacurve.ai/
- https://www.datacamp.com/blog/gpt-6-astra-vs-claude-fable-5-1 (secondary source that collects the vendor-published figures)

## 5. Not in any source

The hardware slide's rule of thumb (about 1 GB per billion parameters at 8 bit, half of that at 4 bit) is plain arithmetic by the authors, not a TU Wien statement.
