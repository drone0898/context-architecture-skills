# Output Contract

When you use this skill, do the work in this order:

## 1. Boundary map
Summarize the context boundaries you discovered, for example:
- repo-wide
- frontend app
- backend service
- infra
- data / migrations
- security-sensitive subtree

## 2. File tree
Show the instruction-file tree you plan to create or update.

Keep it small. Every file should have a reason to exist.

## 3. Write or update the files
The main deliverable is the files themselves.

For each file:
- start with a scope section
- keep only rules relevant to that scope
- prefer commands, paths, and checks
- avoid generic prose

## 4. Final report
Include:
- created / updated files
- one or two lines on why each file exists
- verification notes
- any tool-specific deltas between same-path sibling files, with justification
- unresolved assumptions or TODOs

## 5. Quality bar
Before finishing, confirm:
- the root stayed lean
- nested files contain local deltas
- no commands were invented
- no parent/child conflicts remain
- no unnecessary tool families were added
- no single file exceeds ~200 lines or ~6 KB (compress or split if it does)
- content language matches the user's language preference (if specified)
- same-path sibling files are identical unless a justified tool-specific delta is reported
