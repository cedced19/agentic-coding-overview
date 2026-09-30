# Agentic Coding: Why the Model Is Not Fully Independent from the Harness

## Core idea

It is useful to separate an **agentic coding system** into two components:

```text
Agentic Coding System
    =
Model
    +
Harness
```

However, this separation is mainly an **architectural** one.

In practice, the model and the harness are **not always independent choices**, because the model may have been **post-trained specifically inside agentic environments that resemble a particular harness**.

A more realistic representation is therefore:

```text
                 TRAINING / POST-TRAINING
                         │
                         ▼
              ┌──────────────────────┐
              │  Agent Environment   │
              │                      │
              │  • files             │
              │  • terminal          │
              │  • tests             │
              │  • tool calls        │
              │  • rewards           │
              │  • agent loop        │
              └──────────┬───────────┘
                         │
               reinforcement learning
                 / trajectory training
                         │
                         ▼
                    ┌─────────┐
                    │  Model  │
                    └────┬────┘
                         │
                         ▼
                 DEPLOYMENT HARNESS
```

The model can therefore learn not only **how to write code**, but also **how to behave inside a specific type of agent loop**.

---

## 1. Post-training can include Reinforcement Learning

Modern language models are first **pre-trained** on large corpora.

They are then often subjected to **post-training**, which adapts their behavior to particular tasks.

Post-training can use techniques such as:

- supervised fine-tuning;
- preference optimization;
- reinforcement learning;
- reinforcement learning from verifiable rewards (RLVR);
- trajectory-based training.

For agentic coding, reinforcement learning is particularly interesting because the model can interact with a real or simulated software environment.

Instead of learning only from:

```text
Prompt → Answer
```

the model can learn from complete trajectories:

```text
Task
 ↓
Inspect repository
 ↓
Search files
 ↓
Edit code
 ↓
Run tests
 ↓
Observe failure
 ↓
Edit again
 ↓
Run tests
 ↓
Tests pass
 ↓
Reward
```

The **reward** can depend on whether the generated solution actually works.

For example:

```text
tests fail  → low reward
tests pass  → high reward
```

This turns software engineering into an interactive reinforcement-learning problem.

---

## 2. The Harness Can Become Part of the Training Distribution

The important consequence is that the model is not necessarily trained as an isolated text generator.

During post-training, the model may repeatedly interact with:

- a terminal;
- a filesystem;
- code-editing tools;
- unit tests;
- linters;
- compilers;
- repository search tools;
- specific tool schemas;
- specific prompts;
- a particular agent loop.

These elements strongly overlap with what we call the **harness** at inference time.

Therefore:

> **The harness, or a training environment that reproduces its interaction structure, can effectively become part of the model's post-training distribution.**

This does **not necessarily mean that the exact production harness executable is used during training**.

More generally, the training system can expose the same type of:

```text
Observation
   ↓
Model decision
   ↓
Tool call
   ↓
Environment execution
   ↓
Tool result
   ↓
New model decision
```

The model can consequently learn policies that are particularly effective under that interaction protocol.

---

## 3. Concrete Example: OpenAI Codex Models

OpenAI explicitly states that **codex-1** was trained using reinforcement learning on real-world coding tasks in a variety of environments.

The model was trained to:

- follow software-engineering instructions;
- edit code;
- iteratively run tests;
- continue working until tests pass.

OpenAI describes the Codex execution environment as one in which the agent can:

- read and edit files;
- execute commands;
- run tests;
- run linters;
- run type checkers.

This is already much closer to an **agent harness environment** than to traditional text-only language-model training.

The same principle was later used for **GPT-5-Codex**.

OpenAI states that GPT-5-Codex:

> is a version of GPT-5 optimized for agentic coding in Codex.

It was also trained using reinforcement learning on real-world coding tasks in different environments.

OpenAI further describes GPT-5-Codex as **purpose-built for Codex CLI, the Codex IDE extension, the Codex cloud environment, and GitHub workflows**, and recommends its use in **Codex or Codex-like environments**.

This is important because it means:

```text
GPT-5-Codex
       │
       └── was not optimized only for "coding" in the abstract

           but for agentic coding behavior
           within a tool-using environment
```

### Sources

- OpenAI — Addendum to GPT-5 System Card: GPT-5-Codex  
  https://openai.com/index/gpt-5-system-card-addendum-gpt-5-codex/

- OpenAI — Addendum to o3 and o4-mini System Card: Codex  
  https://openai.com/index/o3-o4-mini-codex-system-card-addendum/

- OpenAI — Introducing upgrades to Codex  
  https://openai.com/index/introducing-upgrades-to-codex/

- OpenAI — Unrolling the Codex agent loop  
  https://openai.com/index/unrolling-the-codex-agent-loop/

---

## 4. Research Evidence: SWE-Gym

The same idea also appears in academic research.

**SWE-Gym** was introduced as an environment for training real-world software-engineering agents.

Each task includes:

- a real codebase;
- an executable runtime environment;
- a natural-language software-engineering task;
- tests that can verify whether the solution works.

