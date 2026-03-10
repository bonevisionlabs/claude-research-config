# Workflow Guide

Day-to-day patterns for using Claude Code as a scientific research assistant.

## The Core Loop

```
Think → Compute → Write → Compile → Review → Iterate
```

You provide the thinking and review. Claude handles compute, write, compile, and the feedback loop.

## Workflows

### Start a new paper

```
You: "Initialize a paper project for my study on [topic].
      We have [N] subjects, [M]-fold cross-validation.
      Target journal: [name]."
```

Claude creates the project structure, registers the paper config, and is ready to write.

### Write a section

```
You: "Write the Methods section. We used [method] with [parameters]."
```

Claude follows the CLAUDE.md workflow protocol: checks feedback history, reads relevant data, writes in IMRAD format with inline figure/table markers, self-reviews against the quality checklist, and saves to `state/paper{N}_*/sections/`.

**Tip**: Provide raw notes, bullet points, or voice transcripts. Claude restructures into academic prose.

### Compute statistics

```
You: "Compare our method vs baseline. Per-fold Dice scores:
      Ours:     [0.978, 0.981, 0.973, 0.980, 0.976]
      Baseline: [0.945, 0.950, 0.940, 0.948, 0.943]"
```

Claude runs the full statistical pipeline from the CLAUDE.md protocol: descriptive stats, normality check, appropriate test, effect size, confidence intervals, per-fold reporting. Results saved as JSON.

### Generate figures

```
You: "Create a bar chart comparing our method vs baseline with error bars."
```

Claude uses the Wong (2011) colorblind-safe palette, 300+ DPI, >= 8pt font. Saves to `state/paper{N}_*/figures/`.

### Compile a paper

```
You: "Compile paper 1 into a Word doc."
```

Claude verifies all sections and referenced figures exist, assembles in IMRAD order, applies academic formatting (Times New Roman, 12pt, double-spaced), and saves to `state/paper{N}_*/drafts/`.

### Review with feedback loop

```
You: "Review the Methods section and record what you learn."
```

Claude scores each criterion 0.0–1.0, records strengths/weaknesses/suggestions, extracts reusable lessons, and retries if score < 0.7. Lessons persist across sessions — quality improves over time.

```
You: "What has the feedback loop learned so far?"
```

Claude shows lessons by category, confidence scores, and top insights.

### Search literature

```
You: "Find recent papers on [topic] from the last 2 years."
```

Claude uses scientific skills for structured literature search with BibTeX generation.

## Session Patterns

### Starting a session

Claude auto-loads your `CLAUDE.md` and `MEMORY.md`. Just pick up where you left off:

```
You: "Continue working on the Discussion section for paper 1."
```

### Ending a session

Claude updates MEMORY.md with new results, completed sections, decisions, and next steps.

### Long sessions

The settings template compacts context at 75% to preserve working memory. Keep MEMORY.md concise and move detailed notes to linked files.

## Tips

**Let Claude lead.** Describe what you need, not the exact steps:
- Good: "Analyze the cross-validation results and determine if our method is significantly better"
- Less good: "Run a Wilcoxon test on arrays A and B"

**Batch related work.** Group tasks for efficiency:
```
You: "For paper 1: compute stats for all experiments, generate comparison figures,
      update the Results section, and recompile."
```

**Use memory for continuity.** When you discover something important:
```
You: "Remember that patient 4 is an outlier — ASSD is 4x the mean.
      Note this in the Discussion."
```

**Iterate on writing.** Academic writing improves through revision:
```
You: "The Introduction is too long. Tighten to 4 paragraphs
      and make the gap statement more specific."
```
