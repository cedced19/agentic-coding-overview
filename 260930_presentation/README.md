# Agentic Coding Overview slides

Slides by Cédric Jung and Jozef Janus for 30 September 2026, based on the files in `../documentation/` and the ACIN Beamer template in `../presentation_template/`.

Build with:

```bash
cd 260930_presentation
latexmk main.tex
```

The PDF is generated at `build/main.pdf` (23 slides). The build uses XeLaTeX and the Segoe UI font.

The deck follows three questions: what you can use at TU Wien (AQUEDUCT models, hardware, access), how a coding agent works (model, harness, post-training), and how to choose (routers, subscription against API pricing, six benchmarks). It ends with three recommendations and the sources.

There is one result slide for each of six benchmarks: GDPval-AA v2.1, DeepSWE v1.1, Terminal-Bench 4.0, Terminal-Bench 2.1, Humanity's Last Exam, and GPQA Diamond. Bars are coloured by whether a model is on AQUEDUCT today, an AQUEDUCT candidate, or proprietary. All slides include clickable source links.

Model availability, access details, prices, and benchmark results are a dated snapshot. Figures that were added after the original research notes are recorded in `../documentation/live_check_2026-09-30.md`. Check the linked sources and the live AQUEDUCT `/v1/models` endpoint before reuse.

Writing rules for edits are in `../plan_refinement.md`: titles are short claims, and the text avoids mid-sentence semicolons and em dashes.
