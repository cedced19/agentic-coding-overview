# Agentic Coding: Model vs. Harness

## Core idea

In **agentic coding**, two components are essential and should be clearly distinguished:

1. **The Model** — the AI model that performs reasoning, understands code, generates text/code, and decides what action should be taken next.
2. **The Harness** — the software layer that turns the model into a practical coding agent by giving it access to tools, files, terminals, context, permissions, and an execution loop.

A useful abstraction is:

```text
User
  │
  ▼
┌──────────────────────────────┐
│           HARNESS            │
│                              │
│  • Builds context            │
│  • Sends prompts to model    │
│  • Exposes tools             │
│  • Reads/writes files        │
│  • Runs terminal commands    │
│  • Manages permissions       │
│  • Maintains agent loop      │
│  • Handles subagents/state   │
└──────────────┬───────────────┘
               │ API / model interface
               ▼
┌──────────────────────────────┐
│            MODEL             │
│                              │
│  • Understands the task      │
│  • Reasons about the code    │
│  • Generates code            │
│  • Chooses next actions      │
│  • Interprets tool results   │
└──────────────────────────────┘
```

The two layers cooperate, but they are **not the same thing**.

---

# 1. The Model

The **model** is the underlying Large Language Model (LLM).

Examples include:

- OpenAI **GPT** models
- Anthropic **Claude Opus**
- Anthropic **Claude Fable**
- Mistral models
- Qwen models
- DeepSeek models

The model provides the core intelligence of the coding agent.

Typical responsibilities include:

- understanding natural-language instructions;
- understanding source code;
- reasoning about bugs and software architecture;
- generating or modifying code;
- deciding which tool should be called;
- interpreting results returned by tools;
- planning several steps ahead.

However, the model itself does **not automatically have access to your computer or repository**.

For example, the model may decide:

> "I should inspect `src/main.py`."

But something else must actually:

1. open the file;
2. read its content;
3. send the relevant content to the model;
4. receive the model's next decision.

That "something else" is the **harness**.

---

# 2. The Harness

A **harness** is the software environment and orchestration layer that surrounds the model and allows it to behave as an autonomous or semi-autonomous coding agent.

OpenAI describes the Codex harness as the component responsible for the **agent loop** and for orchestrating interactions between the user, the model, and the tools available to the model.

The harness typically provides:

- repository and file access;
- file search;
- file editing and patch application;
- terminal / shell execution;
- test execution;
- Git operations;
- tool calling;
- context construction;
- conversation/session state;
- permission and approval mechanisms;
- sandboxing;
- agent-loop orchestration;
- task planning;
- subagents;
- MCP or other external integrations;
- error handling and retries.

A simplified agent loop looks like this:

```text
1. User gives task
        │
        ▼
2. Harness builds context
        │
        ▼
3. Harness calls the model
        │
        ▼
4. Model decides what to do
        │
        ├── "Read this file"
        ├── "Search for this symbol"
        ├── "Run these tests"
        └── "Edit this function"
        │
        ▼
5. Harness executes the requested tool
        │
        ▼
6. Tool result is returned to the model
        │
        ▼
7. Model reasons again
        │
        ▼
8. Repeat until task is complete
```

This iterative process is what makes **agentic coding** different from simply asking an LLM to generate a code snippet.

---

# 3. Examples of Harnesses

## OpenAI Codex

**Codex** is OpenAI's agentic coding environment.

The Codex harness manages the interaction between OpenAI models and tools such as:

- filesystem access;
- shell execution;
- code editing;
- repository inspection;
- web/MCP tools;
- sandboxing;
- task and context management.

OpenAI explicitly uses the term **Codex harness** for the core agent loop and execution logic.

Sources:

