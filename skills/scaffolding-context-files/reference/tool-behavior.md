# Tool Behavior Reference

Use this file before choosing filenames, nesting strategy, or override patterns.

## Claude Code

Recommended context files:
- `CLAUDE.md`
- `.claude/CLAUDE.md`
- nested `CLAUDE.md` files in subdirectories

Behavior to design around:
- ancestor `CLAUDE.md` files load at session start
- nested subdirectory files load when Claude works in those subtrees
- `@path/to/file` imports are supported
- lean files work better than giant files
- `.claude/rules/` exists for more modular rule sets, but do not introduce it unless it truly helps

Good default:
- keep the root `CLAUDE.md` short and repo-wide
- use nested `CLAUDE.md` files for real local rules
- avoid eager imports that re-create one giant context blob

## OpenAI Codex / AGENTS.md

Recommended context files:
- `AGENTS.md`
- `AGENTS.override.md` when same-directory override behavior is intentionally required

Behavior to design around:
- guidance is merged from the root down to the current directory
- later files override earlier guidance because they appear later in the chain
- Codex includes at most one instruction file per directory
- `AGENTS.override.md` wins over `AGENTS.md` in the same directory
- combined instruction size is capped by configuration, so large roots are expensive

Good default:
- use `AGENTS.md` for normal root and nested guidance
- use `AGENTS.override.md` only when you explicitly need override semantics
- do not assume import syntax exists

## Gemini CLI

Recommended context files:
- `GEMINI.md`
- nested `GEMINI.md` files where local context matters

Behavior to design around:
- global, workspace, and ancestor context can all contribute
- highly specific nested files can be discovered just-in-time when a tool touches a subtree
- `@file.md` imports are supported
- the context filename can be configured to alternate names, but default is `GEMINI.md`

Good default:
- keep root context lean
- use nested files for local rules instead of front-loading everything
- use imports only when they help reuse a small shared block without rebuilding a giant root

## GitHub Copilot

Relevant facts:
- Copilot coding agent recognizes `AGENTS.md`
- it also supports `CLAUDE.md` and `GEMINI.md` alongside GitHub's own instruction formats
- nested instruction files are useful when different subtrees have different rules
- skills can live in `.github/skills/` or `.claude/skills/`

Good default:
- if the repo already standardizes on one family, do not add extra families unless asked
- if the team explicitly wants multi-agent compatibility, keep same-path sibling files identical by default

## Multi-tool generation rule

When generating for multiple tools:
- choose the same boundaries and file tree for every requested family
- write one canonical file for each boundary, then copy it to sibling families before making any edits
- keep same-path sibling files identical unless a real tool behavior requires a delta
- adapt to discovery behavior through filenames, placement, nesting, and loading strategy before changing the text itself
- if a tool-specific delta is necessary, keep it minimal and document why it exists
