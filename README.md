# Claude Research Config

**Turn Claude Code into a PhD-level research assistant — in 5 minutes.**

A production-ready configuration for scientific research with Claude Code. Drop the `CLAUDE.md` into any project and Claude immediately knows how to write papers, compute statistics, generate figures, and manage the full manuscript lifecycle.

## What This Is

A single `CLAUDE.md` file that transforms Claude Code from a general-purpose assistant into a research partner with:

- **Behavioral directives** — Claude knows its role, what it must never do, and how to approach every task
- **Workflow protocols** — Step-by-step procedures for writing sections, computing stats, generating figures, compiling papers, and running the feedback loop
- **Quality standards** — IMRAD structure, statistical rigor requirements, figure specs, citation rules
- **A pre-submission checklist** — 11 items verified before any paper ships

Plus optional settings, memory templates, and documentation for day-to-day usage.

## The Architecture

```
┌─────────────────────────────────────────────┐
│              CLAUDE.md (This Repo)           │
│  Behavioral rules, workflow protocols,      │
│  quality standards, guardrails              │
└──────────────────┬──────────────────────────┘
                   │ configures
                   ▼
┌─────────────────────────────────────────────┐
│           Claude Code (The Brain)            │
│  Makes all decisions — writes, reviews,     │
│  learns from feedback, orchestrates          │
└──────────┬──────────────────┬───────────────┘
           │                  │
     imports & calls    reads skill docs
           ▼                  ▼
┌──────────────────┐ ┌───────────────────────┐
│  research-agent  │ │ claude-scientific-     │
│  (Python Tools)  │ │ skills (170+ Skills)  │
│                  │ │                       │
│  metrics.py      │ │ literature-review     │
│  figures.py      │ │ statistical-analysis  │
│  docx_builder.py │ │ scientific-writing    │
│  workflow.py     │ │ pytorch-lightning     │
│  feedback.py     │ │ ... and 166 more      │
└──────────────────┘ └───────────────────────┘
```

| Component | Purpose | Source |
|-----------|---------|--------|
| **CLAUDE.md** | Behavioral config & research standards | This repo |
| **research-agent** | Paper toolkit (stats, figures, DOCX, feedback loop) | [bonevisionlabs/research-agent](https://github.com/bonevisionlabs/research-agent) |
| **claude-scientific-skills** | 170+ domain skills for Claude Code | [K-Dense-AI/claude-scientific-skills](https://github.com/K-Dense-AI/claude-scientific-skills) |

## Quick Start

### 1. Install the tools

```bash
pip install research-agent
```

### 2. Add the CLAUDE.md to your project

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/bonevisionlabs/claude-research-config/main/CLAUDE.md
```

### 3. Customize the top block

Open `CLAUDE.md` and fill in the 5-line project block at the top:

```
Project:  Cell Migration Dynamics
Domain:   Biomedical Imaging
Stage:    writing
Venues:   Nature Methods, IEEE TMI
```

Everything below that line works as-is.

### 4. Start working

```
You: "Set up a new paper project for my cell migration study with 5-fold CV"

Claude Code:
  → Reads CLAUDE.md, understands your standards and workflow
  → Creates project structure, registers paper
  → Ready to write, compute stats, generate figures
```

## What the CLAUDE.md Covers

| Section | What It Does |
|---------|-------------|
| **Role** | Defines Claude as a PhD-level research assistant with clear responsibilities |
| **Critical Rules** | 7 non-negotiable guardrails (no fabrication, no raw data modification, etc.) |
| **Directory Structure** | Where to find and save every type of file |
| **Workflow Protocols** | Step-by-step procedures for writing, stats, figures, compilation, and review |
| **Writing Standards** | Voice, tense, citations, hedging rules |
| **Statistical Rigor** | Test selection, effect sizes, confidence intervals, multiple comparison correction |
| **Quality Checklist** | 11-item pre-submission verification |
| **Coding Conventions** | Python style, naming, path handling |
| **Tools & Memory** | How to use research-agent and persist project state |

## Files

| File | Purpose |
|------|---------|
| [CLAUDE.md](CLAUDE.md) | Drop-in research configuration |
| [templates/settings-template.json](templates/settings-template.json) | Optimized Claude Code settings |
| [templates/MEMORY-TEMPLATE.md](templates/MEMORY-TEMPLATE.md) | Project memory structure |
| [docs/installation.md](docs/installation.md) | Detailed setup guide |
| [docs/workflow-guide.md](docs/workflow-guide.md) | Day-to-day usage patterns |

## Customization

The CLAUDE.md is designed to work out of the box. Common modifications:

- **Citation style** — Switch numbered `[1]` to author-year `(Smith et al., 2024)` for your venue
- **Domain conventions** — Add field-specific metrics, protocols, or reporting standards
- **Quality checklist** — Extend with venue requirements (CONSORT, PRISMA, STROBE, etc.)
- **Statistical defaults** — Change significance level, preferred tests, or effect size measures
- **Directory structure** — Adjust paths to match your existing project layout

## Philosophy

**"Claude Code as Brain"** — one orchestrator, zero multi-agent overhead:

- Claude Code makes all decisions (what to write, how to frame results, when to retry)
- Python tools (`research-agent`) handle I/O and formatting — zero API calls, zero token cost
- State lives on disk (JSON, text, DOCX) — resumable, inspectable, version-controllable
- Configuration as code (`CLAUDE.md`) — reproducible, shareable, forkable

## Origin

Born from a real PhD research pipeline that produced peer-reviewed publications using Claude Code as the primary research assistant. This is the generalized setup — stripped of domain-specific content and ready for any field.

## Contributing

Contributions welcome:

- **Domain templates** — Pre-configured sections for biology, physics, CS, etc.
- **Venue presets** — Quality checklists for Nature, Science, IEEE, etc.
- **Workflow recipes** — Guides for common research tasks
- **Integration guides** — Zotero, Overleaf, OSF, etc.

## License

MIT License. See [LICENSE](LICENSE).
