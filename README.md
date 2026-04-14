<div align="center">

# 🧠 Deep Thinking Skill

### An Operating System for Reasoning

*Stop hallucinating. Start thinking.*

<br>

![Visitor Counter](https://api.visitorbadge.io/api/VisitorHit?user=shade-solutions&repo=deep-thinking-skill&countColor=%237B1FA2)

[![GitHub Stars](https://img.shields.io/github/stars/shade-solutions/deep-thinking-skill?style=for-the-badge&logo=github&color=ffd700)](https://github.com/shade-solutions/deep-thinking-skill/stargazers)
[![GitHub Forks](https://img.shields.io/github/forks/shade-solutions/deep-thinking-skill?style=for-the-badge&logo=github&color=007bff)](https://github.com/shade-solutions/deep-thinking-skill/network/members)
[![GitHub Issues](https://img.shields.io/github/issues/shade-solutions/deep-thinking-skill?style=for-the-badge&logo=github&color=ff4444)](https://github.com/shade-solutions/deep-thinking-skill/issues)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=for-the-badge)](https://github.com/shade-solutions/deep-thinking-skill/pulls)

[![MIT License](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)](https://github.com/shade-solutions/deep-thinking-skill/blob/main/LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude_Code-Compatible-blue.svg?style=for-the-badge&logo=anthropic)](https://github.com/anthropic/claude-code)
[![Version](https://img.shields.io/badge/Version-2.0.0-purple.svg?style=for-the-badge)]()
[![Skill Type](https://img.shields.io/badge/Type-Reasoning_Framework-orange.svg?style=for-the-badge)]()

[![Twitter Follow](https://img.shields.io/twitter/follow/shaswatraj?style=social)](https://twitter.com/shaswatraj)

</div>

---

## ❗ The Problem

AI agents are **fast, but not careful**. They race to an answer without checking their own logic. The result?

| Failure Mode | What Happens |
|---|---|
| **Hallucination** | Confidently states incorrect facts as truth |
| **Surface-level answers** | Fixes the symptom, not the root cause |
| **Missing edge cases** | Works in the demo, breaks in production |
| **No self-doubt** | Never says "I'm not sure" even when it should |
| **Single-path thinking** | Presents one option without exploring alternatives |

**Deep Thinking Skill** solves this by embedding a **7-phase structured reasoning protocol** directly into the agent's behavior.

---

## ⚡ What Makes This Different

This is **not a prompt.** This is an **operating system for thinking.**

| Feature | Description |
|---|---|
| 🔍 **Problem Intelligence** | Forces the agent to restate the problem, map constraints, and surface hidden assumptions before solving |
| 🔧 **Atomic Decomposition** | Breaks complex problems into independently solvable, testable sub-problems |
| 🧮 **Adversarial Verification** | Built-in "steel-man the opposite view" step to catch logical fallacies |
| 🔀 **Multi-Path Analysis** | Always generates 2+ alternatives with a trade-off matrix |
| 🛡️ **Failure Mode Detection** | Pre-mortem analysis, blast radius assessment, and mitigation plans |
| 📊 **Confidence Scoring** | Self-assessed 0-100 score with hard rules (below 50% = refuse to answer confidently) |
| 🧩 **12 Reasoning Frameworks** | Built-in library: 5 Whys, FMEA, Porter's Five Forces, First Principles, RICE, and more |
| 🔄 **Meta-Cognition** | The agent checks whether it actually followed the protocol at the end |

---

## 📦 Installation

### Claude Code (recommended)

```bash
# Install as a project skill
git clone https://github.com/shade-solutions/deep-thinking-skill.git .claude/skills/deep-thinking

# Or install as a personal skill (available across all projects)
git clone https://github.com/shade-solutions/deep-thinking-skill.git ~/.claude/skills/deep-thinking
```

### npx Skills (community tool)

```bash
npx skills install shade-solutions/deep-thinking-skill
```

### Manual

```bash
# Clone and link locally
git clone https://github.com/shade-solutions/deep-thinking-skill.git
cd deep-thinking-skill
npx skills link .
```

---

## 🔍 Usage

Once installed, the skill activates automatically when you use trigger words:

```
analyze • solve • optimize • strategy • why • debug • design
trade-off • evaluate • investigate • plan • root cause
```

### Example Prompts

```
"Analyze the scalability bottleneck in our microservices architecture."

"Why does our payment worker throw duplicate key errors 1 in 1000 times?"

"Design a real-time chat system that handles 10M concurrent users."

"Should we pivot from CRM to Lead Gen? We have 3 months of runway."

"Optimize our dashboard API — it takes 12 seconds to respond."
```

### What You Get Back

Instead of a quick (potentially wrong) answer, the agent will:

1. ✅ **Restate** the problem to confirm understanding
2. ✅ **Decompose** it into atomic sub-problems
3. ✅ **Reason** through each step with hypothesis → simulate → verify
4. ✅ **Compare** multiple solution paths with a trade-off matrix
5. ✅ **Detect** failure modes before they happen
6. ✅ **Score** its own confidence (and refuse to answer if too low)
7. ✅ **Present** a structured, actionable final output

---

## 🧠 The 7 Phases

```
Phase 0  ⚡  ACTIVATION CHECK ─── Is this problem worth deep thinking?
Phase 1  🔍  PROBLEM INTELLIGENCE ─── Restate, define objective, map constraints
Phase 2  🔧  DECOMPOSITION ─── Break into atomic sub-problems
Phase 3  🧮  SYSTEMATIC REASONING ─── Hypothesize → Simulate → Verify → Refine
Phase 4  🔀  MULTI-PATH ANALYSIS ─── Generate alternatives, trade-off matrix
Phase 5  🛡️  FAILURE MODE DETECTION ─── Pre-mortem, blast radius, mitigations
Phase 6  📊  CONFIDENCE SCORING ─── Self-assess 0-100, hard thresholds
Phase 7  📋  STRUCTURED OUTPUT ─── Executive summary, steps, caveats
```

---

## 🧩 Built-in Reasoning Frameworks

| Problem Type | Framework | When to Use |
|---|---|---|
| Root Cause | **5 Whys + Fishbone** | Debugging, incident response |
| Architecture | **C4 Model + ATAM** | System design, tech debt |
| Decisions | **Eisenhower Matrix + RICE** | Prioritization, resource allocation |
| Optimization | **Theory of Constraints** | Bottleneck identification |
| Strategy | **Porter's Five Forces + SWOT** | Business/competitive analysis |
| Risk | **FMEA** | Safety-critical systems |
| Innovation | **First Principles** | Novel problem spaces |
| Trade-offs | **Pareto Analysis (80/20)** | Cost-benefit, feature scoping |

---

## 📂 Examples

Real-world demonstrations of the Deep Thinking protocol in action:

| Example | Domain | Difficulty | Key Technique |
|---|---|---|---|
| [🔧 Debugging Race Conditions](examples/debugging.md) | Distributed Systems | Hard | TOCTOU analysis, timing simulation |
| [📊 Strategic Pivot Decision](examples/decision-making.md) | Startup Strategy | Hard | Porter's Five Forces, RICE scoring |
| [🎲 Monty Hall Probability](examples/problem-solving.md) | Mathematics | Moderate | Bayesian proof, Monte Carlo simulation |
| [🏗️ Real-Time Chat at Scale](examples/system-design.md) | System Architecture | Hard | C4 Model, FMEA, back-of-envelope math |
| [⚡ 12-Second API Fix](examples/optimization.md) | Performance Engineering | Hard | Theory of Constraints, Pareto analysis |

---

## 📂 Project Structure

```text
deep-thinking-skill/
├── SKILL.md              # 🧠 The brain — 7-phase reasoning protocol
├── metadata.json          # ⚙️ Machine-readable skill metadata
├── README.md              # 📖 You are here
├── CONTRIBUTING.md        # 🤝 Community contribution guide
├── CODE_OF_CONDUCT.md     # 📜 Community standards
├── LICENSE                # ⚖️ MIT License
└── examples/
    ├── debugging.md       # 🔧 Race condition root cause analysis
    ├── decision-making.md # 📊 Startup pivot strategy
    ├── problem-solving.md # 🎲 Monty Hall Bayesian proof
    ├── system-design.md   # 🏗️ 10M-user chat architecture
    └── optimization.md    # ⚡ 12-second API performance fix
```

---

## 🤝 Contributing

This is a **community-first** project. We welcome:

- 🧩 New reasoning frameworks (Socratic Method, Inversion, etc.)
- 📝 New real-world examples
- 🔧 Improvements to the SKILL.md protocol
- 🐛 Bug reports for logical gaps in examples

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

---

## 🌟 Vision

We believe the next generation of AI agents won't just be faster — they'll be **disciplined thinkers**.

Today's agents are like brilliant interns: fast, eager, but they skip steps and don't check their own work. Deep Thinking Skill is the **senior engineer mindset** — embedded directly into the agent's reasoning loop.

**The goal**: Every AI agent in the world should *think before it speaks*.

---

## 🗺️ Roadmap

- [x] 7-phase reasoning protocol (v2.0)
- [x] Confidence scoring system
- [x] Failure mode detection
- [x] 12 built-in reasoning frameworks
- [x] 5 real-world examples
- [ ] Integration with memory/retrieval skills
- [ ] Chain-of-thought visualization tool
- [ ] Community-contributed framework packs
- [ ] Automated reasoning quality benchmarks

---

## ⭐ Star History

If this project makes your AI agents smarter, consider giving it a ⭐

<div align="center">

[![Star History Chart](https://api.star-history.com/svg?repos=shade-solutions/deep-thinking-skill&type=Date)](https://star-history.com/#shade-solutions/deep-thinking-skill&Date)

</div>

---

<div align="center">

**Built with 🧠 by [shade-solutions](https://github.com/shade-solutions)**

*Think deeper. Build better.*

</div>
