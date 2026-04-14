# Contributing to Deep Thinking Skill

Thank you for wanting to make AI agents think better. This project is community-first, and every contribution — from fixing a typo to adding a new reasoning framework — makes a real difference.

---

## 🧭 Contribution Types

| Type | Description | Difficulty |
|---|---|---|
| 📝 **New Example** | Add a real-world deep thinking walkthrough | Easy |
| 🔧 **SKILL.md Improvement** | Refine the reasoning protocol | Medium |
| 🧩 **New Framework** | Add a reasoning framework (e.g., Socratic Method) | Medium |
| 🐛 **Bug Report** | Found a logical gap in an example? Report it | Easy |
| 📖 **Documentation** | Improve README, Contributing, or code comments | Easy |
| 🧪 **Benchmark** | Create a test case to measure reasoning quality | Hard |

---

## 🚀 Quick Start

### 1. Fork & Clone

```bash
git clone https://github.com/<your-username>/deep-thinking-skill.git
cd deep-thinking-skill
```

### 2. Create a Branch

```bash
git checkout -b feat/your-feature-name
```

### 3. Make Your Changes

### 4. Submit a PR

```bash
git add .
git commit -m "feat: add <description>"
git push origin feat/your-feature-name
```

Then open a Pull Request from your fork to `shade-solutions/deep-thinking-skill:main`.

---

## 📝 Adding a New Example

This is the **easiest** and **most impactful** way to contribute.

### Template

Create a new file in `examples/` with this structure:

```markdown
# Example: [Title] — [Subtitle]

> **Domain**: [e.g., Backend Engineering · Distributed Systems]
> **Difficulty**: [Easy / Moderate / Hard]
> **Frameworks Used**: [e.g., 5 Whys, First Principles]

## 💬 User Prompt
> "The actual question the user would ask."

---

## Phase 1: Problem Intelligence 🔍
### 1.1 Restatement
### 1.2 Objective
### 1.3 Constraint Space (table)
### 1.4 Hidden Assumptions (with 🟢🟡🔴 risk levels)

## Phase 2: Decomposition Engine 🔧
## Phase 3: Systematic Reasoning 🧮
## Phase 4: Multi-Path Analysis 🔀 (trade-off table)
## Phase 5: Failure Mode Detection 🛡️
## Phase 6: Confidence Scoring 📊 (table)
## Phase 7: Final Output 📋
### 📌 Executive Summary
### 🎯 The Solution
### 📋 Implementation Steps
### ⚠️ Caveats & Risks
### 📊 Confidence: **XX/100**
```

### Quality Checklist

- [ ] All 7 phases are present (Phase 0 can be omitted if trivial)
- [ ] Hidden assumptions use the 🟢🟡🔴 risk notation
- [ ] At least 2 alternative paths in Phase 4
- [ ] Confidence score is honest (not inflated)
- [ ] Code examples (if any) are runnable
- [ ] The example teaches something — it's not just a template fill

---

## 🔧 Improving SKILL.md

The `SKILL.md` file is the **core intelligence** of this project. Changes here affect every AI agent that uses the skill.

### Guidelines for SKILL.md Changes

1. **Test before submitting**: Install the skill locally and verify the agent follows your changes
2. **Backwards compatible**: Don't remove existing phases — extend them
3. **Evidence-based**: If you're adding a new behavioral constraint, explain *why* it improves reasoning quality
4. **Concise**: The skill should stay under ~400 lines. Move supplementary content to `references/`

### What We're Looking For

- Prompting techniques that **measurably reduce hallucinations**
- New behavioral constraints that prevent common failure modes
- Better output format templates
- Improved trigger descriptions for better skill activation

---

## 🧩 Adding a New Reasoning Framework

We maintain a frameworks library inside `SKILL.md`. To add a new one:

1. **Name it**: What's the framework called? (e.g., "Socratic Method")
2. **Describe when to use it**: What problem types does it apply to?
3. **Provide an example**: Create a matching `examples/` file demonstrating it
4. **Add it to the table** in SKILL.md under "Reasoning Frameworks Library"
5. **Update metadata.json**: Add the framework name to the `frameworks` array

---

## 📜 Standards

### Commit Messages
We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add Socratic Method framework
fix: correct Bayesian formula in problem-solving example
docs: improve installation instructions
refactor: restructure Phase 3 for clarity
```

### Markdown Style
- Use ATX-style headers (`#`, `##`, `###`)
- Use tables for structured comparisons
- Use emoji sparingly and consistently (as section markers, not decoration)
- Keep lines under 120 characters where possible
- Use fenced code blocks with language identifiers

### JSON Style
- 2-space indentation
- No trailing commas
- Validate with `python3 -m json.tool` before committing

---

## 🤝 Code of Conduct

We follow the [Contributor Covenant](CODE_OF_CONDUCT.md). Be respectful, constructive, and inclusive.

---

## 💬 Questions?

- **Bug or feature request?** → [Open an Issue](https://github.com/shade-solutions/deep-thinking-skill/issues/new)
- **Big idea or RFC?** → [Start a Discussion](https://github.com/shade-solutions/deep-thinking-skill/discussions)
- **Quick question?** → Tag `@shaswatraj` in an issue

---

<div align="center">

**Every contribution makes AI agents think a little deeper.** 🧠

</div>
