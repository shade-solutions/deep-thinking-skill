# Contributing to Deep Thinking Skill

Thank you for your interest in making AI agents think better! This project is community-first and open to all kinds of improvements.

## 🚀 How to Contribute

### 1. Adding New Examples
If you have a great example of how structured reasoning solved a problem:
- Create a new markdown file in `examples/`.
- Follow the format defined in `examples/problem-solving.md`.
- Ensure it demonstrates **Discovery, Decomposition, Reasoning, and Validation**.

### 2. Improving the SKILL.md
The `SKILL.md` file is the brain of this project. If you have advice on:
- Reducing hallucination rates.
- Better prompting techniques for step-by-step logic.
- New behavioral constraints.

Please open an Issue or a PR with your proposed changes.

### 3. Adding New Frameworks
We want to support multiple mental models! If you'd like to add a "First Principles" or "Socratic Method" framework:
- Suggest a way to integrate it into `SKILL.md` (e.g., using specific triggers).
- Provide an example of it in action.

## 🛠️ Development Setup

```bash
# Clone the repo
git clone https://github.com/shade-solutions/deep-thinking-skill.git

# Link it locally to test with your agent
npx skills link .
```

## 📜 Coding Standards
- **Simplicity**: Keep markdown clean and readable.
- **Modularity**: If a reasoning framework gets too long, consider moving parts to a `references/` directory.
- **JSON Validity**: Always ensure `metadata.json` is valid JSON.

## 🤝 Discussions
Have a big idea? Open a **Discussion** on GitHub before starting work so we can align on the vision.

---

Let's build a smarter future together! 🧠
