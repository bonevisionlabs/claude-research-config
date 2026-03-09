# Workflow Guide

Day-to-day patterns for using Claude Code as a scientific research assistant.

## The Core Loop

```
Think → Compute → Write → Compile → Review → Iterate
```

Claude Code handles the middle four steps. You provide the thinking and review.

## Common Workflows

### 1. Start a New Paper

```
You: "Initialize a paper project for my study on [topic].
      We have [N] subjects, [M]-fold cross-validation.
      Target journal: [name]."

Claude Code:
  → Creates ProjectConfig with your parameters
  → Sets up directory structure (sections/, figures/, drafts/)
  → Registers PaperConfig with venue-appropriate defaults
  → Reports: "Project initialized. Ready to write."
```

### 2. Write a Section

```
You: "Write the Methods section. We used [method] with [parameters].
      Here are the key details: [details]."

Claude Code:
  → Reads CLAUDE.md for writing standards (active voice, past tense, etc.)
  → Writes methods section following IMRAD conventions
  → Saves to state/paper1_*/sections/methods.txt
  → Includes inline markers for figures and tables
```

**Pro tip**: Provide raw notes, bullet points, or even voice transcript.
Claude Code will restructure into proper academic prose.

### 3. Compute Statistics

```
You: "Compare model A vs baseline. Here are the per-fold results:
      Model A: [92.1, 93.0, 91.8, 92.5, 92.1]
      Baseline: [85.0, 86.2, 84.8, 85.5, 84.5]"

Claude Code:
  → Runs descriptive stats (mean, std, median, IQR)
  → Runs Wilcoxon signed-rank test (non-parametric)
  → Runs paired t-test with Cohen's d
  → Reports: p-value, effect size, confidence intervals
  → Saves results to results/ directory
```

### 4. Generate Figures

```
You: "Create a bar chart comparing our method vs baseline with error bars."

Claude Code:
  → Uses create_metric_charts() with 300 DPI, Wong palette
  → Saves to state/paper1_*/figures/
  → Reports figure path and caption suggestion
```

### 5. Compile the Paper

```
You: "Compile paper 1 into a Word doc."

Claude Code:
  → Assembles all sections in IMRAD order
  → Inserts figures at [FIGURE: ...] markers
  → Inserts tables at [TABLE N: ...] markers
  → Applies academic formatting (Times New Roman, 12pt, double-spaced)
  → Saves to state/paper1_*/drafts/
```

### 6. Review with Self-Learning Feedback Loop

```
You: "Review the Methods section and record what you learn."

Claude Code:
  → Builds section-specific checklist via build_review_checklist("methods")
  → Scores each criterion (methodology, writing_quality, completeness, etc.)
  → Records a ReviewResult with strengths, weaknesses, and suggestions
  → Extracts reusable Lessons (anti-patterns and corrections)
  → If score < threshold: retries the task with accumulated guidance
  → Lessons persist to state/feedback/ and inform all future tasks
```

The feedback loop means quality improves **across** sessions, not just within
a single review cycle. Before starting any task, Claude Code checks
`feedback_loop.get_task_guidance()` for relevant lessons from past iterations.

```
You: "What has the feedback loop learned so far?"

Claude Code:
  → loop.learning_summary()
  → Shows lessons by category, confidence scores, and top insights
  → loop.iteration_summary()
  → Shows per-task review progress and retry counts
```

### 7. Literature Search

```
You: "Find recent papers on [topic] from the last 2 years."

Claude Code:
  → Uses scientific skills for literature search
  → Returns structured results with titles, authors, abstracts
  → Can generate BibTeX entries for your references
```

## Session Patterns

### Starting a Session

1. Claude Code auto-loads your `CLAUDE.md` and `MEMORY.md`
2. It knows your project state, paper status, and conventions
3. Just pick up where you left off:

```
You: "Continue working on the Discussion section for paper 1."
```

### Ending a Session

Claude Code automatically updates MEMORY.md with:
- New results computed
- Sections completed
- Decisions made
- Next steps identified

### Long Sessions (Context Management)

For sessions that run long:
- The settings template compacts context at 75% (vs default 80%)
- Keep MEMORY.md concise — detailed notes in linked files
- Break large tasks into focused sessions

## Tips

### Let Claude Code Lead

Instead of dictating exact steps, describe what you need:

- **Good**: "Analyze the cross-validation results and determine if our method is significantly better"
- **Less good**: "Run a Wilcoxon test on arrays A and B"

Claude Code reads your CLAUDE.md and chooses the right approach.

### Batch Related Work

Group related tasks for efficiency:

```
You: "For paper 1:
      1. Compute stats for all 3 experiments
      2. Generate comparison figures
      3. Update the Results section with the new numbers
      4. Recompile the paper"
```

### Use Memory for Continuity

When you discover something important:

```
You: "Remember that patient 4 is an outlier — ASSD is 4x higher
      than the mean. We should note this in the Discussion."
```

Claude Code saves this to MEMORY.md for future sessions.

### Iterate on Writing

Academic writing improves through revision:

```
You: "The Introduction is too long. Tighten it to 4 paragraphs
      and make the gap statement more specific."
```

Claude Code rewrites while maintaining your CLAUDE.md standards.
