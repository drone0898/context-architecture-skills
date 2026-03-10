# Context Design Principles

Use this file when deciding **what belongs in the root** and **what should move into nested context files**.

## The governing idea

**AI coding is usually a context architecture problem before it is a prompting problem.**

A giant instruction file causes:
- noisy context
- conflicting rules
- lower adherence
- wasted tokens
- fuzzy ownership

A good context system works like software architecture:
- broad rules at the top
- sharper rules closer to the code
- local instructions override or narrow broader guidance
- only the relevant context comes into play

## Prime rule

> Put every instruction at the narrowest boundary that actually needs it.

When in doubt:
- do **not** add more to the root
- move the rule down

## What belongs in the root

Root files are for repository-wide invariants such as:
- how to build, test, lint, or validate the repo at a high level
- architecture rules that truly apply everywhere
- cross-cutting security or compliance constraints
- naming or workflow rules that every contributor should follow
- a directory map telling agents where to look for specialized local rules

Root files should not become encyclopedias.

## What belongs in nested files

Nested files are for local deltas such as:
- subtree-specific commands
- stack-specific rules (for example React vs FastAPI vs Terraform)
- local architecture constraints
- folder ownership boundaries
- testing expectations unique to one subsystem
- sensitive operational rules that apply only in one area

A nested file should exist only when it adds real local value.

## What to avoid

- One huge file that mixes frontend, backend, infra, data, and deployment rules
- Copying the same rule into every file
- Vague taste statements that cannot be verified
- Rewriting good existing files just to make them look uniform
- Creating parallel files for tools nobody on the team uses

## Karpathy lens for context authoring

### Think Before Coding
Do not split files mechanically.
First identify:
- repo-wide invariants
- local subsystem rules
- existing conflicts
- which files the team actually uses

### Simplicity First
The best structure is not the most elaborate one.
It is the smallest file graph that creates clear boundaries.

### Surgical Changes
If a giant root file exists already:
- keep the truly global parts
- extract only the clearly local rules
- avoid gratuitous rewriting of wording that already works

### Goal-Driven Execution
The goal is not “nicer docs.”
The goal is:
- smaller active context
- fewer contradictions
- better tool-specific loading behavior
- clearer ownership
- more consistent agent behavior

## Default decision rules

If a rule:
- applies to everyone → root
- applies to one subtree → nested
- applies to one task only → probably not a context file; consider a skill or task doc instead
- is obvious from the code and rarely important → usually omit it
- is repeated in many places → centralize or move to the narrowest shared ancestor

## Good end state

A good result usually looks like this:
- lean root instruction files
- only a few nested files at genuine boundaries
- each file starts with a scope statement
- each file mostly contains local facts, commands, and checks
- any agent can tell *why* a file exists just by reading its title and first section
