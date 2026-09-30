# LLM Routing Platforms: OpenRouter and European Alternatives

> **Status:** September 2026  
> Pricing, model availability, provider availability, and regional support change frequently. Always verify the current catalog before production deployment.

## 1. Why platforms such as OpenRouter exist

When using Large Language Models (LLMs) through APIs, there are usually several decisions to make:

1. **Which model?**
   - GPT
   - Claude
   - Gemini
   - Mistral
   - DeepSeek
   - Qwen
   - Llama
   - etc.

2. **Which provider should execute the model?**
   - the model developer itself;
   - a hyperscaler such as Azure, AWS, or Google Cloud;
   - a specialized inference provider;
   - a European cloud provider.

3. **Where should the inference run?**
   - United States;
   - European Union;
   - a specific cloud region;
   - another jurisdiction.

4. **What should be optimized?**
   - price;
   - latency;
   - throughput;
   - reliability;
   - data residency;
   - zero-data-retention requirements.

Connecting directly to every provider quickly becomes complicated.

Platforms such as **OpenRouter**, **EUrouter**, and **Eden AI** introduce an intermediate routing layer:

```text
                        ┌── Provider A
                        │
Application ──► Router ─┼── Provider B
                        │
                        ├── Provider C
                        │
                        └── Provider D
```

The application talks to **one API**, while the routing platform decides — or lets the user decide — which model and provider should process the request.

These platforms are often called:

- **AI gateways**
- **LLM gateways**
- **model routers**
- **inference routers**
- **model aggregators**

---

# 2. The Important Distinction: Model vs. Provider

A **model** and a **provider** are not necessarily the same thing.

For example, conceptually:

```text
                  Same model
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
      Provider A  Provider B  Provider C
       € cheaper    faster      EU-hosted
```

The model determines the learned capabilities:

```text
Model
=
"Which neural network is answering?"
```

The provider determines where and how that model is served:

```text
Provider
=
"Who is running the inference infrastructure?"
```

This distinction matters because the **same open-weight model** may be available from several providers at different:

- prices;
- latencies;
- throughput levels;
- geographical locations;
- privacy policies;
- retention policies.

A routing platform can therefore optimize not only:

```text
Which MODEL should I use?
```

but also:

```text
Which PROVIDER should execute this model?
```

---

# 3. OpenRouter

