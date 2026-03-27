# Brown M&M

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that triggers a thorough multi-pass audit of any output when you spot a single error.

## The Origin Story

In 1982, Van Halen's concert rider included a now-famous clause: a bowl of M&Ms backstage, with every brown one removed. Not rock-star vanity. Van Halen's stage setup was complex and heavy enough to damage floors or overload stages if the venue cut corners on the technical specs. If the band arrived and found brown M&Ms in the bowl, they knew the venue had skimmed the contract, which meant the venue might have skimmed the structural and electrical specs too.

One small, easy-to-verify failure became a reliable diagnostic for larger, harder-to-find failures.

## What This Skill Does

When you find a mistake in Claude's output (a wrong timezone, a misspelled name, an incorrect number, a broken reference), that mistake is a brown M&M. The error tells you the overall attention to detail was insufficient, and other errors probably exist.

This skill runs a structured four-pass audit:

1. **Fact verification** - checks every verifiable claim against source material (names, dates, numbers, URLs, quotes)
2. **Internal consistency** - looks for contradictions within the output (different spellings, mismatched numbers, tone shifts)
3. **Logical and structural review** - evaluates whether the output accomplishes what was asked, and whether the logic holds
4. **Fresh-eyes re-read** - one final pass reading as if seeing the output for the first time

After the audit, Claude reports the original error (confirmed fixed), any additional errors found (with which pass caught them), items Claude could not verify against the source material, and a corrected version of the full output.

The skill works on any output type: emails, code, documents, research, slide decks, data, plans.

## Getting the most out of it

Brown M&M's fact verification pass works by checking claims in the output against source material in your conversation. The more context Claude has, the more the skill can verify.

In practice, the skill works best when you've given Claude reference material to work from: meeting notes, source data, a brief, a spec, prior conversation context. If Claude generated an output from a vague prompt with no source material, the fact verification pass has nothing to check against. The skill still runs internal consistency and logic passes, but the full four-pass audit shines when there's a ground truth to compare against.

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

Claude fixes the property name, then walks through the full code output. Imports, types, logic, edge cases, hardcoded values. The four-pass structure catches different categories of error at each stage.

## When to use Brown M&M vs. a dedicated debugging tool

Brown M&M is a generalist. You spot one error in any output (an email, a report, a plan, a piece of code) and the skill audits the whole thing. For most AI-generated content, where the failure mode is scattered inaccuracies across an output, Brown M&M is the right tool.

For code specifically, more specialized tools exist. If you find a logic bug and need root cause analysis, stack tracing, and structured repair, reach for a dedicated debugging skill instead. Brown M&M catches surface errors across an entire output (wrong variable names, mismatched imports, hardcoded values that don't match the spec). Debugging tools go deeper on a single defect.

Some good ones:

- [compound-engineering](https://github.com/EveryInc/compound-engineering-plugin) by Every - 13 skills covering the full development lifecycle, including `ce-review` (12 parallel review agents) and `systematic-debugging` (structured root cause analysis)
- [superpowers](https://github.com/obra/superpowers) by Jesse Vincent - agentic development framework with structured code review, TDD enforcement, and severity-gated issue reporting

Brown M&M and these tools solve different problems. I use Brown M&M when I find a small error and want confidence that nothing else is wrong. Use debugging tools when you find a real bug and need to understand why.

## Status

The core four-pass audit structure is stable. Trigger conditions and reporting format may evolve based on real-world usage. Feedback and contributions welcome.

## License

MIT License. See [LICENSE](LICENSE).
