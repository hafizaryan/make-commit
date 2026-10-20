# make-commit 🚀

> An AI agent skill that inspects Git diffs and repository history to generate clean, contextual, and convention-aware commit messages.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Skill: AI Agent](https://img.shields.io/badge/Skill-AI%20Agent-blue.svg)](SKILL.md)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/)

---

## ✨ Features

- 🔍 **Diff & History Inspection**: Analyzes both staged (`git diff --cached`) and unstaged changes while learning from recent repository commits.
- 📐 **Format Auto-Detection**:
  - **Conventional Commits** (`feat(auth): ...`, `fix: ...`)
  - **Issue / Ticket Prefixes** (`[PROJ-123] ...`, `#42: ...`)
  - **Gitmoji** (`✨ feat: ...`, `🐛 fix: ...`)
  - **Imperative Standard** (`Add ...`, `Fix ...`)
- 🛡️ **Secret & Security Guardrails**: Detects and refuses to expose sensitive files, `.env` variables, API keys, or credentials in commit messages.
- ✂️ **Atomic Commit Advice**: Identifies mixed concerns and recommends splitting changes into distinct, logical commits.
- ⚡ **Copy-Ready Output**: Provides commit messages in plain text blocks with zero manual cleanup required.

---

## 📂 Repository Structure

```
make-commit-skill/
├── .gitignore
├── LICENSE
├── README.md
└── SKILL.md          # Core skill definition & workflow
```

---

## 📦 Installation

This skill adheres to standard agent skill specifications and works across modern AI coding assistants.

### 1. Antigravity & Gemini CLI
Copy the skill folder or `SKILL.md` into your Antigravity skills directory:

**Global:**
```sh
# Linux / macOS
mkdir -p ~/.gemini/antigravity-cli/skills/make-commit
cp SKILL.md ~/.gemini/antigravity-cli/skills/make-commit/

# Windows (PowerShell)
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.gemini\antigravity-cli\skills\make-commit"
Copy-Item "SKILL.md" -Destination "$env:USERPROFILE\.gemini\antigravity-cli\skills\make-commit\SKILL.md"
```

**Project-Specific:**
Place `SKILL.md` inside `.gemini/skills/make-commit/SKILL.md` in your project root.

---

### 2. Claude Code / Claude Desktop
Add to your Claude skills directory:

```sh
mkdir -p ~/.claude/skills/make-commit
cp SKILL.md ~/.claude/skills/make-commit/SKILL.md
```

---

### 3. Cursor / Windsurf / Cline / Roo Code
- **Cursor**: Reference or paste `SKILL.md` contents into `.cursor/rules/make-commit.mdc` or add to project instructions.
- **Cline / Roo Code**: Add to `.roomodes` or custom instructions / skills folder.

---

## 🎯 Usage

Once installed, trigger the skill using slash commands or natural language prompts:

### Slash Command
```text
/make-commit
```

### Prompt Examples
- *"Make a commit message for my staged changes."*
- *"Look at what I changed and draft a Conventional Commit message."*
- *"Help me write a commit message that follows our repo's recent history."*
- *"Split my uncommitted changes into atomic commit proposals."*

---

## 📝 Example Output

### Conventional Commits
```text
feat(auth): implement refresh token rotation

Rotate refresh tokens upon each use to protect against replay attacks.
Revoke old tokens immediately and log invalid reuse attempts.

Closes #142
```

### Standard Imperative
```text
Fix off-by-one error in pagination calculation

Page offset calculations previously resulted in an empty first page
when page size matched total record count. Correct zero-based index
handling in list_items query.
```

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
