# 🧠 Deep Thinking Skill

[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://github.com/shade-solutions/deep-thinking-skill/blob/main/LICENSE)
[![Claude Code Compatible](https://img.shields.io/badge/Claude--Code-Compatible-blue.svg)](https://github.com/anthropic/claude-code)

**Deep Thinking Skill** is a production-ready reasoning framework designed for AI agents. It forces agents to slow down, decompose complex problems, and validate their own logic before outputting a final answer.

## ❗ The Problem
Most AI agents jump to conclusions too quickly, leading to:
- **Hallucinations**: confidently stating false info.
- **Surface-level solutions**: missing the root cause of a bug.
- **Logic gaps**: ignoring edge cases in complex probability or math.

## ⚡ Features
- **Structured Logic**: Enforces a 6-step reasoning playbook.
- **Atomic Decomposition**: Breaks monolith problems into tiny, solvable parts.
- **Self-Correction**: Built-in "Pre-Mortem" and validation steps.
- **Compatibility**: Works out-of-the-box with `Claude Code`, `npx skills`, and custom agent frameworks.

## 📦 Installation

Install directly into your agent environment:

```bash
# Using npx skills (community tool)
npx skills install shade-solutions/deep-thinking-skill

# Local linking for development
git clone https://github.com/shade-solutions/deep-thinking-skill.git
cd deep-thinking-skill
npx skills link .
```

## 🔍 Usage

Once installed, simply trigger the skill by using keywords like `analyze`, `solve`, `think deep`, or `strategy`.

**Example:**
> "Analyze the scalability bottleneck in our current microservices architecture using deep thinking."

## 🧠 Examples
Explore how this skill handles real-world complexity:
- [Debugging Race Conditions](examples/debugging.md)
- [Strategic Decision Making (Pivoting)](examples/decision-making.md)
- [Complex Logic & Probability](examples/problem-solving.md)

## 📂 Project Structure
```text
deep-thinking-skill/
├── SKILL.md          # Core intelligence & reasoning playbook
├── metadata.json      # Machine-readable skill metadata
├── README.md          # You are here
├── CONTRIBUTING.md    # Community guidelines
├── LICENSE            # MIT License
└── examples/          # Real-world use cases
```

## 🤝 Contributing
This is a **community-first** project. We welcome:
- New reasoning frameworks (e.g., First Principles, Inversion).
- More robust examples.
- Enhancements to the `SKILL.md` prompts.

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## 🌟 Vision
We believe the next generation of AI agents won't just be faster—they'll be *smarter*. By embedding structured reasoning directly into the agent's DNA, we move from "chatbots" to "autonomous problem solvers."

Built with ❤️ by [shade-solutions](https://github.com/shade-solutions).