[OpenRouter](https://openrouter.ai/) is one of the best-known model-routing platforms.

It provides a **single API** for accessing hundreds of AI models and can automatically route requests between providers.

A simplified architecture is:

```text
Application
    │
    ▼
OpenRouter
    │
    ├── Model A ──► Provider 1
    │          └──► Provider 2
    │
    ├── Model B ──► Provider 3
    │          └──► Provider 4
    │
    └── Model C ──► Provider 5
```

The application therefore does not need a separate integration for every inference provider.

OpenRouter also supports:

- provider fallbacks;
- explicit provider ordering;
- provider filtering;
- provider selection according to price;
- provider selection according to throughput;
- provider selection according to latency;
- model fallbacks;
- Bring Your Own Key (BYOK).

For example, OpenRouter can prioritize the cheapest provider:

```json
{
  "model": "meta-llama/llama-3.1-70b-instruct",
  "provider": {
    "sort": "price"
  }
}
```

or prioritize throughput:

```json
{
  "model": "meta-llama/llama-3.1-70b-instruct",
  "provider": {
    "sort": "throughput"
  }
}
```

OpenRouter also provides shortcuts such as:

```text
:model:floor  → prioritize lowest price
:model:nitro  → prioritize throughput
```

### Sources

- OpenRouter — Quickstart  
  https://openrouter.ai/docs/quickstart

- OpenRouter — Provider routing / performance  
  https://openrouter.ai/blog/insights/evaluate-llm-provider-performance/

- OpenRouter — Nitro and floor-price routing  
  https://openrouter.ai/announcements/introducing-nitro-and-floor-price-shortcuts

---

# 4. Geographic Routing Matters

The cheapest or fastest provider is not always the correct choice.

For organizations working with:

- confidential source code;
- industrial data;
- personal data;
- research data;
- customer documents;
- regulated information;

the **location where inference is performed** may matter.

The routing problem can therefore become:

```text
Find provider P

such that:

    model = desired model
    price = low
    latency = acceptable
    server_region ∈ European Union
    retention_policy = acceptable
```

This adds another dimension to model routing:

```text
MODEL
  +
PROVIDER
  +
REGION
  +
PRICE / PERFORMANCE
```

---

# 5. OpenRouter and EU In-Region Routing

OpenRouter now also provides **EU in-region routing**.

Its EU endpoint is:

```text
https://eu.openrouter.ai/api/v1
```

According to OpenRouter, requests sent through this endpoint are routed only to provider endpoints operating in the European Union, so prompts and completions remain in-region.

At the time of writing, OpenRouter's documentation describes EU in-region routing as available to **enterprise customers by request**.

This means OpenRouter can support an architecture such as:

```text
Application
    │
    ▼
eu.openrouter.ai
    │
    ├── EU Provider Endpoint A
    ├── EU Provider Endpoint B
    └── EU Provider Endpoint C
```

### Sources

- OpenRouter — Sovereign AI / EU In-Region Routing  
  https://openrouter.ai/docs/guides/get-started/sovereign-ai

- OpenRouter — In-Region Routing announcement  
  https://openrouter.ai/blog/announcements/us-in-region-routing/

---

# 6. EUrouter: A European Alternative to OpenRouter

A particularly relevant European alternative is **EUrouter**:

https://www.eurouter.ai/

EUrouter describes itself as:

> **The European AI Gateway**

Its concept is very similar to OpenRouter:

```text
One API
   │
   ▼
Choose a model
   │
   ▼
Route to an inference provider
```

but EUrouter is built specifically around **European data residency**.

According to its documentation:

- all inference traffic is processed inside the EU;
- one API gives access to multiple providers;
- providers can be selected automatically;
- routing considers availability, performance, price, and capacity;
- fallbacks can automatically switch to another provider;
- provider selection can be controlled manually;
- requests can be filtered according to processing region;
- requests can be filtered according to EU ownership;
- provider retention and data-collection policies can be used as routing criteria.

This gives an architecture such as:

```text
                    EU ONLY
            ┌─────────────────────┐
            │                     │
Application │     EUrouter        │
    ───────►│                     │
            └──────────┬──────────┘
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
     Provider A    Provider B    Provider C
       EU infra      EU infra      EU infra
```

The important difference is that **EU infrastructure is a core routing constraint**, rather than merely an optional deployment requirement.

---

# 7. EUrouter Provider Selection

A user can simply specify a model:

```json
{
  "model": "mistral-large-3",
  "messages": [
    {
      "role": "user",
      "content": "Hello!"
    }
  ]
}
```

EUrouter then:

```text
1. Finds providers hosting that model
2. Removes unavailable / unhealthy providers
3. Applies routing constraints
4. Scores eligible providers
5. Selects a provider
6. Falls back to another provider if necessary
```

Its default routing considers provider health and price.

The user can also specify provider preferences.

For example:

```json
{
  "model": "mistral-large-3",
  "messages": [
    {
      "role": "user",
      "content": "Hello!"
    }
  ],
  "provider": {
    "order": ["scaleway", "azure"],
    "allow_fallbacks": true
  }
}
```

EUrouter also supports:

```text
provider.only
```

to restrict execution to an allowlist, and:

```text
provider.ignore
```

to exclude providers.

This is useful when infrastructure constraints matter as much as model quality.

### Sources

- EUrouter — Introduction  
  https://www.eurouter.ai/docs

- EUrouter — Routing  
  https://www.eurouter.ai/docs/concepts/routing

- EUrouter — Providers  
  https://www.eurouter.ai/docs/concepts/providers

---

# 8. Region Is Different from Provider Ownership

An important nuance is:

```text
EU-hosted
    ≠
EU-owned
```

A US company can operate infrastructure inside an EU data center.

Likewise, a European company can potentially operate infrastructure outside the EU.

Therefore, several different constraints can exist:

```text
Processing location:
    "The GPU executing my request must be in the EU."

Provider ownership:
    "The inference provider should be a European company."

Data residency:
    "My prompts and outputs must remain inside the EU."

Retention:
    "The provider must not retain my prompts."

Jurisdiction:
    "I need a provider governed by a particular legal framework."
```

EUrouter explicitly distinguishes these concepts in its provider metadata and routing controls.

This is an important advantage of treating **provider selection as an explicit architectural decision**.

---

# 9. OpenRouter vs. EUrouter

| Feature | OpenRouter | EUrouter |
|---|---|---|
| Unified API | Yes | Yes |
| Multiple models | Yes | Yes |
| Multiple inference providers | Yes | Yes |
| Provider routing | Yes | Yes |
| Route by price | Yes | Yes |
| Route by performance | Yes | Yes |
| Automatic fallback | Yes | Yes |
| Explicit provider selection | Yes | Yes |
| OpenAI-compatible API | Yes | Yes |
| EU-only inference | Available through EU in-region routing | Core platform design |
| EU-specific endpoint | Yes | EU infrastructure by design |
| Provider-region filtering | Yes / EU routing options | Yes |
| EU-provider ownership filter | Not the main abstraction | Explicitly supported |
| Retention-policy filtering | Privacy/provider controls | Provider metadata and filters |
| Primary positioning | Global model marketplace/router | European AI gateway |

The distinction can be summarized as:

```text
OpenRouter
=
Global model/provider router
+
optional regional routing capabilities


EUrouter
=
Model/provider router
+
European infrastructure as a core constraint
```

---

# 10. Eden AI: Another European Platform

Another relevant platform is **Eden AI**, headquartered in France:

https://www.edenai.co/

Eden AI exposes multiple AI providers and models through one API.

Its dedicated European endpoint is:

```text
https://api.eu.edenai.run/v3/
```

According to Eden AI, requests sent through this endpoint keep:

- prompts;
- files;
- requests;
- outputs;

processed within Europe.

If a global provider does not offer compatible European infrastructure, the model/provider is **not exposed through the EU endpoint** rather than silently routing the request outside Europe.

Eden AI therefore offers another architecture:

```text
Application
    │
    ▼
Eden AI EU Endpoint
    │
    ├── European provider
    │
    ├── European provider
    │
    └── Global provider
           │
           └── only if EU-hosted infrastructure is available
```

Eden AI also provides a multi-provider and multi-model abstraction, making it another European alternative to OpenRouter.

### Sources

- Eden AI — European AI Gateway  
  https://www.edenai.co/eu

- Eden AI — Smart routing  
  https://edenai.co/docs/v3/llms/smart-routing

- Eden AI — Pricing  
  https://www.edenai.co/pricing

---

# 11. Other European Hosting Options

There are also platforms that are **not exactly OpenRouter replacements** but solve part of the same problem.

## Scaleway Generative APIs

French cloud provider **Scaleway** provides serverless Generative APIs and dedicated model deployments.

Its serverless models are hosted in European data centers.

As of September 2026, Scaleway states that its serverless inference models are hosted in Paris, France, and that models exposed through `api.scaleway.ai` will remain hosted in Europe.

This provides strong regional control, but it is conceptually different from OpenRouter or EUrouter:

```text
OpenRouter / EUrouter
    =
route across multiple inference providers


Scaleway
    =
one cloud/inference provider offering multiple models
```

So Scaleway can itself be one of the **providers behind a routing platform**.

### Sources

- Scaleway — Generative APIs FAQ  
  https://www.scaleway.com/en/docs/generative-apis/faq/

- Scaleway — Generative APIs  
  https://www.scaleway.com/en/inference/

- Scaleway — Pricing  
  https://www.scaleway.com/en/pricing/model-as-a-service/

---

# 12. The Complete Agentic-Coding Architecture

This distinction becomes particularly useful for **agentic coding**.

Instead of thinking only in terms of:

```text
Harness
   │
   ▼
Model
```

a more complete architecture can be:

```text
┌──────────────────────┐
│       HARNESS        │
│                      │
│ Codex / Claude Code  │
│ OpenCode / Vibe      │
└──────────┬───────────┘
           │
           │ OpenAI-compatible API
           ▼
┌──────────────────────┐
│    ROUTER / GATEWAY  │
│                      │
│ OpenRouter           │
│ EUrouter             │
│ Eden AI              │
└──────────┬───────────┘
           │
           │ select model + provider
           ▼
┌──────────────────────┐
│        MODEL         │
│                      │
│ GPT / Claude         │
│ Mistral / Qwen       │
│ DeepSeek / Llama     │
└──────────┬───────────┘
           │
           │ executed by
           ▼
┌──────────────────────┐
│      PROVIDER        │
│                      │
│ Scaleway / Azure     │
│ specialist provider  │
│ model developer      │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│       REGION         │
│                      │
│ Paris / Frankfurt    │
│ EU / US / ...        │
└──────────────────────┘
```

There are therefore several independent — but interacting — layers:

```text
1. Harness
2. Router
3. Model
4. Provider
5. Physical / cloud region
```

This is a much more accurate representation of modern agentic AI infrastructure.

---

# 13. Example: Cost-Oriented Configuration

Suppose a coding harness needs a particular open model.

The requirement is:

```text
Model:
    Qwen / DeepSeek / Mistral

Priority:
    lowest possible inference cost

Constraint:
    processing must remain inside the EU
```

Instead of hard-coding one provider:

```text
Harness
   │
   ▼
Provider X
```

a router can dynamically select among compatible providers:

```text
Harness
   │
   ▼
EU Router
   │
   ├── Provider A: €€
   ├── Provider B: €
   └── Provider C: €€€

             │
             ▼
       choose Provider B
```

If Provider B becomes unavailable:

```text
Provider B fails
       │
       ▼
Router selects Provider A
```

The application itself does not need to change.

---

# 14. Example: Data-Sovereignty-Oriented Configuration

For confidential source code, the requirement may instead be:

```text
Model:
    suitable coding model

Provider:
    must satisfy organization policy

Region:
    EU only

Retention:
    zero / acceptable retention policy

Fallback:
    only to providers satisfying the same constraints
```

The router becomes a **policy enforcement layer**:

```text
Request
   │
   ▼
Routing constraints
   │
   ├── Is provider EU-hosted?
   ├── Is provider allowed?
   ├── Is model available?
   ├── Is retention acceptable?
   ├── Is price below limit?
   └── Is provider healthy?
           │
           ▼
     Eligible provider
```

This is much more powerful than simply choosing a model name.

---

# 15. Key Takeaway

Platforms such as **OpenRouter** introduce an important abstraction:

> **The application does not necessarily need to choose one fixed model provider.**

Instead, a routing platform can mediate between:

```text
Application
   │
   ▼
Model
   │
   ▼
Provider
   │
   ▼
Region
```

and optimize the provider according to:

```text
PRICE
PERFORMANCE
LATENCY
AVAILABILITY
PRIVACY
DATA RESIDENCY
```

For European applications, platforms such as **EUrouter** and **Eden AI's EU endpoint** are particularly interesting because European processing is a first-class design constraint.

A useful summary is therefore:

```text
OpenRouter
    → global model/provider marketplace and router

EUrouter
    → OpenRouter-like routing with EU-hosted inference as a core constraint

Eden AI EU
    → European unified AI API with dedicated EU data-residency routing

Scaleway
    → European inference provider/cloud, rather than a multi-provider router
```

The infrastructure choice can consequently be made at several levels:

```text
Choose the HARNESS
       +
Choose the ROUTER
       +
Choose the MODEL
       +
Choose / constrain the PROVIDER
       +
Choose / constrain the REGION
       =
Complete Agentic AI Stack
```

---

# References

## OpenRouter

1. OpenRouter — Quickstart  
   https://openrouter.ai/docs/quickstart

2. OpenRouter — Provider routing and performance  
   https://openrouter.ai/blog/insights/evaluate-llm-provider-performance/

3. OpenRouter — Nitro and Floor Price routing  
   https://openrouter.ai/announcements/introducing-nitro-and-floor-price-shortcuts

4. OpenRouter — EU / Sovereign AI routing  
   https://openrouter.ai/docs/guides/get-started/sovereign-ai

5. OpenRouter — In-Region Routing  
   https://openrouter.ai/blog/announcements/us-in-region-routing/

## EUrouter

6. EUrouter — Introduction  
   https://www.eurouter.ai/docs

7. EUrouter — Routing  
   https://www.eurouter.ai/docs/concepts/routing

8. EUrouter — Providers  
   https://www.eurouter.ai/docs/concepts/providers

9. EUrouter — European AI Gateway  
   https://www.eurouter.info/

## Eden AI

10. Eden AI — EU Endpoint  
    https://www.edenai.co/eu

11. Eden AI — Smart Routing  
    https://edenai.co/docs/v3/llms/smart-routing

12. Eden AI — Pricing  
    https://www.edenai.co/pricing

## Scaleway

13. Scaleway — Generative APIs FAQ  
    https://www.scaleway.com/en/docs/generative-apis/faq/

14. Scaleway — Generative APIs / Managed Inference  
    https://www.scaleway.com/en/inference/

15. Scaleway — Model-as-a-Service Pricing  
    https://www.scaleway.com/en/pricing/model-as-a-service/