- [OpenAI — Unrolling the Codex agent loop](https://openai.com/index/unrolling-the-codex-agent-loop/)
- [OpenAI — Codex](https://openai.com/codex/)
- [OpenAI — Introducing the Agents API](https://openai.com/index/introducing-the-agents-api/)

---

## Anthropic Claude Code

**Claude Code** is Anthropic's agentic coding tool.

It provides an agentic environment around Claude models and can:

- inspect repositories;
- read and edit files;
- execute commands;
- plan tasks;
- use project instructions;
- use skills;
- use subagents;
- connect to external systems through MCP.

The underlying **model** and the **Claude Code harness/tool** are therefore separate concepts.

For example:

```text
Claude Fable / Claude Opus
          │
          ▼
      Claude Code
          │
          ▼
 Repository + Terminal + Tools
```

Source:

- [Anthropic — Claude Code: Foundations](https://www.anthropic.com/webinars/claude-code-foundations)

---

# 4. A Harness Does Not Necessarily Belong to One Model

An important consequence of separating **model** and **harness** is that a harness can potentially support models from several providers.

Conceptually:

```text
                     ┌── GPT
                     │
                     ├── Claude
User ──► Harness ────┼── Mistral
                     │
                     ├── Qwen
                     │
                     └── DeepSeek
```

The harness is responsible for translating its agent loop and tool interface into API calls that the selected model/provider understands.

This is especially useful when:

- comparing coding models;
- using a company-hosted model;
- connecting to a local model;
- using an OpenAI-compatible endpoint;
- switching providers according to cost or performance;
- using different models for different subagents.

Model compatibility is **not automatically guaranteed**. The harness still needs an adapter for the provider/API, and the model must support the interaction pattern expected by the harness, especially tool calling and sufficiently large context windows.

---

# 5. OpenCode: an Open-Source, Model-Agnostic Harness

[OpenCode](https://opencode.ai/) is an open-source coding agent/harness.

Its architecture explicitly separates the coding-agent environment from the underlying model.

OpenCode can connect to many model providers and currently advertises support for **75+ LLM providers**, including models from providers such as:

- OpenAI;
- Anthropic;
- Google;
- local or self-hosted providers;
- other providers exposed through supported APIs.

This allows combinations such as:

```text
OpenCode + GPT
OpenCode + Claude
OpenCode + Gemini
OpenCode + another supported model
```

This illustrates a fundamental concept:

> **Choosing a harness and choosing a model are two separate architectural decisions.**

Source:

- [OpenCode — The open source AI coding agent](https://opencode.ai/)

---

# 6. Mistral Vibe: a Configurable Open-Source Harness

[Mistral Vibe](https://github.com/mistralai/mistral-vibe) is an open-source CLI coding assistant developed by Mistral.

It provides a full coding-agent environment with capabilities such as:

- file reading and editing;
- code search;
- shell execution;
- task tracking;
- tool permissions;
- project-aware context;
- subagents;
- skills;
- MCP integrations;
- configurable models and providers.

Its configuration explicitly distinguishes **models** from **providers**, meaning that the harness is architected independently from a single hard-coded model.

Recent versions also include provider/backend abstractions and an OpenAI Responses API adapter, making it possible to integrate model endpoints beyond the default Mistral configuration when a compatible provider configuration is available.

Conceptually:

```text
              model/provider API
                     │
                     ▼
                Mistral Vibe
                     │
        ┌────────────┼─────────────┐
        ▼            ▼             ▼
      Files        Shell         Tools
```

Sources:

- [Mistral Vibe — GitHub repository](https://github.com/mistralai/mistral-vibe)
- [Mistral Vibe — README / configuration](https://github.com/mistralai/mistral-vibe/blob/main/README.md)
- [Mistral Vibe — Changelog](https://github.com/mistralai/mistral-vibe/blob/main/CHANGELOG.md)

---

# 7. Model vs. Harness — Summary Table

| Component | Model | Harness |
|---|---|---|
| Main purpose | Intelligence and reasoning | Orchestration and execution |
| Understands code | Yes | Usually delegates understanding to the model |
| Generates code | Yes | Applies/generated changes to files |
| Reads repository files directly | No, not by itself | Yes |
| Runs terminal commands directly | No, not by itself | Yes |
| Executes tests | No, not by itself | Yes |
| Manages tools | Chooses/invokes them conceptually | Defines and executes them |
| Manages permissions | No | Yes |
| Maintains agent loop | No | Yes |
| Manages context | Consumes context | Selects/builds context |
| Examples | GPT, Claude Opus, Claude Fable, Mistral | Codex, Claude Code, OpenCode, Mistral Vibe |

---

# 8. Why the Distinction Matters

When comparing agentic coding systems, saying:

> "Model A is better than Model B"

is often incomplete.

The observed performance depends on both:

```text
Agentic Coding Performance
        =
Model Capability
        +
Harness Quality
        +
Tooling
        +
Context Management
        +
Prompt / Agent Design
        +
Execution Environment
```

The same model can behave differently when used through different harnesses because each harness may:

- expose different tools;
- construct context differently;
- use different system prompts;
- handle tool errors differently;
- manage long conversations differently;
- implement different planning strategies;
- provide different sandbox and permission systems;
- use different subagent strategies.

Therefore, a fair comparison should distinguish between:

```text
MODEL BENCHMARK
"What can the underlying model do?"

and

AGENT / HARNESS BENCHMARK
"What can the complete coding system do?"
```

---

# 9. Recommended Mental Model

A simple analogy is:

```text
MODEL   = the engine
HARNESS = the vehicle around the engine
```

A powerful engine is important, but the complete performance also depends on:

- transmission;
- steering;
- sensors;
- controls;
- navigation;
- safety systems.

Similarly, in agentic coding:

```text
Model
  = reasoning capability

Harness
  = everything required to transform that capability
    into actions on a real software project
```

The most important takeaway is therefore:

> **Agentic coding is not only about the model. It is the combination of a capable model and a capable harness.**

And because some harnesses support several providers, the two components can increasingly be selected independently:

```text
Choose the Harness
        +
Choose the Model
        =
Agentic Coding System
```

---

## References

1. OpenAI, **Unrolling the Codex agent loop**  
   https://openai.com/index/unrolling-the-codex-agent-loop/

2. OpenAI, **Codex**  
   https://openai.com/codex/

3. OpenAI, **Introducing the Agents API**  
   https://openai.com/index/introducing-the-agents-api/

4. Anthropic, **Claude Code: Foundations**  
   https://www.anthropic.com/webinars/claude-code-foundations

5. OpenCode  
   https://opencode.ai/

6. Mistral Vibe  
   https://github.com/mistralai/mistral-vibe

7. Mistral Vibe README  
   https://github.com/mistralai/mistral-vibe/blob/main/README.md

8. Mistral Vibe Changelog  
   https://github.com/mistralai/mistral-vibe/blob/main/CHANGELOG.md
