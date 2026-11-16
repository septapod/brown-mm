# Brown M&M

![Brown M&M](brown-mm.png)

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that triggers a thorough multi-pass audit of any output when you spot a single error.

## The Origin Story

In 1982, Van Halen's concert rider included a now-famous clause: a bowl of M&Ms backstage, with every brown one removed. Not rock-star vanity. Van Halen's stage setup was complex and heavy enough to damage floors or overload stages if the venue cut corners on the technical specs. If the band arrived and found brown M&Ms in the bowl, they knew the venue had skimmed the contract, which meant the venue might have skimmed the structural and electrical specs too.

One small, easy-to-verify failure became a reliable diagnostic for larger, harder-to-find failures.

## What This Skill Does

When you find a mistake in Claude's output (a wrong timezone, a misspelled name, an incorrect number, a broken reference), that mistake is a brown M&M. The error tells you the overall attention to detail was insufficient, and other errors probably exist.

This skill runs a structured audit:

1. **The error's own class first** - a wrong timezone means every time, date, and weekday is compared with the source before anything else. Siblings of the error you found are the ones most likely to have survived.
2. **Facts** - every claim that has a source is compared with that source again, using tools: re-reading the file, recomputing the number, fetching the link.
3. **Consistency** - the output is read against itself for contradictions (different spellings, numbers that disagree, a duration that does not match its start and end times).
4. **Logic** - whether each conclusion, ranking, and stated cause follows from the figures.
5. **A reader who has not seen the reasoning** - a fresh subagent where one is available, since a re-read in the same conversation shares the blind spots that produced the error.

One rule holds the audit together: a claim counts as verified only when Claude can show the source line or the tool result. Memory of the source is the thing that failed the first time.

After the audit, Claude reports:

- the original error, its fix, and the other errors it found, each with the source line that proves it
- statements that nothing in the source supports, and anything it could not reach
- a proof list: every claim it compared, beside its source, marked match, mismatch, or no source
- the corrected output

The skill works on any output type: emails, code, documents, research, slide decks, data, plans.

## Getting the most out of it

Brown M&M's fact pass works by checking claims in the output against source material in your conversation. The more context Claude has, the more the skill can verify.

In practice, the skill works best when you've given Claude reference material to work from: meeting notes, source data, a brief, a spec, prior conversation context. If Claude generated an output from a vague prompt with no source material, the fact pass has nothing to compare against. The skill still runs the consistency and logic passes, and it lists every unsupported statement as "no source" so you can see how much of the output rests on nothing. The full audit works best when there is source material to compare against.

## Installation

### Prerequisites

You need [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and working.

### Install the skill

1. Create the skill directory:
   ```bash
   mkdir -p ~/.claude/skills/brown-mm
   ```

2. Copy `SKILL.md` into that directory:
   ```bash
   cp SKILL.md ~/.claude/skills/brown-mm/SKILL.md
   ```

That's it. Claude Code automatically discovers skills in `~/.claude/skills/`. The skill will be available in your next session.

### Verify it's installed

Start a new Claude Code session and look for `brown-mm` in the skill list, or just try using it (see examples below).

## Usage Examples

### Scenario 1: Wrong detail in an email draft

You ask Claude to draft an email referencing a meeting on Tuesday at 2pm EST. Claude writes "Tuesday at 2pm PST" instead.

```
You: "That timezone is wrong, it should be EST. Brown M&M this."
```

Claude fixes the timezone, then audits the entire email. Names, dates, subject line, recipient match, tone. Everything gets checked against the source material you provided.

### Scenario 2: Bad number in a report

Claude produces an analysis that says "revenue grew 12%" when the source data shows 8%.

```
You: "The growth number is wrong, it's 8%, not 12%. Audit the rest."
```

Claude fixes the percentage and re-reads every number, claim, and comparison in the report against the source data. Anything Claude cannot verify gets flagged explicitly.

### Scenario 3: Incorrect variable in generated code

Claude generates a function that references `user.email` when it should be `user.emailAddress`.

```
You: "That property name is wrong. What else did you get wrong?"
```

Claude fixes the property name, then walks through the full code output. Imports, types, logic, edge cases, hardcoded values. Each pass catches a different kind of error.

## When to use Brown M&M vs. a dedicated debugging tool

Brown M&M is a generalist. You spot one error in any output (an email, a report, a plan, a piece of code) and the skill audits the whole thing. For most AI-generated content, where the failure mode is scattered inaccuracies across an output, Brown M&M is the right tool.

For code specifically, more specialized tools exist. If you find a logic bug and need root cause analysis, stack tracing, and structured repair, reach for a dedicated debugging skill instead. Brown M&M catches surface errors across an entire output (wrong variable names, mismatched imports, hardcoded values that don't match the spec). Debugging tools go deeper on a single defect.

Some good ones:

- [compound-engineering](https://github.com/EveryInc/compound-engineering-plugin) by Every - 13 skills covering the full development lifecycle, including `ce-review` (12 parallel review agents) and `systematic-debugging` (structured root cause analysis)
- [superpowers](https://github.com/obra/superpowers) by Jesse Vincent - agentic development framework with structured code review, TDD enforcement, and severity-gated issue reporting

Brown M&M and these tools solve different problems. I use Brown M&M when I find a small error and want confidence that nothing else is wrong. I use debugging tools when I find a real bug and need to understand why.

## Models

I tested this version on Haiku 5.5 and Opus 5.5. Both caught the errors I planted.

I tend to run it on Opus 5.5 or something similar, in Claude Code and Codex. That's what I have open when I spot a mistake.

## Status

This is the second version of the skill. The first ran four passes and reported what it found. This one adds the sweep of the error's own class, the rule that a claim counts as verified only with its source shown, and the proof list. Feedback and contributions welcome.

## License

MIT License. See [LICENSE](LICENSE).
