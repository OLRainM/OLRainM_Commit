---
name: olrainm-commit-style
description: Write or revise Git commit messages in OLRainM's Chinese Conventional Commits style after inspecting the actual changes. Use when the user asks for OLRainM-style commit wording or asks to commit changes in that style; do not use for general prose.
---

# OLRainM Commit Style

Produce a commit message that is specific enough to explain the diff without opening it. Base every claim on repository evidence; do not invent tests, issue numbers, causes, performance figures, or compatibility guarantees.

## Inspect the Change

- Read the staged diff when one exists. Otherwise inspect the working-tree diff and make clear that the message describes unstaged changes.
- Identify the single dominant intent, the affected subsystem, the user-visible or architectural outcome, and any tests or documentation changed with it.
- Check the repository's recent commit vocabulary before introducing a scope. Reuse the project's established subsystem name when it still matches the diff.
- Keep tests and documentation in the same commit when they directly support the implementation. Use a separate `test` or `docs` commit only when that artifact is the intent itself.

## Write the Header

Use one of these forms:

```text
type(scope): 中文动宾短句
type: 中文动宾短句
```

- Use lowercase Conventional Commit types: `fix`, `refactor`, `feat`, `docs`, `test`, `chore`, or `perf`.
- Choose `fix` for corrected behavior; `refactor` for ownership or structure changes that preserve behavior; `feat` for new capability; `docs` or `test` when those are the primary artifact; `chore` for repository/version maintenance; and `perf` only for an evidenced performance improvement.
- Add a concise scope when one subsystem clearly dominates. Omit it for cross-cutting work. Prefer an existing project term, whether English or Chinese, over inventing a broader label.
- Write the subject in Chinese by default. Preserve exact identifiers such as `SessionService`, `SteamID`, `appids`, commands, and filenames.
- Lead with a concrete action or outcome such as `修复`, `避免`, `防止`, `阻止`, `抽出`, `下沉`, `清理`, `完善`, `恢复`, `记录`, `支持`, or `优化`. Do not end the subject with punctuation.
- Avoid vague summaries such as `更新代码`, redundant forms such as `fix: 修复(模块): ...`, and English-only wording unless the repository context requires it.

## Write the Body

For any nontrivial change, add a body. A title-only message is reserved for an obvious, tightly scoped maintenance change.

1. In the first paragraph, state the implemented mechanism and concrete behavioral result. Name the relevant path, state, API, or boundary when that detail explains the change.
2. After a blank line, explain the prior failure, root cause, design reason, or tradeoff. State what remains unchanged when compatibility or ownership boundaries matter.
3. Mention tests, validation, performance impact, or migration behavior only when the diff or supplied evidence proves it.

Use short prose paragraphs by default. For a complex incident with several independent causes or consequences, use brief labeled sections and `-` bullets. Do not force artificial line wrapping; keep one idea per paragraph.

When an issue is known and actually resolved, add a blank line followed by `Closes #N`, or mention `修复 #N` when the issue is central to the explanation. Never infer an issue number.

## Deliver or Commit

- Return the finished message without extra alternatives unless the user asks to compare options.
- Do not run `git add` or `git commit` merely because the user asked for wording. Execute a commit only when explicitly requested, and do not stage unrelated changes.
- Let Git generate merge commit messages. Do not imitate merge messages as part of this personal style.

For calibration when the type, scope, or body depth is ambiguous, read [references/style-evidence.md](references/style-evidence.md).
