# Brown M&M

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that triggers a thorough multi-pass audit of any output when you spot a single error.

## The Origin Story

In 1982, Van Halen's concert rider included a now-famous clause: a bowl of M&Ms backstage, with every brown one removed. Not rock-star vanity. Van Halen's stage setup was complex and heavy enough to damage floors or collapse stages if the venue cut corners on the technical specs. If the band arrived and found brown M&Ms in the bowl, they knew the venue had skimmed the contract, which meant the venue might have skimmed the structural and electrical specs too.

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

### Scenario 2: Incorrect variable in generated code

Claude generates a function that references `user.email` when it should be `user.emailAddress`.

```
You: "That property name is wrong. What else did you get wrong?"
```

Claude fixes the property name, then walks through the full code output. Imports, types, logic, edge cases, hardcoded values. The four-pass structure catches different categories of error at each stage.

### Scenario 3: Bad number in a report

Claude produces an analysis that says "revenue grew 12%" when the source data shows 8%.

```
You: "The growth number is wrong, it's 8%, not 12%. Audit the rest."
```

Claude fixes the percentage and re-reads every number, claim, and comparison in the report against the source data. Anything Claude cannot verify gets flagged explicitly.

## Status

The core four-pass audit structure is stable. Trigger conditions and reporting format may evolve based on real-world usage. Feedback and contributions welcome.

## License

MIT License. See [LICENSE](LICENSE).
