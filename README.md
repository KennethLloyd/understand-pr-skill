# understand-pr

A small agent skill for the final human comprehension pass before merging a pull request.

It does **not** replace code review.

Instead, it turns a potentially large PR into a concise mental model:

- what changed
- before → after behavior
- the main execution flow
- the best order to read the important code
- which files are mostly mechanical
- which tests explain the behavior
- the few things you should remember before merging

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

## Usage

Start from a fresh agent context when possible:

```text
/understand-pr https://github.com/owner/repo/pull/123
```

Then drill down naturally:

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

Agentic development makes generating code cheap.

The human bottleneck becomes understanding what is about to enter the codebase.

`understand-pr` uses AI to compress the review surface without outsourcing final ownership of the code.