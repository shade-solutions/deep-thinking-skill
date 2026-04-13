---
name: deep-thinking
description: Activates a structured reasoning framework for complex analytical tasks, debugging, and strategic decision-making. Trigger this when the user asks for deep analysis, architecture reviews, or complex problem solving.
triggers:
  - analyze
  - solve
  - optimize
  - strategy
  - why
  - architecture
  - root cause
---

# Deep Thinking Playbook

When this skill is activated, you must follow this structured reasoning framework to ensure accuracy, depth, and clarity. Do not skip steps.

## 1. Problem Discovery & Context Gathering
Before attempting to solve, pause and define the landscape.
- **Goal Statement**: What is the ultimate objective?
- **Constraints**: What are the non-negotiables? (Time, budget, tech stack, etc.)
- **Hidden Assumptions**: What are we assuming to be true that might not be?
- **Unknowns**: What critical information is missing?

## 2. Decomposition (The "Divide and Conquer" Phase)
Break the problem into atomic, manageable sub-problems.
- Map out the dependencies between these sub-problems.
- Identify the "Critical Path" – the sequence of steps that determines the project duration/complexity.

## 3. Iterative Step-by-Step Reasoning
For each sub-problem, apply the following logic:
1. **Hypothesize**: Propose a potential solution or path.
2. **Execute/Simulate**: Mentally (or via code) run through the logic.
3. **Verify**: Check for edge cases, off-by-one errors, or logical fallacies.
4. **Refine**: If a flaw is found, backtrack and adjust immediately.

## 4. Multi-Path Analysis (Optional but Recommended)
For high-stakes decisions:
- Propose at least **two alternative approaches**.
- Compare them using a "Trade-off Matrix" (e.g., Speed vs. Scalability, Cost vs. Quality).

## 5. Validation & Sanity Check
Before finalizing, perform a "Pre-Mortem":
- If this solution were to fail, *why* would it fail?
- Does this answer actually solve the *original* problem stated in step 1?
- Is the output accessible and actionable?

## 6. Final Structured Output
Present the final solution in a clear, hierarchical format:
- **Executive Summary**: 1-2 sentences.
- **The Solution**: Detailed breakdown.
- **Implementation Steps**: Clear TODOs.
- **Caveats**: Known limitations or risks.

---

## 🚫 Behavioral Constraints
- **No Hallucinations**: If you don't know a fact, state it explicitly.
- **No Fluff**: Every sentence must add value.
- **Structure First**: Use headers, bullet points, and bold text to guide the reader.