The authors explicitly discuss **post-training models as agents** and the need for a training environment with reliable reward signals.

In this setting, the training environment is therefore not merely a benchmark.

It becomes part of the learning process:

```text
Model
  │
  ▼
Agent scaffold / harness
  │
  ▼
Repository + runtime
  │
  ▼
Unit tests
  │
  ▼
Reward / training signal
```

The model learns from **agent-environment interaction trajectories**.

### Source

Pan et al., *Training Software Engineering Agents and Verifiers with SWE-Gym*, ICML 2025:

https://proceedings.mlr.press/v267/pan25g.html

---

## 5. Reinforcement Learning in Agentic Environments

More recent work makes this coupling even more explicit.

For example, **Agent-RLVR** trains software-engineering agents using reinforcement learning from verifiable rewards.

The model produces multi-step agent trajectories, interacts with the environment, receives feedback such as unit-test results, and is then updated using the reward from these interactions.

The learning problem is therefore defined over:

```text
Model policy
   +
Agent interaction protocol
   +
Environment
   +
Reward function
```

rather than over the model alone.

### Source

Da et al., *Agent-RLVR: Training Software Engineering Agents via Guidance and Environment Rewards*:

https://arxiv.org/abs/2506.11425

---

# 6. Consequence: Model Choice and Harness Choice Are Coupled

A simple architecture diagram can suggest:

```text
Choose any model
      +
Choose any harness
      =
Coding agent
```

Technically, this can often be done through APIs.

For example:

```text
OpenCode + GPT
OpenCode + Claude
OpenCode + Mistral
```

However, **technical compatibility does not imply optimal compatibility**.

If a model has been post-trained with:

- a particular tool vocabulary;
- particular tool descriptions;
- a specific patch mechanism;
- specific terminal interactions;
- certain context-management conventions;
- certain feedback loops;
- a particular style of agent trajectory;

then another harness may present a different distribution.

This creates a form of **distribution shift**.

For example:

```text
POST-TRAINING

Model sees:
shell
apply_patch
test result
shell
apply_patch
test result


DEPLOYMENT IN ANOTHER HARNESS

Model sees:
custom_edit_file
execute_task
workspace_state
custom verification tool
```

Even if both harnesses provide approximately the same capabilities, the interaction pattern can be different.

The model may therefore perform differently.

---

# 7. A Better Mental Model

Instead of thinking of model and harness as completely independent modules:

```text
Model  ⟂  Harness
```

it is better to think of them as **separate but coupled components**:

```text
          ┌───────────────┐
          │     Model     │
          └───────┬───────┘
                  │
        learned interaction
          assumptions / habits
                  │
          ┌───────▼───────┐
          │    Harness    │
          └───────────────┘
```

The model still contains the intelligence.

The harness still contains the execution infrastructure.

But the model's behavior may have been **optimized around a particular class of harnesses during post-training**.

---

# 8. Practical Consequence

When benchmarking agentic coding systems, it is therefore insufficient to ask only:

> **Which model is better?**

A better question is:

> **Which model + harness combination performs best for this task?**

Formally:

```text
Agent Performance
   =
f(
    model capability,
    model post-training,
    harness,
    system prompt,
    available tools,
    tool schemas,
    context management,
    execution environment,
    reward / verification loop
 )
```

This also explains why the **same model may perform differently in different coding agents**.

Conversely, changing the model inside a harness may not reproduce the performance observed with the model for which that harness or interaction style was originally optimized.

---

# Key Takeaway

> **Model and harness are distinct components, but they are not fully independent.**

In agentic coding, reinforcement-learning post-training can occur inside interactive software-engineering environments containing terminals, files, tests, tools, and agent loops.

As a result, the model can learn behaviors that are specifically adapted to a certain type of harness.

Therefore:

```text
Model capability
      ≠
complete agent capability
```

and:

```text
Best Model
      +
Arbitrary Harness

does not necessarily equal

Best Agent
```

A more accurate view is:

```text
Agentic Coding System
       =
Model
       ↕
Harness
```

with a **coupling between both components created partly during post-training**.

---

## References

1. OpenAI, **Addendum to GPT-5 System Card: GPT-5-Codex**  
   https://openai.com/index/gpt-5-system-card-addendum-gpt-5-codex/

2. OpenAI, **Addendum to o3 and o4-mini System Card: Codex**  
   https://openai.com/index/o3-o4-mini-codex-system-card-addendum/

3. OpenAI, **Introducing upgrades to Codex**  
   https://openai.com/index/introducing-upgrades-to-codex/

4. OpenAI, **Unrolling the Codex agent loop**  
   https://openai.com/index/unrolling-the-codex-agent-loop/

5. Pan et al., **Training Software Engineering Agents and Verifiers with SWE-Gym**, ICML 2025  
   https://proceedings.mlr.press/v267/pan25g.html

6. Da et al., **Agent-RLVR: Training Software Engineering Agents via Guidance and Environment Rewards**  
   https://arxiv.org/abs/2506.11425
