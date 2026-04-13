# Example: Logic & Probability Problem Solving

## User Prompt
"We have three doors. Behind one is a car, behind the others are goats. You pick Door 1. The host, who knows what's behind the doors, opens Door 3, which has a goat. Should you switch to Door 2?"

---

## 🧠 Deep Thinking Process

### 1. Problem Discovery
- **Goal**: Maximize the probability of winning the car.
- **Constraints**: Standard Monty Hall rules apply.
- **Assumption**: The host *must* open a door with a goat and *cannot* open the door you picked.

### 2. Decomposition
- Initial probabilities for each door.
- Impact of the host's action on the sample space.
- Conditional probability calculation.

### 3. Step-by-Step Reasoning
- **Initial State**: Each door has a 1/3 chance: P(D1)=1/3, P(D2)=1/3, P(D3)=1/3.
- **Action**: User picks D1. Probability of car being in {D2, D3} is 2/3.
- **Host Action**: Host opens D3. Since the host knows where the car is, they *guarantee* D3 has a goat.
- **Verification**: The 2/3 probability originally assigned to the set {D2, D3} now collapses entirely onto D2, because D3 is confirmed as 0.

### 4. Validation
- **Pre-Mortem**: What if the host opened a random door? If it was random and happened to be a goat, the odds would be 50/50. But here, the host's choice is *informed*.
- **Sync Check**: Does switching actually help? Yes, it doubles the win chance from 1/3 to 2/3.

---

## 🏁 Final Answer

### Executive Summary
Yes, you should switch to Door 2. Switching gives you a 2/3 chance of winning, while staying keeps you at 1/3.

### Detailed Logic
By picking Door 1, you are betting on a 1/3 event. The remaining 2/3 chance is spread over Doors 2 and 3. When the host eliminates Door 3, the entire 2/3 "weight" is transferred to Door 2.

### Implementation Tip
Always switch in scenarios where the "host" or "system" eliminates options based on hidden knowledge.
