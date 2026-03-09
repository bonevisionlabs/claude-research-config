# Claude Research Setup

**Turn Claude Code into a PhD-level research assistant — in 5 minutes.**

A ready-to-use configuration for scientific research with Claude Code. Drop the `CLAUDE.md` into any project and Claude immediately understands how to write papers, compute statistics, generate figures, and compile manuscripts.

## What This Is

A configuration template that transforms Claude Code from a general-purpose coding assistant into a domain-aware scientific research partner. It provides:

- **CLAUDE.md** — Project-level instructions that teach Claude Code your research workflow, writing standards, and quality requirements
- **Settings template** — Optimized Claude Code settings for research work
- **Memory template** — Structure for persistent project memory across sessions
- **Workflow guide** — How to use the full stack day-to-day

## Why This Exists

Claude Code is powerful out of the box, but it doesn't know your field's conventions, your paper's structure, or your statistical standards — unless you tell it. Most researchers either:

1. Repeat the same instructions every session
2. Get inconsistent output quality
3. Don't leverage Claude Code's full capability

This setup solves all three by encoding research best practices into configuration files that persist across sessions.

## The Stack

```
┌─────────────────────────────────────────────┐
│              CLAUDE.md (This Repo)           │
│  Project instructions, writing standards,    │
│  quality checklists, workflow definitions     │
└──────────────────┬──────────────────────────┘
                   │ configures
                   ▼
┌─────────────────────────────────────────────┐
│           Claude Code (The Brain)            │
│  Makes decisions, writes prose, orchestrates │
└──────────┬──────────────────┬───────────────┘
           │                  │
     imports & calls    reads skill docs
           ▼                  ▼
┌──────────────────┐ ┌───────────────────────┐
│  research-agent  │ │ claude-scientific-     │
│  (Python Tools)  │ │ skills (170+ Skills)  │
│                  │ │                       │
│  figures.py      │ │ literature-review     │
│  metrics.py      │ │ statistical-analysis  │
│  docx_builder.py │ │ scientific-writing    │
│  workflow.py     │ │ pytorch-lightning     │
│                  │ │ ... and 166 more      │
└──────────────────┘ └───────────────────────┘
```

| Component | Purpose | Source |
|-----------|---------|--------|
| **CLAUDE.md** | Project configuration & standards | This repo |
| **research-agent** | Paper toolkit (stats, figures, DOCX) | [bonevisionlabs/research-agent](https://github.com/bonevisionlabs/research-agent) |
| **claude-scientific-skills** | 170+ domain skills for Claude Code | [K-Dense-AI/claude-scientific-skills](https://github.com/K-Dense-AI/claude-scientific-skills) |

## Quick Start

### 1. Install the tools

```bash
# Python toolkit for paper compilation
pip install research-agent

# Scientific skills (install via Claude Code marketplace or clone)
# See: https://github.com/K-Dense-AI/claude-scientific-skills
```

### 2. Copy the CLAUDE.md

```bash
# Option A: Clone this repo and copy
git clone https://github.com/bonevisionlabs/claude-research-setup.git
cp claude-research-setup/CLAUDE.md /path/to/your/project/

# Option B: Download just the file
curl -o CLAUDE.md https://raw.githubusercontent.com/bonevisionlabs/claude-research-setup/main/CLAUDE.md
```

### 3. Customize for your project

Open `CLAUDE.md` and fill in:

- **Project Overview**: Your project name, domain, target venues
- **Papers in Progress**: Current manuscripts and their status
- **Domain-Specific Notes**: Conventions for your field

### 4. Apply the settings (optional)

```bash
# Copy optimized settings (back up your existing settings first)
cp claude-research-setup/templates/settings-template.json ~/.claude/settings.json
```

### 5. Start working

```
You: "Set up a new paper project for my cell migration study with 10 bootstraps"

Claude Code:
  → Reads CLAUDE.md, understands your standards
  → from research_agent.config import ProjectConfig
  → Creates project structure, registers paper
  → Ready to write, compute stats, generate figures
```

## What the CLAUDE.md Does

When Claude Code starts a session in your project, it reads `CLAUDE.md` and learns:

| Section | What Claude Learns |
|---------|-------------------|
| **Project Overview** | What you're working on, what stage you're at |
| **Directory Structure** | Where to find and save files |
| **Coding Conventions** | Your style preferences, Python standards |
| **Writing Standards** | IMRAD structure, voice, tense, citation format |
| **Figure Standards** | 300 DPI, colorblind palette, naming conventions |
| **Table Standards** | Formatting, significant figures, bold conventions |
| **Statistical Analysis** | Which tests to use, what to report |
| **Workflow** | How to use research-agent for each task |
| **Quality Checklist** | What to verify before submission |
| **Memory Management** | What to persist across sessions |

## Templates

| File | Purpose |
|------|---------|
| [CLAUDE.md](CLAUDE.md) | Drop-in project configuration |
| [templates/settings-template.json](templates/settings-template.json) | Optimized Claude Code settings |
| [templates/MEMORY-TEMPLATE.md](templates/MEMORY-TEMPLATE.md) | Project memory structure |
| [docs/installation.md](docs/installation.md) | Detailed setup guide |
| [docs/workflow-guide.md](docs/workflow-guide.md) | Day-to-day usage patterns |

## Customization

The CLAUDE.md is designed to be forked and customized. Common modifications:

### Change the citation style
Replace the numbered reference format with author-year (APA) or any other style required by your target venue.

### Add domain-specific sections
The "Domain-Specific Notes" section at the bottom is a blank canvas. Add your field's conventions, required metrics, reporting standards, etc.

### Extend the quality checklist
Add venue-specific requirements (e.g., CONSORT for clinical trials, PRISMA for systematic reviews).

### Adjust statistical defaults
Change the default significance level, preferred tests, or effect size measures for your field.

## Philosophy

This setup follows the **"Claude Code as Brain"** architecture:

- **One orchestrator** (Claude Code) makes all decisions
- **Pure Python tools** (research-agent) do computation — zero API calls, zero token overhead
- **State on disk** (JSON, text files, DOCX) — resumable, inspectable, version-controllable
- **Configuration as code** (CLAUDE.md) — reproducible, shareable, forkable

No multi-agent frameworks. No token-burning coordination layers. Just a well-configured LLM with the right tools.

## Origin

Born from a real PhD research pipeline that produced peer-reviewed publications using Claude Code as the primary research assistant. This is the generalized setup pattern — stripped of domain-specific content and ready for any field.

## Contributing

Contributions welcome! Areas where help is needed:

- **Domain templates** — Pre-configured CLAUDE.md sections for specific fields (biology, physics, CS, etc.)
- **Venue presets** — Quality checklists for specific journals (Nature, Science, IEEE, etc.)
- **Workflow recipes** — Step-by-step guides for common research tasks
- **Integration guides** — How to combine with other tools (Zotero, Overleaf, etc.)

## License

MIT License. See [LICENSE](LICENSE).
