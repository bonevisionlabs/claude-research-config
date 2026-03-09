# CLAUDE.md — Scientific Research Project

> Drop this file in the root of any research project to configure Claude Code
> for PhD-level scientific work. Customize the sections below for your domain.

## Project Overview

**Project**: [Your project name]
**Domain**: [e.g., biomedical imaging, NLP, materials science]
**Stage**: [e.g., data collection, analysis, writing, revision]
**Target venues**: [e.g., Nature Methods, IEEE TMI, NeurIPS]

### Papers in Progress

| # | Title (working) | Status | Target |
|---|-----------------|--------|--------|
| 1 | [Paper 1 title] | [drafting/revision/submitted] | [journal/conference] |
| 2 | [Paper 2 title] | [drafting/revision/submitted] | [journal/conference] |

## Directory Structure

```
project-root/
├── CLAUDE.md              ← this file
├── data/                  # Raw and processed datasets
│   ├── raw/               # Never modify originals
│   └── processed/         # Pipeline outputs
├── code/                  # Training, inference, analysis scripts
├── results/               # Experiment outputs (JSON, CSV)
├── state/                 # research-agent managed state
│   └── paper1_*/          # Per-paper sections, figures, drafts
│       ├── sections/      # .txt files (IMRAD structure)
│       ├── figures/       # .png files (300 DPI)
│       └── drafts/        # .docx compiled papers
├── notebooks/             # Exploratory analysis
└── references/            # Literature, BibTeX
```

## Coding Conventions

- **Python >= 3.10** with type hints on all public functions
- **Docstrings**: NumPy style for research code, Google style for utilities
- **Naming**: `snake_case` for functions/variables, `PascalCase` for classes
- **No magic numbers**: Constants at module top with descriptive names
- **Reproducibility**: Set random seeds, log hyperparameters, version dependencies

### Data Handling Rules

- **NEVER** modify raw data files — always write to `processed/` or `results/`
- **NEVER** commit patient data, PHI, or datasets to git
- Store paths in config, not hardcoded in scripts
- Use relative paths from project root where possible

## Scientific Writing Standards

### Structure

All papers follow **IMRAD** format with sections stored as `.txt` files:

```
abstract.txt, introduction.txt, methods.txt, results.txt,
discussion.txt, conclusion.txt, references.txt
```

Optional sections: `data_availability.txt`, `ethics.txt`,
`author_contributions.txt`, `competing_interests.txt`, `acknowledgements.txt`

### Writing Rules

- **Active voice preferred**: "We trained the model" not "The model was trained"
- **Past tense for methods/results**: "We used..." / "The model achieved..."
- **Present tense for established facts**: "Deep learning is..." / "Figure 1 shows..."
- **Quantitative claims require evidence**: Every number needs a source or computation
- **No unsupported superlatives**: Avoid "best", "novel", "first" without proof
- **Hedging for appropriate claims**: "suggests", "indicates", "is consistent with"

### Citation Format

- Use numbered references: `[1]`, `[2, 3]`, `[4-7]`
- Or author-year per venue requirements: `(Smith et al., 2024)`
- Every citation must appear in references section
- Prefer primary sources over reviews when citing specific methods

### Figure Standards

- **300 DPI minimum** (600 DPI for line art)
- **Colorblind-safe palette**: Use Wong (2011) colors — `#0072B2`, `#E69F00`, `#009E73`, `#D55E00`, `#56B4E9`, `#CC79A7`, `#F0E442`
- **Font size**: >= 8pt in final printed size
- **File format**: PNG for raster, PDF/SVG for vector
- **Naming**: `fig1_description.png`, `fig2_description.png` (sequential)
- **Captions**: Stored in `PaperConfig.figure_captions` dict, not embedded in images

### Table Standards

