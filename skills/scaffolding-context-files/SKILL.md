---
name: scaffolding-context-files
description: Designs and generates layered CLAUDE.md, AGENTS.md, GEMINI.md, and nested instruction files for repositories. Use when bootstrapping AI coding context, splitting bloated root files, or adding per-package guidance in a monorepo.
argument-hint: "[stack notes, team boundaries, or requested file formats]"
---

# Scaffolding Context Files

Design context architecture first. Then generate the smallest useful set of AI instruction files for the repository.

Use the user's extra notes if provided: `$ARGUMENTS`

## Language preference

If the user specifies a language (e.g., "한국어", "English", "日本語") in their arguments or conversation, **write all generated context files in that language**. This includes section headings, descriptions, comments, and inline guidance. Commands, paths, and tool-specific keywords remain in their original form.

If no language is specified, fall back to the repository's dominant documentation language (see Authoring rules).

## Prime directive

**Every instruction belongs at the narrowest boundary that actually needs it.**

Do not solve context problems by making a bigger root file.

## Non-negotiables

- Prefer **context architecture** over context dumping.
- Root files hold only repository-wide invariants.
- Nested files hold only subtree-specific rules, commands, and architectural constraints.
- Do not create equivalent files for tools the team does not use **unless the user explicitly asks for them**.
- If the repo already has instruction files, update them **surgically** instead of rewriting everything.
- Do not invent build, test, lint, or deploy commands. Verify them from repo files, CI, scripts, or docs. If a command cannot be verified, mark it as `TODO` rather than guessing.
- Do not create nested files for directories that have no genuinely local rules.

## Karpathy guardrails

### 1. Think Before Coding
- Inspect the repository before authoring anything.
- State assumptions if discovery is incomplete.
- If multiple context layouts are plausible, prefer the simplest one that preserves boundaries.
- If the repo is large, conflicting, or monorepo-shaped, **ultrathink** before writing.

### 2. Simplicity First
- Create the minimum number of files that preserves clarity.
- Do not spray `CLAUDE.md`, `AGENTS.md`, and `GEMINI.md` into every directory by default.
- If a subtree has no unique rules, leave it covered by the root file.

### 3. Surgical Changes
- Preserve good existing context.
- Split noisy files by extracting only the rules that clearly belong to lower scopes.
- Keep file moves and rewrites directly traceable to the boundary design.

### 4. Goal-Driven Execution
Success means:
1. The file tree has clear boundaries.
2. Each file has an explicit scope.
3. The root file stays lean.
4. Nested files contain local deltas, not root duplication.
5. No conflicts remain between parent and child files.
6. Commands and paths are verifiable.

## Required workflow

### 1. Survey the repository
Inspect at least these sources if they exist:
- existing `CLAUDE.md`, `AGENTS.md`, `AGENTS.override.md`, `GEMINI.md`
- `.claude/`, `.github/instructions/`, `.github/copilot-instructions.md`, `README*`
- manifests and task runners: `package.json`, `pyproject.toml`, `Makefile`, `justfile`, CI workflows, docker files
- top-level directories that imply ownership or stack boundaries

### 2. Build a context boundary map
Identify the narrowest useful layers, typically from:
- repository-wide
- app/service/domain
- package/library
- infra / ops / data / migration areas
- especially sensitive or specialized subtrees

Default rule:
- If a rule applies to the whole repo, keep it at the root.
- If a rule applies to one subtree, move it down.
- If you are unsure whether a rule is root or nested, bias toward **nested**.

### 3. Choose which file families to generate
Use only what is needed:
- If the user explicitly requests `CLAUDE.md`, `AGENTS.md`, and `GEMINI.md`, generate all requested families.
- Otherwise, generate only the family or families clearly relevant to the repo's tool usage.
- For Codex-style nested guidance, default to `AGENTS.md`. Use `AGENTS.override.md` only when same-directory override semantics are intentionally needed.

### 4. Write or update files
Use:
- [reference/context-design-principles.md](reference/context-design-principles.md) for the philosophy and boundary rules
- [reference/tool-behavior.md](reference/tool-behavior.md) before deciding filenames, nesting, or override semantics
- [templates/root-context-template.md](templates/root-context-template.md) for repo-level files
- [templates/nested-context-template.md](templates/nested-context-template.md) for subtree files
- [examples/monorepo-example.md](examples/monorepo-example.md) as a target shape when the repo is a monorepo
- [templates/output-contract.md](templates/output-contract.md) for the final report shape

Authoring rules:
- If the user specified a language preference, use that language for all generated content (see Language preference above).
- Otherwise, match the repository's dominant documentation language. If mixed, prefer the language already used in the repo docs.
- Keep files skimmable with short sections and concrete bullets.
- Prefer commands, paths, and checks over vague advice.
- Put local rules in local files. Do not restate them in the root unless truly global.
- If you create multiple tool-specific files with near-identical meaning, keep them semantically aligned but adapt to each tool's discovery model.

### 5. Compress oversized files

After writing, check each file's size. If a file exceeds approximately **200 lines** or **6 KB**, it is too large for efficient context loading. Apply these steps:

1. **Extract subtree rules down.** Move any rules that only apply to a subdirectory into a nested file in that subdirectory.
2. **Deduplicate.** Remove repeated or near-identical guidance that already exists in a parent file.
3. **Tighten prose.** Replace explanatory paragraphs with concise bullets. Remove filler, hedging, and restated rationale.
4. **Split by concern.** If a root file covers multiple unrelated domains (e.g., frontend + infra + data), split into nested files at the appropriate boundaries.
5. **Use imports sparingly.** For tools that support `@file` imports (Claude Code, Gemini), extract a shared block into a separate file and import it rather than duplicating content.

After compression, re-validate that no content was lost and no contradictions were introduced.

### 6. Validate before finishing
Check each file against this rubric:
- clear scope sentence near the top
- only relevant rules for that scope
- no duplicated local guidance in the root
- no contradictory instructions between parent and child
- no invented commands
- file existence is justified by a real boundary

### 7. Report the result
Your final output must include:
1. a short boundary map
2. the resulting file tree
3. one or two lines explaining why each created or updated file exists
4. verification notes
5. unresolved assumptions or TODOs

The main deliverable is the files themselves, not a long essay.
