# Monorepo Example

This example shows the *shape* to aim for, not a file tree to copy blindly.

## Bad pattern

A single root instruction file contains:
- React / Next.js rules
- Python / FastAPI rules
- Terraform rules
- migration rules
- deployment steps
- data warehouse notes
- package-specific test commands
- team ownership notes

This creates noise and conflicting instructions.

## Better pattern

```text
repo/
├── CLAUDE.md
├── AGENTS.md
├── GEMINI.md
├── apps/
│   ├── web/
│   │   ├── CLAUDE.md
│   │   ├── AGENTS.md
│   │   └── GEMINI.md
│   └── api/
│       ├── CLAUDE.md
│       ├── AGENTS.md
│       └── GEMINI.md
├── infra/
│   ├── CLAUDE.md
│   ├── AGENTS.md
│   └── GEMINI.md
└── packages/
    └── ui/
```

If all three families are generated for the same boundary, the matching files should start from identical content.
Only keep a tool-specific delta when the tool's behavior genuinely requires it.

## What belongs at the root

- repo-wide build/test/lint map
- shared safety constraints
- global architecture invariants
- brief directory map pointing to local files

## What belongs in `apps/web/`

- Next.js / React conventions
- component, route, and UI testing rules
- local frontend commands
- browser-only or design-system constraints

## What belongs in `apps/api/`

- API contract rules
- migration or schema expectations
- backend-specific tests
- local service boundaries

## What belongs in `infra/`

- Terraform / IaC safety rules
- plan/apply validation
- environment and secret-handling constraints

## What does *not* automatically need a nested file

`packages/ui/` should get a nested file only if it has local rules that differ from the root.
If it just inherits repo-wide rules, do not add one.

## Tool-minimal variant

If the team only uses one family, create only that family:
- Claude-only → just `CLAUDE.md`
- Codex-only → just `AGENTS.md`
- Gemini-only → just `GEMINI.md`

Only generate all three families when the user explicitly wants multi-agent compatibility or the repo clearly uses multiple tools.
When you do, keep matching files at the same path identical by default.
