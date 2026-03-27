# Brown M&M

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that triggers a thorough multi-pass audit of any output when you spot a single error.

## The Origin Story

In 1982, Van Halen's concert rider included a now-famous clause: a bowl of M&Ms backstage, with every brown one removed. This was not rock-star vanity. Van Halen's stage setup was complex and heavy enough to damage floors or collapse stages if the venue did not follow the technical specifications exactly. The brown M&M clause was buried deep in the rider. If the band arrived and found brown M&Ms in the bowl, they knew the venue had skimmed the contract. That meant the structural and electrical specs might have been skimmed too.

One small, easy-to-verify failure became a reliable diagnostic for larger, harder-to-find failures.

## What This Skill Does

When you find a mistake in Claude's output (a wrong timezone, a misspelled name, an incorrect number, a broken reference), that mistake is a brown M&M. It tells you the overall attention to detail was insufficient, and other errors probably exist.

This skill runs a structured four-pass audit:

1. **Fact verification** - Checks every verifiable claim against source material (names, dates, numbers, URLs, quotes)
2. **Internal consistency** - Looks for contradictions within the output (different spellings, mismatched numbers, tone shifts)
3. **Logical and structural review** - Evaluates whether the output actually accomplishes what was asked, and whether the logic holds
4. **Fresh-eyes re-read** - One final pass reading as if seeing the output for the first time

After the audit, Claude reports: the original error (confirmed fixed), any additional errors found (with which pass caught them), items that could not be verified, and a corrected version of the full output.

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

Claude fixes the timezone, then audits the entire email: Are the names right? Is the date correct? Does the subject line match the content? Is the tone appropriate for the recipient?

### Scenario 2: Incorrect variable in generated code

Claude generates a function that references `user.email` when it should be `user.emailAddress`.

```
You: "That property name is wrong. What else did you get wrong?"
```

Claude fixes the property name, then audits the full code output: Do all imports resolve? Are types consistent? Does the logic handle edge cases? Are there other hardcoded values that might be wrong?

### Scenario 3: Bad number in a report

Claude produces an analysis that says "revenue grew 12%" when the source data shows 8%.

```
You: "The growth number is wrong — it's 8%, not 12%. Audit the rest."
```

Claude fixes the percentage, then checks every other number, percentage, and claim in the report against the source material. Flags anything it cannot verify.

## Status

This skill is actively being iterated on. The core four-pass audit structure is stable, but the trigger conditions and reporting format may evolve based on real-world usage.

## License

MIT License. See [LICENSE](LICENSE).
