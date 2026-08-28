---
name: understand-pr
description: Build a concise, human-oriented mental model of a pull request before merging it. Use when the user wants to understand what a PR actually changed, how the important code flows work, and which small subset of files they should read.
---

# Understand PR

Help the human owner understand a pull request before they merge it.

This is a **comprehension task, not a code review**.

Assume automated or agentic code review has already happened.

Your job is to reduce a potentially large PR into the smallest useful mental model the human needs to confidently understand what is entering their codebase.

## Input

Usually invoked with:

`/understand-pr <PR URL>`

Use the PR URL to inspect:

- the PR description
- linked issue or specification when available
- commits
- changed files
- relevant surrounding code
- tests related to the change

Do not limit yourself to the diff when unchanged surrounding code is needed to understand the flow.

## Core rules

### Optimize for understanding

Do not explain every changed file.

Find the small subset of code that explains most of the change.

For a large PR, aim to reduce dozens of changed files into roughly 5–10 useful human reading targets when possible.

### Use simple technical English

Be technically precise, but easy to read.

Prefer:

> `VoteService` now owns vote transitions.

Over:

> The implementation introduces an abstraction responsible for orchestrating vote state transitions.

Use:

- short sentences
- familiar engineering terms
- concrete names from the code
- short paragraphs
- direct explanations

Avoid:

- unnecessary jargon
- corporate language
- long sentences
- repeating the same idea in different words
- explaining obvious syntax

Do not simplify away important technical meaning.

### Do not perform another code review

Do not hunt for:

- style problems
- naming issues
- refactoring opportunities
- generic best-practice violations
- speculative bugs

Another review process should handle those.

If you discover something genuinely concerning while tracing the code, mention it briefly at the end under **Worth double-checking**.

Do not let this section dominate the report.

## Process

### 1. Understand the intent

Determine:

- what problem the PR is solving
- what behavior existed before
- what behavior should exist after

If a linked issue or specification exists, use it.

Do not rely only on the PR author's summary.

### 2. Reconstruct what was actually implemented

Trace the relevant code.

Identify the main execution or data flow.

Examples:

`UI → hook → API → service → repository → database`

or:

`request → controller → use case → domain → persistence`

or:

`simulation → action → service → repository`

Use the structure that actually exists in the repository.

### 3. Separate meaningful changes from supporting changes

Distinguish between:

- behavioral changes
- architectural changes
- wiring
- types
- generated files
- fixtures
- formatting
- migrations
- test support

Do not give supporting changes equal attention unless they matter to understanding the PR.

### 4. Build the recommended reading path

Choose the files, functions, or changed sections the human should inspect.

Order them by the **best reading sequence**, not alphabetically and not merely by importance.

Optimize for minimum mental jumping.

A common sequence is:

`entry point → orchestration/business logic → downstream dependency → persistence/external effect → tests`

But adapt this to the actual change.

The most important file does not always need to be first.

Sometimes an entry point provides the context needed to understand the core logic.

For each reading target, include:

- file path
- relevant function/class when useful
- a short role label
- why to read it
- what specifically to notice

Example:

**1. `vote-action.ts` — START HERE**

This is where the changed vote flow begins.

Focus on what happens when a vote already exists.

**2. `vote-service.ts` — CORE LOGIC**

This contains the transition rules.

Understand how upvote, downvote, and no-vote states change.

### 5. Link directly when reliable

When possible, make each recommended reading target clickable.

Prefer:

1. a reliable link to the relevant PR diff or changed lines
2. otherwise, a link to the file at the PR head commit
3. otherwise, show the exact repository-relative path

Never invent or guess a URL that may not work.

Plain file paths are better than broken links.

### 6. Explain the tests

Identify the tests that best describe the new behavior.

Explain what each important test proves in plain English.

Do not enumerate every test.

### 7. Produce the human mental model

End by reducing the whole PR into a few things the human should remember.

The goal is:

> “I now understand what I am merging.”

Not:

> “An AI told me the code is correct.”

## Output format

Keep the report concise unless the PR genuinely requires more detail.

### PR in one minute

Explain the PR in a few short paragraphs.

Cover:

- why it exists
- before
- after
- how it was implemented at a high level

### Main flow

Show the primary execution/data flow.

Example:

`VoteAction → VoteService → VoteRepository → DB`

Briefly explain each step only when needed.

### What actually changed

Summarize the meaningful behavioral and architectural changes.

Ignore mechanical noise.

### Recommended reading order

Provide the Pareto reading path.

Use contextual labels such as:

- `START HERE`
- `CORE LOGIC`
- `BOUNDARY`
- `PERSISTENCE`
- `SIDE EFFECT`
- `VERIFY`

Explain what the human should understand from each target.

Explicitly mention when the remaining changed files are mostly supporting or mechanical and do not need line-by-line inspection.

### Tests that matter

Explain the few tests that best prove the intended behavior.

### Keep these in your head

End with 3–5 concise facts the human should remember about the PR.

### Worth double-checking

Only include this section when something genuinely deserves human attention.

Do not turn it into a second code review.

## Follow-up behavior

After presenting the map, be ready to drill into any part of it.

For example:

- “Explain step 2 more deeply.”
- “Why was this service changed?”
- “Show me where this state transition happens.”
- “Why does this repository method need to change?”
- “Walk me through this test.”
- “Trace what happens when the user clicks this button.”

When answering follow-ups, preserve the same simple technical English.