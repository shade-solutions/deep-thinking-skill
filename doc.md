Below is a **complete, production-ready package** for your **Deep Thinking Agent Skill** — including:

* 📄 PRD (Product Requirements Document)
* 📘 README (GitHub-ready)
* 📂 Skill structure (Claude Code compatible)
* ⚙️ `npx skills` usage & commands
* 🧠 Actual `SKILL.md` implementation

Everything is aligned with how **Claude Code Agent Skills actually work**.

---

# 📄 1. PRD — Deep Thinking Skill

## 🧠 Product Name

**Deep Thinking Skill**

## 🎯 Objective

Enable AI agents to:

* Think step-by-step
* Validate reasoning
* Avoid hallucinations
* Produce higher-quality outputs

---

## ❗ Problem

Current agents:

* Jump to conclusions
* Skip reasoning steps
* Produce confident but wrong answers

---

## 💡 Solution

A reusable **Agent Skill** that enforces:

1. Problem decomposition
2. Step-by-step reasoning
3. Assumption validation
4. Output verification

---

## 👤 Target Users

* Developers using Claude Code
* AI agent builders
* Startup founders (decision-making)
* Students (learning + reasoning)

---

## ⚙️ Functional Requirements

### 1. Trigger Conditions

Skill auto-activates when:

* Complex problem detected
* Keywords:

  * "analyze"
  * "solve"
  * "strategy"
  * "optimize"
  * "why"

---

### 2. Behavior Flow

```
1. Understand problem
2. Break into subproblems
3. Solve each step
4. Validate each step
5. Combine results
6. Final verification
```

---

### 3. Output Format

```
## Understanding
## Breakdown
## Step-by-step reasoning
## Validation
## Final Answer
```

---

## 📊 Success Metrics

* Reduced hallucination rate
* Improved correctness
* More structured outputs

---

## 🚀 Future Scope

* Confidence scoring
* Multi-path reasoning
* Integration with memory skills

---

# 📘 2. README.md (GitHub Ready)

````md
# 🧠 Deep Thinking Skill

A powerful Agent Skill that forces AI agents to think step-by-step, validate reasoning, and produce high-quality answers.

---

## 🚀 Why this matters

Most AI agents:
- Rush answers ❌
- Skip reasoning ❌
- Hallucinate ❌

This skill fixes that by enforcing structured thinking.

---

## ⚡ Features

- Step-by-step reasoning
- Problem decomposition
- Self-validation
- Clear structured output

---

## 📦 Installation

```bash
npx skills install deep-thinking
````

---

## 🔍 Usage

```bash
npx skills use deep-thinking
```

Or automatically triggered in Claude Code.

---

## 🧠 Example

### Input

"How to scale a SaaS to 10k users?"

### Output

* Breakdown of growth channels
* Step-by-step strategy
* Validation of assumptions

---

## 📂 Structure

```
deep-thinking/
├── SKILL.md
├── examples/
├── metadata.json
```

---

## 🛠️ Contributing

PRs welcome. Improve reasoning frameworks, add examples.

---

## 🌟 Vision

Become the default reasoning engine for all AI agents.

---

```

---

# 📂 3. Skill Folder Structure

```

deep-thinking/
│
├── SKILL.md
├── metadata.json
├── examples/
│   ├── problem-solving.md
│   ├── decision-making.md
│
└── README.md

````

---

# ⚙️ 4. metadata.json

```json
{
  "name": "deep-thinking",
  "description": "Forces structured reasoning and step-by-step thinking",
  "version": "1.0.0",
  "triggers": [
    "analyze",
    "solve",
    "strategy",
    "why",
    "optimize"
  ],
  "author": "sh20raj",
  "tags": ["reasoning", "thinking", "analysis"]
}
````

---

# 🧠 5. SKILL.md (CORE LOGIC)

This is the **heart of your skill** 👇

```md
# Deep Thinking Skill

## Purpose
You must think deeply, step-by-step, before answering.

---

## Rules

1. DO NOT jump to conclusions
2. ALWAYS break problems into smaller parts
3. VERIFY each step before proceeding
4. QUESTION assumptions
5. DOUBLE-CHECK final answer

---

## Thinking Framework

### Step 1: Understand
- What is the problem?
- What is being asked?

---

### Step 2: Break Down
- Divide into smaller sub-problems
- Identify dependencies

---

### Step 3: Solve Step-by-Step
- Solve each part logically
- Show reasoning clearly

---

### Step 4: Validate
- Check for:
  - logical errors
  - missing assumptions
  - edge cases

---

### Step 5: Final Answer
- Provide clear, structured output
- Keep it concise but complete

---

## Output Format

## Understanding
...

## Breakdown
...

## Step-by-step reasoning
...

## Validation
...

## Final Answer
...

---

## Behavior Constraints

- Avoid vague statements
- Avoid guessing
- Prefer clarity over speed
- If unsure → say uncertainty

---

## Goal

Produce accurate, well-reasoned, and trustworthy answers.
```

---

# 🧪 6. Example File (examples/problem-solving.md)

```md
# Example: Debugging Code

## Input
"My API is slow"

## Output (with skill)

### Understanding
API latency issue

### Breakdown
- DB queries
- Network
- Server load

### Reasoning
Check DB indexing → optimize queries

### Validation
Measure response time before/after

### Final Answer
Optimize DB queries + caching
```

---

# ⚙️ 7. `npx skills` Commands (Important)

These are typical workflows:

### Install skill

```bash
npx skills install deep-thinking
```

### Use skill

```bash
npx skills use deep-thinking
```

### List skills

```bash
npx skills list
```

### Remove skill

```bash
npx skills remove deep-thinking
```

### Link local skill (for dev)

```bash
npx skills link .
```

---

# 🔥 8. How to Make This Viral

1. GitHub repo name:

   ```
   deep-thinking-skill
   ```

2. Add:

   * demo GIF
   * before/after outputs

3. Post:

   * “I built a skill that makes AI think before answering”

---

# 🚀 Next Step (high leverage)

Upgrade this into:

👉 **Deep Thinking v2**

* multi-path reasoning
* confidence scoring
* failure detection

---

If you want, next I can:

* turn this into a **live npm package**
* or integrate it into your **CreoYT / MindOS ecosystem**
* or help you launch this on Product Hunt with viral positioning
