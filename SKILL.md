---
name: brown-mm
description: "Run a full, evidence-backed audit of an output after the user spots one error in it. Use when the user points out a mistake (a wrong time zone, a misspelled name, a wrong number, a broken link, a wrong variable) and wants to know what else is wrong, or says 'brown M&M', 'what else did you get wrong', or 'audit this'. Works on any output: emails, code, documents, research, decks, data, plans."
---

# Brown M&M

Van Halen's 1982 concert rider required a bowl of M&Ms backstage with every brown one removed. Brown M&Ms in the bowl meant the venue had not read the contract, so the stage-safety specs might be wrong too. One small, easy-to-see miss is evidence about the misses that are hard to see.

When the user spots an error in your output, that error is the brown M&M. Your attention on this output was lower than you believed, and other errors probably exist. Set your confidence in the whole output to zero. You are looking for what else is wrong.

## The one rule that makes this audit worth trusting

**A check counts only when you can show it.** For every claim you call verified, you must have looked at the source again during this audit, with a tool, and you must be able to quote the source line or show the tool result. Memory of the source is the thing that already failed once. If you did not re-open the source, the claim is "not checked," and you say so.

## Step 1: Name the error's class

Say in one line what kind of error the user found. Use this list.

| Class | What to sweep |
|---|---|
| Time | Every time, date, weekday, time zone, duration, and deadline. Confirm each weekday matches its date. |
| Name | Every person, company, product, and place name, plus every title and role. Compare spelling character by character. |
| Number | Every amount, count, percentage, total, and average. Recompute every figure that is derived from other figures. |
| Reference | Every URL, email address, file path, ID, version, and citation. Compare character by character. Fetch links when you can. |
| Quote | Every quotation, paraphrase, and attribution, against the exact words of the source. |
| Logic | Every conclusion, comparison, ranking ("highest," "first"), and cause. |
| Invention | Every statement with no basis in the source or the conversation. |

## Step 2: Size the audit

- **Short output (under about 150 words, low stakes):** run Step 3, then Passes 1 and 2.
- **Everything else:** run every pass.

If you are unsure, run every pass.

## Step 3: Sweep the error's own class first

The reported error shows exactly where your attention failed. Before anything else, find every other item of the same class in the output and check each one against its source. Siblings of the reported error are the errors most likely to have survived.

## The passes

**Pass 1: Facts.** Go line by line. List every claim that could be checked. Check each against the source with a tool: re-read the file, recompute the number, fetch the URL. A claim with no source goes in the report as "no source," never as verified.

**Pass 2: Consistency.** Read the output against itself. Look for the same thing under two names or spellings, numbers that disagree between sections, counts that do not match their lists, durations that do not match their start and end times, and a greeting or sign-off that does not match the people involved. In code: names, signatures, imports, and types must agree.

**Pass 3: Logic.** Does the output do what was asked? Does each conclusion follow from the figures? Is every ranking and comparison right when you recompute it? Is any cause or explanation stated that the source never gave? In code: does the logic do what it claims, including empty inputs, nulls, and boundaries?

**Pass 4: A reader who has not seen your reasoning.** Your own re-read shares the blind spots that produced the error. If you can start a subagent or a fresh session, give it only the output and the source material, with no summary of your audit, and ask it to list anything wrong or unsupported. Add what it finds. If you cannot, re-read the output from the top as its recipient, and say in the report that this pass ran in the same context.

## The report

Deliver these, in order.

1. **The reported error:** restated, its class named, and the fix.
2. **Other errors found:** for each one, the wrong text, the correct text, the source line that proves it (quoted), and the pass that caught it.
3. **No source:** statements in the output that nothing in the source or conversation supports. Recommend removing each one or asking the user to confirm it.
4. **The proof list:** one line per claim you compared, in this form:
   `claim in the output | source line or tool result | match / mismatch / no source`
   Include the claims that matched. The proof list is how the user sees that the audit happened.
5. **Not checked:** anything you could not reach (a link you could not fetch, a file you do not have), with the reason.
6. **The corrected output,** ready to use. If the output lives in a file, fix the file and say where it is.

If the audit finds nothing beyond the reported error, say so plainly. The proof list still goes in the report.

## Rules

- **Confirm every correction against the source before you apply it.** A wrong fix is worse than the first error.
- **Stay on accuracy.** No restyling, no rewriting, and no additions unless the user asks.
- **Report plainly.** A clean audit and a long list of errors get the same tone.
- **Never write "verified" for something you did not re-open.**
