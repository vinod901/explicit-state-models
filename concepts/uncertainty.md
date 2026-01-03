---
layout: default
title: Uncertainty and Confidence in State Models
---

# Uncertainty and Confidence in State Models

[← Back to Home](/)

## The Central Problem

State models typically represent state as **binary**: an entity is in State A or State B. Transitions are **discrete**: at time T, the entity moves from A to B.

But reality is rarely this clean:
- "The patient is probably ready for discharge, but we should double-check one thing"
- "Credit looks good, assuming the appraisal comes in as expected"
- "I think compliance reviewed this, but I'm not 100% certain"

**Uncertainty is inherent in real-world processes.** The question is: how do we represent it in explicit state models?

## Types of Uncertainty

### 1. State Uncertainty: "Where Are We?"

**Healthcare Example:**

The discharge checklist shows:
- ✓ Medical clearance
- ✓ Home care arranged
- ✓ Medications reconciled
- ⏳ Family availability

**Question:** Is the patient in state READY_TO_DISCHARGE or AWAITING_CONFIRMATIONS?

**Reality:** The daughter said "I'll be there," but didn't confirm the specific time. The nurse *thinks* it's settled but isn't certain.

**Implicit uncertainty:** "Probably ready, maybe 80% confident"

**Binary state model forces:** Pick one state (losing the nuance)

### 2. Transition Uncertainty: "What Should Happen Next?"

**Finance Example:**

Loan application is in CREDIT_REVIEW. The analyst has reviewed the financials.

**Possible transitions:**
- → APPROVED (if cash flow is sufficient)
- → PENDING_CONDITIONS (if need personal guarantee)
- → DECLINED (if cash flow is insufficient)

**Reality:** Cash flow is *borderline*. Analyst thinks: "It's close. Could go either way. Maybe 60% approve with conditions, 30% approve as-is, 10% decline."

**Discrete transition model forces:** Pick one transition (losing the probability distribution)

### 3. Data Uncertainty: "What Do We Know?"

**Healthcare Example:**

Home equipment status:
- ✓ Equipment ordered
- ✓ Company confirmed delivery
- ? Will it arrive before discharge?

**Reality:** Company said "We'll deliver Thursday morning." But:
- Their on-time rate is 85%
- Weather might delay delivery
- The address might be wrong (happened before)

**Binary checklist forces:** Mark as "confirmed" or "unconfirmed" (losing the confidence level)

### 4. Temporal Uncertainty: "When Will This Happen?"

**Finance Example:**

Appraisal status: "Ordered, appraiser has been assigned"

**Question:** When will appraisal report be ready?

**Reality:** 
- Appraiser usually takes 3-5 business days
- This property is complex, might take longer
- Appraiser is backed up, might be delayed
- Best estimate: 6 days, but could be 4 or could be 10

**State model typically:** "AWAITING_APPRAISAL" (no time estimate)

## Approaches to Modeling Uncertainty

### Approach 1: Ignore It (Forced Binary)

**Strategy:** Accept that state models are approximations. Make best judgment, pick a state.

**Example:**
```
State: READY_TO_DISCHARGE
Family availability: ✓ Confirmed

(Even though it's "90% sure, not 100%")
```

**Benefits:**
- Simple
- No additional complexity
- Clear decision-making

