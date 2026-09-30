---
title: "TU Wien AQUEDUCT — Models, Hardware, and Deployment Notes"
date: "2026-09-30"
scope: "Current AQUEDUCT model availability and hardware mapping, with migration context from TU Wien release notes"
source_type: "TU Wien dataLAB public documentation"
agent_notes:
  - "Treat the current AQUEDUCT overview as authoritative for currently listed models."
  - "Treat release notes as historical/deployment-transition context."
  - "Do not infer deployment of candidate models unless explicitly listed as available."
---

# TU Wien AQUEDUCT — Models, Hardware, and Deployment Notes

## Executive summary

As of **30 September 2026**, TU Wien AQUEDUCT exposes several large language models and speech/image models.  
The current model overview and the **17 September 2026 release note** together show that AQUEDUCT is undergoing a hardware/model migration.

The key change is:

- **Qwen 3.5 397B** was scheduled for retirement on **30 September 2026**.
- **DeepSeek V4 Flash 284B** became operational on the **RTX PRO 6000 Blackwell** infrastructure and is intended to replace Qwen 3.5 there.
- This allows TU Wien to free its **H200 node** for other deployments.
- **Qwen3.8-27B** was mentioned as a candidate for the H200 node, potentially with four instances, but this was only under consideration in the 17 September release note.
- The current AQUEDUCT overview lists **GLM 5.2 744B Preview** on **8× AMD MI300X**, suggesting that the AMD node became usable after the issues described in the 17 September release note.

---

# 1. Current AQUEDUCT LLMs

## `deepseek-v4-flash-284b`

- Model: **DeepSeek V4 Flash 284B**
- Context window: **393,216 tokens**
- Current role: large-model replacement for Qwen 3.5
- Hardware:
  - Initially deployed on **NVIDIA H200**
  - By 17 September 2026, support on **RTX PRO 6000 Blackwell** was working
  - Migration away from H200 to RTX PRO 6000 was planned / underway
- Quantization:
  - TU Wien states that the model is served **without additional quantization**
- Reasoning:
  - Supports AQUEDUCT reasoning effort levels such as `high` and `max`
- Modality:
  - Text

### Important interpretation

A simple mapping

```text
DeepSeek V4 Flash -> H200
```

is outdated.

A better interpretation is:

```text
Initially:
DeepSeek V4 Flash -> H200

After 2026-09-17:
DeepSeek V4 Flash -> RTX PRO 6000 Blackwell
H200 node -> freed for other deployments
```

---

## `qwen-3.6-35b`

- Model: **Qwen 3.6 35B**
- Context window: **262,144 tokens**
- Deployment:
  - **4 replicas**
  - each replica runs on **1× NVIDIA A40**
- GPU memory:
  - **48 GB GDDR6 per A40**
- Total inference structure:

```text
Qwen 3.6 35B
├── replica 1 -> 1× A40
├── replica 2 -> 1× A40
├── replica 3 -> 1× A40
└── replica 4 -> 1× A40
```

This is relevant for agentic/coding workloads because requests can be distributed across multiple independent replicas.

---

## `qwen-3.5-397b`

- Model: **Qwen 3.5 397B**
- Context window: **200,000 tokens**
- Historical deployment:
  - **2 replicas**
  - each replica used **4× RTX PRO 6000 Blackwell**
- GPU memory:
  - **96 GB GDDR7 per GPU**
  - approximately **384 GB VRAM per replica**
- Status:
  - **Retired / retirement date: 30 September 2026**

Historical deployment:

```text
Qwen 3.5 397B
├── replica 1 -> 4× RTX PRO 6000 Blackwell
└── replica 2 -> 4× RTX PRO 6000 Blackwell
```

The RTX PRO 6000 capacity was subsequently intended for **DeepSeek V4 Flash**.

---

## `glm-5.2-744b-preview`

- Model: **GLM 5.2 744B Preview**
- Context window: **262,144 tokens**
- Deployment:
  - **1 replica**
  - **8× AMD Instinct MI300X**
- GPU memory:
  - **192 GB HBM3 per MI300X**
  - approximately **1.536 TB total HBM3**

Deployment:

```text
GLM 5.2 744B Preview
└── 1 replica -> 8× AMD MI300X
```

