# CLAUDE.md

Guidance for AI assistants (and humans) working in this repository.

## Current state of the repository

This repository is currently a **bare scaffold**. As of this writing it contains:

- `README.md` — a single-line placeholder (`# Claude`)
- `CLAUDE.md` — this file

There is **no application code, build tooling, dependency manifest, test suite, or
CI configuration yet**. Do not assume any framework, language, or directory layout
is in place — none has been chosen. When you add the first real code, update this
file in the same change so it always reflects reality.

> Keep this document honest. It should describe what the repo *actually contains*,
> not aspirational structure. If a section below no longer matches the code, fix it.

## Repository structure

```
.
├── README.md     # Project placeholder
└── CLAUDE.md     # This file
```

Update the tree above whenever the layout changes.

## Development workflow

### Branching

- The default/integration branch is `main`.
- Do all work on a feature branch; never commit directly to `main`.
- Branch names in use follow a `claude/<short-description>-<suffix>` pattern
  (e.g. `claude/claude-md-docs-anpgrk`). Match the existing convention.

### Committing

- Write clear, descriptive commit messages in the imperative mood
  (e.g. "Add CLAUDE.md", not "added stuff").
- Keep commits focused; group related changes together.

### Pushing

- Push with `git push -u origin <branch-name>`.
- Open a pull request only when explicitly requested.

## Conventions for AI assistants

1. **Reflect reality.** This repo has almost nothing in it. Don't fabricate
   structure, commands, or conventions that don't exist. If asked to document
   something that isn't here, say so.
2. **Bootstrap deliberately.** When introducing the first real code, also add the
   matching tooling (dependency manifest, formatter/linter config, test runner)
   and document the install / build / test / lint commands in this file.
3. **Update this file with each meaningful change** so it stays a trustworthy map
   of the codebase.
4. **Verify before claiming.** Run the relevant build/test/lint commands (once they
   exist) before reporting that a change works.

## Build / test / lint commands

_None yet — no tooling has been set up. Populate this section when it is._
