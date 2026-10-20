---
name: make-commit
description: Inspect current Git changes and recent commit history, then draft a clear commit message matching the repository's established format. Use when the user invokes make-commit or requests a commit message.
version: 1.0.0
tags:
  - git
  - devops
  - workflow
  - productivity
---

# Make Commit

## Purpose

Inspect current Git changes and repository commit history, analyze the intent of the diff, and draft a high-quality commit message tailored to the project's established conventions.

---

## Workflow

### 1. Inspect Changes and Context

Run the following non-destructive inspection commands:

```sh
git status --short
git log -15 --format='%h %s%n%b%n---'
git diff --cached --stat
git diff --stat
```

#### Scope Rules:
- **Staged changes present (`git diff --cached`)**: Focus strictly on staged changes.
- **No staged changes**: Analyze unstaged changes (`git diff`) and untracked files. State clearly that the draft covers unstaged changes.
- **Deep inspect**: Inspect relevant file diffs with `git diff -U3` or read specific files when semantics are ambiguous.
- **Zero changes**: If no modified or staged files exist, report "No changes detected" and stop.
- **Missing repo**: If not inside a Git worktree, request repository access or the raw diff and commit history.

---

### 2. Guardrails & Secret Prevention

Before drafting, scan the diff and file paths:

- **Never reference or expose secrets**: Reject or warn on API keys, private tokens, passwords, `.env` entries, SSH keys, credentials, or certificates.
- **Never inspect or add ignored files**: Respect `.gitignore`.
- **Do not execute commits automatically**: Never run `git commit`, `git add`, `git push`, or alter Git history unless explicitly commanded by the user.

---

### 3. Identify Repository Commit Format

Prioritize documented project guidelines (e.g., `CONTRIBUTING.md`, `AGENTS.md`, `.commitlintrc*`, or `commitlint.config.js`).

If undocumented, infer the dominant pattern from recent human commits (discount automated merge commits and bump bot commits):

1. **Conventional Commits**:
   - Pattern: `<type>(<scope>): <subject>`
   - Types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`
   - Breaking changes: `<type>!: <subject>` or `BREAKING CHANGE:` in body/footer.
2. **Issue / Ticket Prefixes**:
   - Pattern: `[PROJ-1234] <subject>` or `feat: [ABC-99] <subject>` or `#123: <subject>`.
   - Never invent ticket or issue numbers. Only include if evident from branch name (`feature/PROJ-123`), staged files, or user prompt.
3. **Gitmoji**:
   - Pattern: `:emoji: <subject>` or `✨ feat: <subject>`.
   - Match existing emoji conventions if predominantly used.
4. **Standard Imperative (Default)**:
   - Pattern: `<Verb in imperative mood> <concise summary>`
   - Example: `Add user authentication middleware`

Match repository casing (lowercase vs capitalized subject), punctuation (no trailing period in subject line), and language.

---

### 4. Analyze Intent and Draft Content

1. **Subject Line**:
   - Imperative mood ("Add", "Fix", "Refactor", not "Added", "Fixing").
   - Target <= 50-72 characters.
   - Accurately summarize the core change.
   - Avoid generic phrases like "update files", "fix bugs", "misc changes".

2. **Commit Body**:
   - Separate subject and body with a blank line.
   - Focus on **WHY** the change was made, not just **WHAT** lines changed.
   - Explain context, trade-offs, before-and-after behavior, or potential side-effects.
   - Wrap body text at 72-80 characters for terminal readability.
   - For trivial changes (typo fix, small dependency bump), a subject line alone is sufficient.

3. **Multi-Concern Handling**:
   - If the diff contains unrelated changes (e.g., refactoring auth + fixing UI CSS), recommend splitting into atomic commits and provide message drafts for each.

---

### 5. Output Format

Return the recommended message in a fenced `text` code block ready to copy:

```text
<commit message here>
```

Add any scope explanations, split recommendations, or notes outside the block.

---

## Examples

### Example 1: Conventional Commit (Feature)

```text
feat(auth): add refresh token rotation and revocation endpoint

Rotate refresh tokens upon each use to protect against replay attacks.
Revoke old tokens immediately and log invalid reuse attempts.

Closes #142
```

### Example 2: Bug Fix (Standard Imperative)

```text
Fix off-by-one error in pagination calculation

Page offset calculations previously resulted in an empty first page
when page size matched total record count. Correct zero-based index
handling in list_items query.
```

### Example 3: Breaking Change

```text
feat(api)!: migrate v1 response envelope to standard JSON:API format

BREAKING CHANGE: Response data is now nested under `data.attributes`.
Clients must update deserializer models before upgrading to v2.0.
```
