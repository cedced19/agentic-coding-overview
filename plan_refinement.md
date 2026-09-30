Goal : Refine 260930_presentation/ so it reads like a talk given by people, matches the live sources on 30 September 2026, and covers the current frontier models.

Follows plan.md. Same authors, same template, same build (latexmk).


What is wrong with the first version:
- Titles are labels ("OpenRouter", "GPQA Diamond"). No opening, no thread, no conclusion.
- The same "verify before use" caveat is repeated on several slides.
- AQUEDUCT slides do not match the live overview page: it still lists four LLMs (qwen-3.5-397b retires today, deepseek-v4-flash-284b is still shown on H200), and gives quantization per model.
- Access slide is vague. The live page says: TU Wien SSO, personal API key, OpenAI-compatible base URL, setup guides for Pi, OpenCode, Crush, Zed.
- GPT-6 Astra (3 Sept) and Claude Fable 5.1 (1 Sept) are missing from charts and pricing.
- Layout bugs: legend overflow on the benchmark intro slide, misaligned "Candidate" tags, a bullet touching the page number.


The slides should be as follows:
- (1 slide) Title
- (1 slide) NEW Three questions for today (what can I use, how does it work, how do I choose)
- (2 slides) AQUEDUCT today (four listed LLMs, with quantization) and candidates
- (1 slide) Hardware, explained through memory per replica
- (1 slide) Access: SSO + API key, students allowlisted, base URL, documented harnesses
- (1 slide) Model vs harness
- (1 slide) NEW Harness landscape: which exist, which work with AQUEDUCT
- (1 slide) Post-training, Codex example, plus the measured harness effect on Terminal-Bench 4.0
- (3 slides) Routers: why, OpenRouter, European options
- (1 slide) Subscription vs API, as a chart of list prices against $20/month, including the $10/$50 tier
- (1 slide) Which benchmark answers which question
- (6 slides) One chart per benchmark, takeaway as title, bars coloured by
  on AQUEDUCT today / AQUEDUCT candidate / proprietary, with GPT-6 Astra and Claude Fable 5.1 added
- (1 slide) NEW What we would do (three recommendations)
- (2 slides) Sources


Writing rules:
- Title = a short claim (max ~44 characters, the logos take the rest of the line). Detail goes in the subtitle.
- Plain sentences, "we" and "you", concrete numbers. One caveat per topic, said once.
- No AI language. No semicolon in the middle of a sentence, no em dash, no decorative separators. Split into two sentences or use a comma.
- Every number keeps its source link. New numbers are recorded in documentation/live_check_2026-09-30.md.

Check compiling with latexmk, render every page, fix overflow.
