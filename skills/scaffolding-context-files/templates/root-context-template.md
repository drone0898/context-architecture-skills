# <PROJECT> Context

## Scope
Applies to the entire repository. Keep only repo-wide rules here.

## What this repository is
- Brief description of the product or system
- Primary stacks or major areas
- High-level ownership map if useful

## Default commands
- Install:
- Dev:
- Test:
- Lint:
- Build:
- Typecheck:
- Other critical validation:

Only include commands you can verify from the repo.

## Global architecture rules
- Cross-cutting constraints that truly apply everywhere
- Shared quality bar
- Shared safety / security rules
- Important repo-wide paths or boundaries

## How to work here
- Follow existing patterns before introducing abstractions
- Keep changes surgical
- Add local rules to nested context files instead of expanding this root file
- Prefer concrete verification over vague “looks good” checks

## Subtree map
Point to the places where more specific context lives or should live.
Examples:
- `apps/web/` → frontend-specific rules live in nested context files
- `apps/api/` → backend-specific rules live in nested context files
- `infra/` → infrastructure and deployment rules live in nested context files

## Verification
State the default validation expectations for repo-wide work.