- **Bold headers**, 9pt body text
- **Significant figures**: Match measurement precision (don't report 0.95432 if instrument resolves to 0.01)
- **Mean +/- SD** format for aggregate metrics
- **Statistical significance**: Report exact p-values, mark with * p<0.05, ** p<0.01, *** p<0.001
- **Best results in bold** within comparison tables

## Statistical Analysis

### Required for Any Comparison

1. **Descriptive statistics**: mean, std, median, IQR for all metrics
2. **Appropriate test selection**:
   - Paired data, small N, non-normal → Wilcoxon signed-rank test
   - Paired data, large N, normal → Paired t-test
   - Multiple groups → ANOVA with post-hoc correction (Bonferroni/Tukey)
3. **Effect size**: Cohen's d (paired) or eta-squared (ANOVA)
4. **Confidence intervals**: 95% CI for primary metrics
5. **Multiple comparison correction**: When testing >1 hypothesis

### Cross-Validation Protocol

- Report **per-fold** results, not just aggregate
- Use **same folds** across all methods being compared
- Report fold-level statistics to show variance

## Workflow

### Starting a New Paper

```python
from research_agent.config import ProjectConfig
from research_agent.docx_builder import register_paper, PaperConfig

# 1. Configure project
cfg = ProjectConfig(
    name="my-study",
    papers={1: "main-results"},
    constants={"NUM_FOLDS": 5},
)
cfg.ensure_dirs()

# 2. Register paper
register_paper(1, PaperConfig(
    title="Your Paper Title",
    authors="Author One, Author Two\nUniversity Name",
    sections_dir=cfg.paper_sections(1),
    figures_dir=cfg.paper_figures(1),
    drafts_dir=cfg.paper_drafts(1),
))
```

### Writing a Section

1. Create `state/paper1_*/sections/{section}.txt`
2. Write in plain text with inline markers:
   - `[FIGURE: fig1_pipeline]` — inserts figure with registered caption
   - `[TABLE 1: Results]` — inserts registered table
   - `## Subsection Title` — renders as heading
   - `2.1 Numbered Subsection` — renders as numbered heading
3. Compile: `compile_paper(paper_id=1)`

### Computing Statistics

```python
from research_agent.metrics import run_group_comparison

comparison = run_group_comparison(
    our_scores, baseline_scores,
    labels=("Ours", "Baseline"),
)
# Returns: wilcoxon test, paired t-test, descriptive stats, effect size
```

### Generating Figures

```python
from research_agent.figures import create_metric_charts

create_metric_charts(
    groups=[
        {"name": "Ours", "mean": 92.3, "std": 1.5},
        {"name": "Baseline", "mean": 85.2, "std": 3.0},
    ],
    metric_name="Accuracy (%)",
    output_dir=cfg.paper_figures(1),
)
```

### Compiling the Document

```python
from research_agent.docx_builder import compile_paper
paper_path = compile_paper(paper_id=1)
# Output: Times New Roman, 12pt, double-spaced, IMRAD ordered
```

## Self-Learning Feedback Loop

The workflow includes a built-in review-learn-improve cycle powered by
`research-agent`'s feedback module. After writing each section:

### Review → Learn → Improve

```python
from research_agent.feedback import FeedbackLoop, create_review, create_lesson, build_review_checklist
from research_agent.workflow import Workflow

loop = FeedbackLoop(state_dir=Path("state/feedback"))
wf = Workflow.load("state/workflow.json")

# 1. Build a section-specific checklist
checklist = build_review_checklist("methods")

# 2. Review the output (Claude Code scores each criterion 0.0–1.0)
review = create_review(
    task_id="write_methods", iteration=0,
    scores={"methodology": 0.9, "writing_quality": 0.7, "completeness": 0.8},
)
review.strengths = ["Clear protocol description"]
review.weaknesses = ["Missing reproducibility details"]
review.suggestions = ["Add software versions and hyperparameters"]
loop.record_review(review)
wf.set_feedback("write_methods", review.to_dict())

# 3. Extract reusable lessons
lesson = create_lesson(
    source_task="write_methods",
    category="methodology",
    pattern="Methods section missing software/library versions",
    correction="Always list exact versions (e.g., PyTorch 2.1, nnU-Net v2)",
)
loop.record_lesson(lesson)

# 4. Retry if below threshold
if loop.should_retry("write_methods"):
    wf.retry_task("write_methods")  # resets to pending, increments iteration

# 5. Before starting any task, get accumulated guidance
guidance = loop.get_task_guidance("write_results", agent="writer")

loop.save()
wf.save("state/workflow.json")
```

### Workflow DAG with Reviews

```
write_intro → review_intro ─┐
write_methods → review_methods ─┤
write_results → review_results ─┤
write_discussion → review_discussion ─┤
                                      ↓
                              compile_draft → review_draft → apply_learnings
```

Sections are reviewed before compilation, ensuring quality at each stage.
Lessons persist across sessions and are surfaced as guidance on future tasks.

## Quality Checklist

Before submitting any paper, verify:

- [ ] Abstract: states objective, methods, key results, and conclusion
- [ ] Introduction: clearly states the gap and contribution
- [ ] Methods: reproducible by an independent researcher
- [ ] Results: all claims backed by data in tables/figures
- [ ] Discussion: addresses limitations honestly
- [ ] All figures are 300+ DPI with colorblind-safe colors
- [ ] All statistics include test name, test statistic, p-value, effect size
- [ ] All tables have consistent significant figures
- [ ] Reference list matches in-text citations exactly
- [ ] No orphaned figures/tables (every one is referenced in text)
- [ ] Supplementary materials are complete and referenced

## Tools Reference

| Tool | Purpose | Install |
|------|---------|---------|
| [research-agent](https://github.com/bonevisionlabs/research-agent) | Paper toolkit (stats, figures, DOCX) | `pip install research-agent` |
| [claude-scientific-skills](https://github.com/K-Dense-AI/claude-scientific-skills) | 170+ scientific skills for Claude | Install via marketplace |

## Memory Management

Use Claude Code's auto-memory (`~/.claude/projects/{hash}/memory/MEMORY.md`) to persist:

- **Project metrics**: Key experimental results (updated as experiments run)
- **Paper status**: Current state of each paper (section completion, figures done, etc.)
- **Decisions**: Important methodological decisions and their rationale
- **File paths**: Where key data, models, and outputs live
- **Conventions**: Project-specific naming conventions, abbreviations, terminology

**Do NOT store** in memory: raw data, large tables, temporary debugging notes, or anything that changes every session.

## Domain-Specific Notes

> Customize this section for your research domain. Examples:

### For Medical Imaging
- DICOM handling: Use pydicom, always anonymize before processing
- Segmentation metrics: Dice, HD95, ASSD (not just accuracy)
- Ethics: IRB approval number, data use agreement status

### For NLP/ML
- Dataset splits: train/val/test with fixed random seed
- Tokenization: Document which tokenizer and vocab size
- Baselines: Compare against published SOTA, not just random

### For Wet Lab / Experimental
- Protocol references: Link to protocols.io or supplementary
- Reagent tracking: Catalog numbers, lot numbers, vendors
- N values: Biological replicates vs technical replicates clearly distinguished
