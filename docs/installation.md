# Installation

## Prerequisites

- [Claude Code](https://claude.ai/claude-code) installed and authenticated
- Python >= 3.10

## 1. Install research-agent

```bash
pip install research-agent
```

Verify:

```python
from research_agent.config import ProjectConfig
from research_agent.metrics import compute_descriptive_stats
print("OK")
```

## 2. Add CLAUDE.md to your project

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/bonevisionlabs/claude-research-config/main/CLAUDE.md
```

Or copy it manually from this repo into your project root.

## 3. Fill in the project block

Open `CLAUDE.md` and edit the 5-line block at the top:

```
Project:  Cell Migration Dynamics
Domain:   Biomedical Imaging
Stage:    writing
Venues:   Nature Methods, IEEE TMI
```

Add your papers to the table below it. Everything else works as-is.

## 4. Install claude-scientific-skills (optional)

170+ scientific slash commands for Claude Code.

```bash
git clone https://github.com/K-Dense-AI/claude-scientific-skills.git
```

Follow the [installation instructions](https://github.com/K-Dense-AI/claude-scientific-skills#installation) or install via the Claude Code marketplace.

## 5. Apply optimized settings (optional)

```bash
cp ~/.claude/settings.json ~/.claude/settings.json.bak
cp templates/settings-template.json ~/.claude/settings.json
```

This sets:
- **Auto-compact at 75%** — preserves working memory during long research sessions
- **Auto-allow read-only tools** — faster research workflows (Read, Glob, Grep, WebSearch, WebFetch)

## 6. Set up project memory (optional)

```bash
cp templates/MEMORY-TEMPLATE.md ~/.claude/projects/{project-hash}/memory/MEMORY.md
```

Claude Code auto-loads this at session start and updates it as you work. Find your project hash in `~/.claude/projects/`.

## Start working

```
You: "Set up a new paper project for my cell migration study with 5-fold CV"
```

Claude reads your CLAUDE.md, creates the project structure, and is ready to write.

## Troubleshooting

**ModuleNotFoundError: No module named 'research_agent'**
Install in the same Python environment Claude Code uses: `which python && pip install research-agent`

**Claude doesn't read the CLAUDE.md**
- File must be named exactly `CLAUDE.md` (case-sensitive)
- Must be in the project root (where you open Claude Code)
- Restart the session after adding it

**Context window fills up**
- Apply the settings template (auto-compact at 75%)
- Keep MEMORY.md under 200 lines
- Move detailed notes to linked files
