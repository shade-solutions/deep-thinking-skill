# Example: Logic & Probability — The Monty Hall Deception

> **Domain**: Mathematics · Probability Theory · Bayesian Reasoning  
> **Difficulty**: Moderate  
> **Frameworks Used**: First Principles Decomposition, Adversarial Verification  

## 💬 User Prompt

> "We have three doors. Behind one is a car, behind the others are goats. You pick Door 1. The host, who knows what's behind the doors, opens Door 3, which has a goat. Should you switch to Door 2?"

---

## Phase 1: Problem Intelligence 🔍

### 1.1 Restatement
This is the classic **Monty Hall Problem** — a conditional probability puzzle where the host's informed action changes the sample space. The question is whether switching doors after a reveal increases the win probability.

### 1.2 Objective
**Determine the mathematically optimal strategy** (switch or stay) and prove it rigorously.

### 1.3 Constraint Space
| Dimension | Value |
|-----------|-------|
| **Hard Constraints** | Standard Monty Hall rules: host always opens a losing door, host never opens your door, host never opens the car door |
| **Unknowns** | None — this is a fully specified problem |

### 1.4 Hidden Assumptions
- 🟢 The host **must** open a door (not optional)
- 🟢 The host **knows** what's behind each door (informed action)
- 🟢 The car placement is uniformly random
- 🔴 **Dangerous if violated**: If the host opens a door at random (and it happens to be a goat), the analysis changes completely (it becomes 50/50). The word "who knows" in the problem statement confirms informed action.

---

## Phase 2: Decomposition Engine 🔧

### Sub-problems
1. **Prior Probabilities**: What are the initial odds?
2. **Information Update**: How does the host's action change the probability distribution?
3. **Bayesian Update**: Apply Bayes' theorem to compute posterior probabilities.

---

## Phase 3: Systematic Reasoning 🧮

### Step 1: Enumerate All Scenarios

| Car Location | You Pick | Host Opens | Switch Result | Stay Result |
|---|---|---|---|---|
| Door 1 | Door 1 | Door 2 or 3 | 🐐 LOSE | 🚗 WIN |
| Door 2 | Door 1 | Door 3 (forced) | 🚗 WIN | 🐐 LOSE |
| Door 3 | Door 1 | Door 2 (forced) | 🚗 WIN | 🐐 LOSE |

### Step 2: Count the Outcomes
- **Switching wins**: 2 out of 3 scenarios → **P(win|switch) = 2/3**
- **Staying wins**: 1 out of 3 scenarios → **P(win|stay) = 1/3**

### Step 3: Bayesian Proof
Using Bayes' theorem where H = host opens Door 3, C_i = car behind Door i:

```
P(C₂|H) = P(H|C₂) × P(C₂) / P(H)

P(H|C₁) = 1/2  (host can open Door 2 or 3)
P(H|C₂) = 1    (host MUST open Door 3, since you picked Door 1)
P(H|C₃) = 0    (host can't open Door 3 if car is there)

P(H) = P(H|C₁)P(C₁) + P(H|C₂)P(C₂) + P(H|C₃)P(C₃)
     = (1/2)(1/3)   + (1)(1/3)       + (0)(1/3)
     = 1/6 + 1/3 + 0 = 1/2

P(C₂|H) = (1 × 1/3) / (1/2) = 2/3 ✅
P(C₁|H) = (1/2 × 1/3) / (1/2) = 1/3 ✅
```

### Verification via Monte Carlo Simulation
```python
import random

def monty_hall(switch, trials=1_000_000):
    wins = 0
    for _ in range(trials):
        car = random.randint(0, 2)
        pick = 0  # always pick Door 0
        # Host opens a door that is not the car and not the pick
        doors = [d for d in range(3) if d != pick and d != car]
        host_opens = random.choice(doors)
        if switch:
            pick = [d for d in range(3) if d != pick and d != host_opens][0]
        if pick == car:
            wins += 1
    return wins / trials

print(f"Stay:   {monty_hall(switch=False):.4f}")  # ~0.3333
print(f"Switch: {monty_hall(switch=True):.4f}")   # ~0.6667
```

Simulation confirms: **switching wins 66.7% of the time**.

---

## Phase 4: Multi-Path Analysis 🔀

| Strategy | Win Probability | Proof Method |
|----------|----------------|-------------|
| **Always Switch** | 2/3 (66.7%) | Enumeration, Bayes, simulation |
| **Always Stay** | 1/3 (33.3%) | Enumeration, Bayes, simulation |
| **Random Choice** | 1/2 (50.0%) | Expected value of 50/50 coin flip |

Three independent proof methods (enumeration, Bayesian, empirical simulation) all converge on the same answer. This is a **high-confidence result**.

---

## Phase 5: Failure Mode Detection 🛡️

### When This Analysis Breaks
The 2/3 result depends on **specific rules**. It changes if:

| Rule Violation | New Probability |
|---|---|
| Host opens randomly (not informed) | 50/50 |
| Host can open the car door | Problem changes entirely |
| Host doesn't always open a door | Need game theory analysis |
| More than 3 doors | P(switch) = (n-1)/n for n doors |

The key insight is: **the host's knowledge is what makes switching valuable**. Without informed action, there is no information transfer.

---

## Phase 6: Confidence Scoring 📊

| Metric | Score |
|--------|-------|
| Problem Understanding | 100 |
| Solution Correctness | 100 |
| Completeness | 95 |
| Evidence Quality | 100 |
| **Overall Confidence** | **99** |

This is a mathematically proven result verified by three independent methods.

---

## Phase 7: Final Output 📋

### 📌 Executive Summary
**Always switch.** Switching gives you a 2/3 (66.7%) chance of winning. Staying gives you 1/3 (33.3%). This is proven by enumeration, Bayesian inference, and Monte Carlo simulation.

### 🎯 The Key Insight
Your initial pick has a 1/3 chance of being right. The other two doors collectively hold 2/3 of the probability. When the host eliminates one of those doors (guaranteed to be a loser), the entire 2/3 collapses onto the remaining door. The host's **informed action** is what creates the asymmetry.

### ⚠️ Caveats
- This only applies when the host **always** opens a losing door and **knows** where the car is
- In the "ignorant host" variant (host opens randomly), the answer is 50/50

### 📊 Confidence: **99/100**
This is a settled mathematical proof. The only source of error would be misunderstanding the problem rules.
