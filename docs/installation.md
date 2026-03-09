# Installation Guide

Complete setup guide for configuring Claude Code as a scientific research assistant.

## Prerequisites

- [Claude Code](https://claude.ai/claude-code) installed and authenticated
- Python >= 3.10
- Git

## Step 1: Install research-agent

The Python toolkit for statistics, figures, and document compilation.

```bash
# From PyPI
pip install research-agent

# Or from source
git clone https://github.com/bonevisionlabs/research-agent.git
cd research-agent
pip install -e .
```

Verify installation:

```python
from research_agent.config import ProjectConfig
from research_agent.metrics import compute_descriptive_stats
print("research-agent installed successfully")
```

## Step 2: Install claude-scientific-skills (Optional)

170+ scientific skills for Claude Code by K-Dense Inc.

```bash
# Clone the skills library
git clone https://github.com/K-Dense-AI/claude-scientific-skills.git
```

Install via the Claude Code marketplace, or follow the instructions in the
[claude-scientific-skills README](https://github.com/K-Dense-AI/claude-scientific-skills#installation).

## Step 3: Add CLAUDE.md to Your Project

```bash
# Download the template
curl -o CLAUDE.md https://raw.githubusercontent.com/bonevisionlabs/claude-research-config/main/CLAUDE.md
```

Or copy it manually from this repository into your project root.

### Customize the CLAUDE.md

Open `CLAUDE.md` in your editor and fill in:

1. **Project Overview** — Your project name, domain, current stage
2. **Papers in Progress** — What you're writing and for which venue
3. **Directory Structure** — Adjust paths to match your layout
4. **Domain-Specific Notes** — Add your field's conventions

## Step 4: Configure Claude Code Settings (Optional)

Back up your existing settings first:

```bash
cp ~/.claude/settings.json ~/.claude/settings.json.bak
```

Then apply the optimized settings:

```bash
cp templates/settings-template.json ~/.claude/settings.json
```

### What the settings do

| Setting | Value | Why |
|---------|-------|-----|
| `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` | `75` | Compacts context earlier to preserve working memory for long research sessions |
| `permissions.allow` | Read, Glob, Grep, WebSearch, WebFetch | Auto-allow read-only operations for faster research workflows |

## Step 5: Set Up Project Memory (Optional)

Copy the memory template to your project's Claude memory directory:

```bash
# Find your project's memory directory
# It's at: ~/.claude/projects/{project-hash}/memory/

# Copy the template
cp templates/MEMORY-TEMPLATE.md ~/.claude/projects/{project-hash}/memory/MEMORY.md
```

Claude Code will auto-load this file at the start of every session and update it
as you work.

## Step 6: Initialize a Paper Project

Open Claude Code in your project directory and say:

```
"Set up a new paper project for my [topic] study with [N]-fold cross-validation"
```

Claude Code will read your CLAUDE.md and use research-agent to create the project
structure, configure directories, and prepare for writing.

## Verifying the Setup

Run this quick check to confirm everything works:

```
You: "Show me the research tools available"

Claude Code should:
  → Import research_agent modules successfully
  → Reference your CLAUDE.md standards
  → Know your project structure and paper status
```

## Troubleshooting

### "ModuleNotFoundError: No module named 'research_agent'"

Make sure you installed research-agent in the same Python environment that
Claude Code uses:

```bash
which python  # Check which Python is active
pip install research-agent
```

### Claude Code doesn't read the CLAUDE.md

- The file must be named exactly `CLAUDE.md` (case-sensitive)
- It must be in the project root directory (where you open Claude Code)
- Restart the Claude Code session after adding the file

### Context window fills up quickly

- Use the settings template with `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE: 75`
- Keep MEMORY.md under 200 lines
- Move detailed notes to separate files linked from MEMORY.md
