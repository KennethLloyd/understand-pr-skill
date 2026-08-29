---
name: understand-pr
description: Explain a pull request when the user asks what it changed, how its code flows, or what to read before merging. Produce a concise mental model and Pareto reading path; keep code-review findings out of scope.
---

# Understand PR

Help the human owner understand a pull request before they merge it.

This is a comprehension task. Reduce a potentially large PR to the smallest useful mental model the human needs to understand what is entering their codebase.

## Input

Usually invoked with:

`/understand-pr <PR URL>`

Use the PR URL to inspect:

- the PR description
- a linked issue or specification when available
- commits
- changed files
- relevant surrounding code
- tests related to the change

Cross-check the author's summary against the issue/specification and the implementation. Read unchanged surrounding code whenever the flow depends on it.

## Core rules

### Optimize for signal

Find the minimum set of code that explains most of the change.

For a large PR, usually target 5–10 useful reading targets. Use fewer for a smaller PR. Exceed 10 only when each additional target explains a distinct part of the mental model.

Give behavioral and architectural changes the detail they need. Summarize wiring, types, generated files, fixtures, formatting, migrations, and test support when they do not materially change the mental model.

### Write plainly

Be technically precise and easy to read:

- use short sentences and paragraphs
- prefer familiar engineering terms
- name concrete code elements
- preserve technical meaning while simplifying language

Prefer:

> `VoteService` now owns vote transitions.

Over:

> The implementation introduces an abstraction responsible for orchestrating vote state transitions.

### Stay in comprehension scope

Spend attention on intent, behavior, flow, reading order, and tests. Put only a concrete concern that surfaced while tracing the implementation under **Worth double-checking**. Keep style, naming, refactoring, generic best-practice, and speculative-bug judgments out of the report unless they are necessary to explain the behavior.

## Process

### 1. Establish intent

Inspect the PR description, linked issue/specification, and implementation. Record:

- the problem the PR solves
- the behavior before the change
- the behavior after the change

Done when those three points are clear and checked against the available issue/specification and code, not only the author's summary.

### 2. Reconstruct the flow

Trace the relevant code and identify the main execution or data flow. Use the structure that exists in the repository, such as:

`UI → hook → API → service → repository → database`

`request → controller → use case → domain → persistence`

`simulation → action → service → repository`

Use the structure that exists in the repository.

Done when the primary flow and its important decisions or effects can be named without relying on the diff alone.

### 3. Sort signal from support

Classify changed files or sections as:

- behavioral changes
- architectural changes
- wiring
- types
- generated files
- fixtures
- formatting
- migrations
- test support

Use the classification to decide where close explanation is needed and where a concise summary is enough.

Done when every behavioral or architectural change is represented and supporting changes are either tied to the mental model or identified as mechanical.

### 4. Build the reading path

Choose the minimum set of files, functions, or changed sections the human should inspect. Order them by the best reading sequence, minimizing mental jumps rather than sorting alphabetically or by file size.

A common sequence is:

`entry point → orchestration/business logic → downstream dependency → persistence/external effect → tests`

Adapt the sequence when a domain model, schema, or other contract provides the necessary starting context.

For every reading target, provide:

- the repository-relative file path
- the relevant function or class when useful
- a short role label
- why to read it
- what specifically to notice

Use labels such as `START HERE`, `CORE LOGIC`, `BOUNDARY`, `PERSISTENCE`, `SIDE EFFECT`, and `VERIFY`. When several files are repetitive consumers, choose one representative, explain the shared pattern, and identify which file to read first. Group files only when they serve one cohesive conceptual role.

Example:

**1. `vote-action.ts` — START HERE**

This is where the changed vote flow begins. Focus on what happens when a vote already exists.

**2. `vote-service.ts` — CORE LOGIC**

This contains the transition rules. Understand how upvote, downvote, and no-vote states change.

Done when the path takes the human from the change's context to its observable effect using the fewest targets that still make the explanation accurate.

### 5. Link verified targets

Make recommended reading targets clickable when the repository and PR head commit are known. For a GitHub repository, use a permalink in this form:

`https://github.com/<owner>/<repo>/blob/<head-sha>/<repository-relative-path>`

The path starts at the repository root, and the commit SHA appears once immediately after `/blob/`. When the repository-relative path or head SHA cannot be verified, use the plain path instead of an unverified link.

Done when every clickable target resolves to the PR head commit and every uncertain target is presented as a plain path.

### 6. Explain the tests

Select the few tests that best describe the intended behavior. Name the test file or case and explain in plain English what it proves.

Done when the selected tests and the behavior they prove are described.

### 7. Produce the mental model

Reduce the whole PR into a few facts the human should remember. Keep the report concise unless the PR genuinely requires more detail.

Done when the report follows the output contract below and includes 3–5 accurate facts in `Keep these in your head`.

## Output format

### PR in one minute

Explain why the PR exists, the before → after behavior, and the high-level implementation in a few short paragraphs.

### Main flow

Show the primary execution or data flow. Briefly explain each step when needed.

### What actually changed

Summarize the meaningful behavioral and architectural changes. Identify supporting or mechanical changes without giving them equal attention.

### Recommended reading order

Provide the Pareto reading path with contextual labels. Explain what the human should understand and notice at each target. Describe remaining supporting or mechanical files with a concise summary rather than line-by-line detail.

### Tests that matter

Explain the few tests that best prove the intended behavior.

### Worth double-checking

Include this optional section only for a concrete concern encountered while tracing the implementation. Keep it brief and limited to that concern.

### Keep these in your head

End with 3–5 concise facts the human should remember about the PR.

## Follow-up behavior

After presenting the map, answer follow-up questions directly within the same mental model. Preserve the same simple technical English.