**Costs:**
- False precision (claims certainty that doesn't exist)
- Hides risk (problems appear suddenly when uncertainty resolves badly)
- Can't prioritize based on confidence (treat all "confirmed" equally)

**When appropriate:**
- Low-stakes decisions (uncertainty doesn't matter much)
- High confidence in most cases (rare to be wrong)
- Downstream processes can handle occasional errors

### Approach 2: Additional States for Uncertainty

**Strategy:** Create states that explicitly represent uncertain conditions.

**Example:**
```
States:
- READY_TO_DISCHARGE
- AWAITING_CONFIRMATIONS
- LIKELY_READY (new state: probably ready but not certain)
- TENTATIVELY_CLEARED (new state: cleared pending final check)
```

**Benefits:**
- Acknowledges uncertainty exists
- Allows different handling of uncertain vs certain states

**Costs:**
- State explosion (for each state, add uncertain variants)
- Ambiguity about which uncertain state to use
- Still doesn't capture degree of uncertainty (60% vs 90%)

**Example from Healthcare:**
```
Family availability:
- CONFIRMED: Daughter signed form, has backup plan
- LIKELY: Daughter verbally committed
- UNCERTAIN: Left message, awaiting callback
- NOT_ARRANGED: No contact with family
```

**When appropriate:**
- A few critical dimensions with uncertainty
- Clear categories of certainty levels
- Different actions based on certainty

### Approach 3: Confidence Scores

**Strategy:** Attach a confidence level to state assignments or checklist items.

**Example:**
```
State: READY_TO_DISCHARGE (Confidence: 75%)

Breakdown:
- Medical clearance: ✓ (100%)
- Home care arranged: ✓ (95%)
- Medications available: ✓ (90%)
- Family availability: ✓ (60%)
- Equipment delivered: ✓ (70%)
```

**Benefits:**
- Quantifies uncertainty
- Allows risk-based prioritization
- Can alert when confidence is low

**Costs:**
- False precision (is it really 75% vs 70%?)
- Maintenance burden (updating confidence scores)
- How to combine scores? (Math becomes complex)
- People game the numbers

**When appropriate:**
- Decisions have significant consequences
- Historical data exists to calibrate confidence
- Downstream systems can use confidence scores

### Approach 4: Parallel Tracking of Certainty

**Strategy:** Maintain explicit state, but also track what's uncertain.

**Example:**
```
State: READY_TO_DISCHARGE

Uncertainty Register:
- Family availability: Verbal commitment only, no written confirmation
- Equipment delivery: Scheduled for discharge day morning (timing risk)

Risk Assessment: Medium (2 uncertainties, 1 timing-sensitive)
```

**Benefits:**
- Explicit state for process flow
- Explicit uncertainty tracking for risk management
- Separates "what state we're in" from "how confident we are"

**Costs:**
- Duplicate tracking (state + uncertainty)
- Requires discipline to maintain
- Uncertainty register can grow unwieldy

**When appropriate:**
- High-stakes processes where risk matters
- Uncertainty affects multiple states
- Need both process automation and human oversight

### Approach 5: Probabilistic State

**Strategy:** Entity is in multiple states simultaneously, each with a probability.

**Example:**
```
Loan Application State Distribution:
- APPROVED: 30%
- APPROVED_WITH_CONDITIONS: 60%
- DECLINED: 10%

Based on:
- Cash flow analysis (borderline)
- Collateral value (pending appraisal)
- Industry trends (weakening)
```

**Benefits:**
- Captures reality (uncertainty is real)
- Allows portfolio-level risk assessment
- Supports probabilistic decision-making

**Costs:**
- Conceptually complex (people struggle with probability)
- How to transition? (When does 60% become 100%?)
- Most systems not built for probabilistic state
- Difficult to audit ("Why was it 60% not 70%?")

**When appropriate:**
- Sophisticated users (data scientists, risk analysts)
- Aggregate analysis matters (portfolio risk)
- Uncertainty is central to decision-making

## Real-World Examples

### Healthcare: Three Approaches to Family Availability

**Hospital A: Binary (Approach 1)**
```
✓ Family availability confirmed
(Nurse judgment: mark it confirmed when daughter says "I'll be there")
```
**Result:** 15% of discharges delayed because family unavailable despite "confirmed"

**Hospital B: States for Uncertainty (Approach 2)**
```
States: CONFIRMED | LIKELY | UNCERTAIN
Current: LIKELY (verbal commitment, no written plan)
```
**Result:** Discharge coordinator knows to do extra confirmation for "LIKELY" cases

**Hospital C: Uncertainty Register (Approach 4)**
```
State: READY_TO_DISCHARGE
Uncertainty: Family gave verbal commitment but has work conflict, trying to arrange backup
Risk Flag: Yellow (discharge may need to be delayed)
```
**Result:** Proactive outreach to family, backup plan arranged, discharge succeeds

### Finance: Three Approaches to Collateral Value

**Bank A: Binary (Approach 1)**
```
State: COLLATERAL_REVIEW
Appraisal value: $420,000
LTV: 119%
Status: ISSUE (exceeds 80% guideline)
```
**Result:** Clear, but appraisal value is estimate; actual liquidation might be lower

**Bank B: Confidence Score (Approach 3)**
```
State: COLLATERAL_REVIEW
Appraisal value: $420,000 (Confidence: 75%)
  - Market is volatile
  - Comparable sales limited
  - Equipment is specialized
LTV: 119% (likely range: 110-125%)
```
**Result:** Credit decision incorporates uncertainty; might require additional cushion

**Bank C: Probabilistic (Approach 5)**
```
Collateral Value Distribution:
- $380K: 10%
- $420K: 50%
- $450K: 30%
- $480K: 10%

Expected LTV: 119% (range: 104% - 132%)
```
**Result:** Risk models use distribution; pricing reflects uncertainty

## The Trade-off Between Precision and Usability

**More precise uncertainty modeling:**
- Captures reality better
- Supports better risk management
- Enables more sophisticated analysis

**But:**
- Increases complexity
- Harder for people to understand and use
- Requires more maintenance
- May not be actionable

**Key question:** Will downstream users actually use the uncertainty information? Or will they treat "75% confident" the same as "100% confident" because they don't know what to do differently?

## When Uncertainty Matters Most

Uncertainty modeling is most valuable when:

### 1. High Consequences

- Patient safety (delayed discharge with no home care = danger)
- Financial loss (loan defaults, collateral shortfall)
- Compliance failure (regulatory violation)

### 2. Long Lead Times

- Decisions made today affect outcomes weeks later
- Uncertainty can grow or resolve over time
- Early awareness enables proactive mitigation

### 3. Resource Constraints

- Limited capacity to handle problems
- Need to prioritize based on risk
- Can't afford to treat all cases equally

### 4. Coordination Across Parties

- Multiple organizations involved
- Different parties have different information
- Uncertainty affects handoffs

### 5. Learning and Improvement

- Track what was uncertain and how it resolved
- Improve predictions over time
- Calibrate confidence scores

## When Uncertainty Modeling Isn't Worth It

Uncertainty modeling is likely not valuable when:

### 1. Routine, Low-Stakes

- Many similar cases
- Consequences of errors are small
- Simple recovery if wrong

### 2. Immediate Feedback

- Uncertainty resolves quickly
- Can verify state easily
- Correction is fast

### 3. No Differential Action

- Would handle case the same way regardless of confidence
- Uncertainty information not actionable
- Downstream systems can't use it

### 4. Insufficient Data

- Can't calibrate confidence scores meaningfully
- No historical basis for probabilities
- Confidence becomes subjective guess

## Representing What You Don't Know

Sometimes the most honest state model acknowledges explicit ignorance:

### Example: Equipment Delivery Status

**Bad:**
```
✓ Equipment delivery confirmed
```
(When reality is: we called, left a message, no callback)

**Better:**
```
⏳ Equipment delivery pending confirmation
```
(Shows it's not confirmed, but doesn't say we have a problem)

**More honest:**
```
⚠ Equipment delivery: Called vendor, no response yet, status unknown
Expected resolution: Callback by 3 PM today
Escalation plan: If no response by 3 PM, supervisor will call vendor manager
```

**This captures:**
- We don't know the current state
- We're waiting for information
- We have a plan to resolve the uncertainty
- We have a backup plan

## Implications

1. **State models are always approximations** - they simplify messy reality
2. **Uncertainty is real** - pretending it doesn't exist creates risk
3. **Multiple approaches exist** - from binary to probabilistic, each with trade-offs
4. **Context determines approach** - high-stakes and high-uncertainty benefit most from explicit uncertainty modeling
5. **Usability matters** - complex uncertainty models are only useful if people use them

The goal is not to eliminate uncertainty (impossible) but to **represent it honestly** in ways that **enable better decisions**.

Sometimes that means simple binary states with human judgment. Sometimes it means confidence scores and risk flags. The right choice depends on the domain, the stakes, and the users.

---

**Continue to:** [Failure Modes →](/concepts/failure-modes)

**Explore:** [Healthcare Scenario](/scenarios/healthcare) | [Finance Scenario](/scenarios/finance) | [Core Concepts](/concepts/core) | [Back to Home](/)