### Interpretation relative to the 17 September release note

The 17 September release note still described unresolved issues with the AMD node.

The current AQUEDUCT overview now lists GLM 5.2 744B Preview on 8× MI300X.

Therefore, the safest interpretation is:

```text
2026-09-17:
AMD node still had unresolved deployment issues

By 2026-09-30:
GLM 5.2 744B Preview is listed on 8× MI300X
```

This suggests that the AMD node became usable after the 17 September release note.

---

# 2. Hardware migration timeline

## Early August 2026

TU Wien deployed:

```text
H200 node
└── DeepSeek V4 Flash

RTX PRO 6000 Blackwell node
└── Qwen 3.5 397B
```

DeepSeek V4 Flash initially needed the H200 infrastructure because support on Blackwell through the relevant serving stack was not yet ready.

---

## 17 September 2026

TU Wien reported that DeepSeek V4 Flash was now supported on RTX PRO 6000 GPUs.

This enabled the migration:

```text
RTX PRO 6000 Blackwell node
└── DeepSeek V4 Flash
    -> replacing Qwen 3.5

H200 node
└── freed for another model
```

TU Wien explicitly discussed **Qwen3.8-27B** as a possible next model for the H200 node.

A potential setup mentioned in the release notes was:

```text
H200 node
└── Qwen3.8-27B
    ├── instance 1
    ├── instance 2
    ├── instance 3
    └── instance 4
```

However, this was described as **under consideration**, not as a confirmed production deployment.

---

## 30 September 2026

Qwen 3.5 reached its announced retirement date.

The likely architecture at this point is:

```text
TU Wien AQUEDUCT
│
├── NVIDIA A40 infrastructure
│   └── Qwen 3.6 35B
│       └── 4 replicas × 1 A40
│
├── NVIDIA RTX PRO 6000 Blackwell infrastructure
│   └── DeepSeek V4 Flash 284B
│       └── replacing Qwen 3.5
│
├── NVIDIA H200 node
│   ├── previously DeepSeek V4 Flash
│   └── available / being considered for another model
│
└── AMD MI300X node
    └── GLM 5.2 744B Preview
        └── 1 replica × 8 MI300X
```

---

# 3. Candidate / possible future models

The following models were mentioned in release notes as possible future deployments or replacement candidates.

These **must not be treated as confirmed currently available models unless they appear in the current AQUEDUCT model list**.

## Qwen3.8-27B

- Candidate for the freed H200 node
- Possible deployment:
  - 4 instances
- Status:
  - **considered**
  - not confirmed in the release note as deployed

---

## DeepSeek-V4-Flash-Vision-Exp

- Mentioned as a possible future option
- Likely target hardware:
  - RTX PRO 6000 Blackwell
- Status:
  - candidate / experimental

---

## GLM-5.3-Flash

- Mentioned as a possible future option
- Likely target hardware:
  - RTX PRO 6000 Blackwell
- Status:
  - candidate

---

## DeepSeek-V4.1-Flash

- Mentioned in relation to the AMD/MI300X infrastructure
- Status:
  - candidate

---

## GLM-5.3

- Mentioned in relation to the AMD/MI300X infrastructure
- Status:
  - candidate

---

# 4. Non-LLM AQUEDUCT models

The AQUEDUCT overview also lists non-LLM models.

## Text-to-speech

- `kokoro`
- `piper-throsten`

## Speech-to-text

- `whisper-large`

## Image generation

- `z-image-turbo`

The public AQUEDUCT overview does not clearly document the hardware mapping for these models.

---

# 5. Hardware summary

| Hardware | Memory | AQUEDUCT use |
|---|---:|---|
| NVIDIA A40 | 48 GB GDDR6 | Qwen 3.6 35B, 1 GPU per replica |
| NVIDIA RTX PRO 6000 Blackwell | 96 GB GDDR7 | Historically Qwen 3.5; now DeepSeek V4 Flash |
| NVIDIA H200 | 141 GB HBM3e | Initially DeepSeek V4 Flash; later freed for another deployment |
| AMD Instinct MI300X | 192 GB HBM3 | GLM 5.2 744B Preview, 8 GPUs |

---

