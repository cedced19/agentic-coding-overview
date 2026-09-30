# ACIN Beamer Template

LaTeX Beamer presentation theme matching the ACIN / TU Wien PowerPoint template.

## Requirements

- **XeLaTeX** (required for Segoe UI font support)
- **Segoe UI** font installed on your system (pre-installed on Windows)
- Standard LaTeX packages: `tikz`, `graphicx`, `amsmath`, `booktabs`, `hyperref`, `listings`

## Quick Start

1. Copy this template folder
2. Edit `main.tex` — replace content with your own
3. Compile with `xelatex main.tex` (twice) or `latexmk main.tex` (as you prefer)

## Details on the compilation

```bash
xelatex main.tex
xelatex main.tex   # twice for correct slide numbers and references
```
or more easily if you have [`latexmk`](https://miktex.org/packages/latexmk) (your `main.pdf` will then be in `build/` folder): 
```bash
latexmk main.tex
```

## Project Structure

```
├── beamerthemeACIN.sty   # Theme file (include in your project)
├── main.tex              # Demo/template — copy and edit for your presentation
├── sample_pub.pdf        # Dummy publication for demo
├── sample_pub.tex        # Source for sample_pub.pdf
└── logos/
    ├── acin_logo.png
    ├── acin_logo_short.png
    ├── tuwien_logo.png
    └── ait_logo_ohne_claim_c1_rgb.jpg
```

## Theme Colors

| Name             | Hex       | Usage                        |
|------------------|-----------|------------------------------|
| `ACINred`        | `#BA122B` | Structure, bullets, blocks   |
| `TUblue`         | `#006699` | Example blocks, links        |
| `ACINgray`       | `#7B7B7B` | Subtitles, secondary text    |
| `ACINlightgray`  | `#B2B2B2` | Progress bar background      |

## Custom Commands

### Agenda with status indicators

```latex
\agendaitem{Phase 1:}{Description text}{done}
```

Status options: `done` (green), `inprogress` (blue), `next` (orange), `planned` (gray), `future` (gray italic)

### Publication showcase

```latex
% Single publication with PDF preview
\publication{paper.pdf}{Title}{Authors}{Venue}

% Optional: control how much of the page is shown (default 160mm crop from bottom)
\publication[100mm]{paper.pdf}{Title}{Authors}{Venue}

% Grid of up to 4 publication thumbnails
\pubgrid{paper1.pdf, paper2.pdf, paper3.pdf, paper4.pdf}
```

### Logo paths

Override if your logos are in a different location:

```latex
\renewcommand{\ACINlogopath}{path/to/acin_logo.png}
\renewcommand{\TUWlogopath}{path/to/tuwien_logo.png}
\renewcommand{\AITlogopath}{path/to/ait_logo.jpg}
```

## PPTX Pipeline 

**Work in progress. Currently Linux only.**
Converts a Beamer `main.tex` into an editable PowerPoint file (`output_main.pptx`). The pipeline rasterizes TikZ figures, tables, and equations to PNG, compiles a flattened TeX document, imports the PDF into LibreOffice Impress, then post-processes the PPTX.

### Requirements

**Python** (3.9+):

```bash
pip install -r requirements.txt
```

**System tools** (must be on `PATH`):

| Tool | Purpose |
|------|---------|
| **XeLaTeX** | Compile figures, tables, equations, and `output_main.tex` |
| **LibreOffice** (`libreoffice`) | Headless PDF → PPTX conversion (`impress_pdf_import`) |
| **poppler-utils** (`pdftoppm`, `pdftocairo`) *or* **ImageMagick** (`convert`, `mogrify`) | PDF → PNG rasterization |

On Debian/Ubuntu/WSL:

```bash
sudo apt install texlive-xetex texlive-latex-extra poppler-utils libreoffice-impress
```

**Fonts:** Segoe UI is used when available (default on Windows); otherwise DejaVu Sans is used for table rendering.

### Platform support

| Environment | Supported |
|-------------|-----------|
| **Linux** | Yes — primary target |
| **WSL** (Windows) | Yes — recommended on Windows |
| **Native Windows** | Partial — XeLaTeX and Segoe UI work, but `compile_to_pptx.py` calls `libreoffice` and `python3` as Unix commands; use WSL or adapt the script to `soffice.exe` / `python` |

### Usage

```bash
python3 compile_to_pptx.py
```

Produces `output_main.pptx` in the project root. Intermediate files (`tmp_tex/`, `output_main.tex`, `output_main.pdf`) are gitignored.

### Clean generated files

Removes pipeline artifacts and other untracked/ignored files:

```bash
git clean -fdx
```