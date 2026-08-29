# understand-pr

A small agent skill for the final human comprehension pass on a pull request.

It turns a potentially large PR into a concise mental model:

- what changed
- before → after behavior
- the main execution flow
- the best order to read the important code
- which files are mostly mechanical
- which tests explain the behavior
- the few things you should remember before merging

Code review remains a separate responsibility.

## Install

```bash
npx skills@latest add KennethLloyd/understand-pr-skill
```

Or install only this skill:

```bash
npx skills@latest add KennethLloyd/understand-pr-skill --skill understand-pr
```

To install globally:

```bash
npx skills@latest add KennethLloyd/understand-pr-skill --skill understand-pr -g
```

## Use

Start from a fresh agent context for the initial request:

```text
/understand-pr https://github.com/owner/repo/pull/123
```

Then ask focused follow-up questions:

```text
Explain the second file more deeply.
```

```text
Trace what happens after this service method runs.
```

```text
Why did this repository need to change?
```

## Philosophy

Agentic development makes code generation cheap. Understanding what is about to enter the codebase is the human bottleneck.

`understand-pr` compresses the review surface into a mental model while keeping final ownership with the human.