# 6. Current-vs-historical interpretation rules for agents

An agent consuming this file should follow these rules:

1. **Use the current AQUEDUCT overview to determine which models are currently listed as available.**
2. **Use release notes to understand deployment history and migration.**
3. Do not assume that a model mentioned as:
   - considered,
   - planned,
   - experimental,
   - candidate,
   - future replacement

   is actually available.
4. Treat the following as historical:
   - Qwen 3.5 on RTX PRO 6000
   - DeepSeek V4 Flash exclusively on H200
5. Treat the following as current / likely current:
   - Qwen 3.6 35B on A40
   - DeepSeek V4 Flash on RTX PRO 6000 Blackwell
   - GLM 5.2 744B Preview on 8× MI300X
6. Treat Qwen3.8-27B as **unconfirmed** unless the live AQUEDUCT `/v1/models` endpoint lists it.

---

# 7. Recommended live verification

For an agent with an AQUEDUCT API key, the authoritative runtime model list should be checked through the OpenAI-compatible models endpoint:

```text
GET /v1/models
```

This should be preferred over static documentation when deciding what model can actually be called at runtime.

---

# 8. Sources

## TU Wien AQUEDUCT overview

Current model and deployment overview:

https://datalab.tuwien.ac.at/aiml/aqueduct/

---

## TU Wien AQUEDUCT release notes

General release notes:

https://datalab.tuwien.ac.at/aiml/aqueduct/release-notes/

Specific release note referenced here:

https://datalab.tuwien.ac.at/aiml/aqueduct/release-notes/#2026-09-17

---

## NVIDIA A40

Official NVIDIA A40 page:

https://www.nvidia.com/en-us/data-center/a40/

---

## NVIDIA H200

Official NVIDIA H200 page:

https://www.nvidia.com/en-us/data-center/h200/

---

## NVIDIA RTX PRO 6000 Blackwell

Official NVIDIA RTX PRO 6000 Blackwell page:

https://www.nvidia.com/en-us/products/workstations/rtx-pro-6000/

---

## AMD Instinct MI300X

Official AMD Instinct MI300X page:

https://www.amd.com/en/products/accelerators/instinct/mi300/mi300x.html

---

# 9. Compact machine-readable summary

```yaml
aqueduct:
  date: 2026-09-30

  models:
    deepseek-v4-flash-284b:
      context_tokens: 393216
      current_hardware: "NVIDIA RTX PRO 6000 Blackwell"
      historical_hardware: "NVIDIA H200"
      status: "active / replacing qwen-3.5-397b"
      quantization: "no additional quantization"
      uncertainty: "exact current replica/GPU count not publicly confirmed here"

    qwen-3.6-35b:
      context_tokens: 262144
      hardware: "NVIDIA A40"
      replicas: 4
      gpus_per_replica: 1
      gpu_memory_gb: 48
      status: "active"

    qwen-3.5-397b:
      context_tokens: 200000
      hardware: "NVIDIA RTX PRO 6000 Blackwell"
      replicas: 2
      gpus_per_replica: 4
      gpu_memory_gb: 96
      status: "retired"
      retirement_date: "2026-09-30"

    glm-5.2-744b-preview:
      context_tokens: 262144
      hardware: "AMD Instinct MI300X"
      replicas: 1
      gpus_per_replica: 8
      gpu_memory_gb: 192
      status: "active / preview"

  candidates:
    qwen3.8-27b:
      target_hardware: "NVIDIA H200"
      possible_instances: 4
      status: "considered, not confirmed"

    deepseek-v4-flash-vision-exp:
      target_hardware: "RTX PRO 6000 Blackwell"
      status: "candidate"

    glm-5.3-flash:
      target_hardware: "RTX PRO 6000 Blackwell"
      status: "candidate"

    deepseek-v4.1-flash:
      target_hardware: "AMD MI300X"
      status: "candidate"

    glm-5.3:
      target_hardware: "AMD MI300X"
      status: "candidate"
```

---

# 10. Caveat

The AQUEDUCT infrastructure changes quickly.  
For automated model selection, agents should ideally combine:

1. this documentation,
2. the release notes,
3. and a live query to `/v1/models`.

The live API response should be treated as authoritative for actual runtime availability.
