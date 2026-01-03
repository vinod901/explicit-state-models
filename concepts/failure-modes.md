---
layout: default
title: Failure Modes of Explicit State Models
---

# Failure Modes of Explicit State Models

[← Back to Home](/)

## Introduction

Explicit state models are not a universal solution. They can fail, create new problems, or make situations worse. This page catalogs the ways explicit state modeling goes wrong, drawn from the scenarios and real-world patterns.

Understanding failure modes is as important as understanding benefits. It prevents over-application and helps design better models when they are appropriate.

## 1. Reality Doesn't Fit the Model

### The Problem

Real-world situations are complex and varied. State models create categories. Sometimes reality doesn't fit the categories.

### Example: Healthcare Home Care

**The Model:**
```
Home Care Plan: [None | Family | Home Health | Skilled Nursing Facility]
```

**The Reality:**
Mrs. Chen's daughter can be there evenings and weekends. A neighbor (retired nurse) will check in daily. Home health will come 3x/week. Family will hire a part-time aide for the other days.

**Which category is this?** 

- "Family" - undersells the professional support
- "Home Health" - oversells their frequency
- Custom option? - defeats the standardization

**The Failure:**
- Model forces an imprecise choice
- Staff add workaround notes (defeating structured capture)
- Or, model is expanded with more categories (complexity explosion)

### Why This Happens

Models simplify to make reality manageable. But:
- Edge cases are more common than expected
- Domains evolve (new care models emerge)
- Each organization has unique patterns

### Consequences

- Staff frustration ("the system doesn't match reality")
- Shadow documentation (notes outside the model)
- Bad data ("I'll pick the closest option even though it's not right")
- Gaming ("I know what to select to get through the system")

## 2. False Precision and False Certainty

### The Problem

Marking something as "complete" or "approved" suggests certainty that may not exist. Explicit state can provide false confidence.

### Example: Finance Credit Approval

**The Model:**
```
State: APPROVED
Credit Decision: PASS
Approval Date: March 16, 2024
Approver: Sarah Chen
```

**The Reality:**
Sarah reviewed the application and thought: "Cash flow is borderline. Industry trends are concerning. If I had more time, I'd dig deeper. But I have 15 other applications to review. It probably passes our guidelines. I'll approve it."

**The System Shows:**
"✓ APPROVED" (looks certain)

**The Reality:**
"Maybe 70% confident, rushed decision, lingering concerns"

### Why This Happens

Binary state (approved/declined) doesn't capture:
- Confidence levels
- Conditional nature of decisions
- Concerns that didn't rise to declining level
- Information quality and completeness

### Consequences

- Downstream users assume more certainty than exists
- Risk is hidden until it materializes
- No way to identify "weak" approvals for extra scrutiny
- Blame when problems emerge ("but you approved it!")

## 3. Process Rigidity

### The Problem

Explicit state models define allowed transitions. This can prevent necessary flexibility.

### Example: Finance Loan Review Sequence

**The Model:**
```
CREDIT_REVIEW → COLLATERAL_REVIEW → COMPLIANCE_REVIEW → PRICING
(strict sequence)
```

**The Reality:**
During collateral review, Mike discovers the borrower is restructuring ownership. This affects the credit decision (changes personal guarantee availability) and compliance review (new ownership structure).

**The Model Says:**
Can't go backward to CREDIT_REVIEW. Must complete sequence.

**What Should Happen:**
Credit and compliance need to re-review based on new information.

**The Workaround:**
- Mike adds notes (hoping someone reads them)
- Or, the application is "rejected" and resubmitted (gaming the process)
- Or, complex "re-review" state is added (model becomes complex)

### Why This Happens

Real processes are:
- Iterative (new information changes previous decisions)
- Parallel (multiple things happen simultaneously)
- Context-dependent (flow varies by situation)

Linear state models don't capture this fluidity.

### Consequences

- Important information gets lost in workaround notes
- Process delays while waiting for "proper" sequence
- Gaming (force the model to allow what's needed)
- Complexity growth (add states for every variation)

## 4. Coordination Overhead

### The Problem

Explicit state tracking requires work. Someone must update the system, maintain data, resolve ambiguities. This is new overhead.

### Example: Healthcare Discharge Dashboard

**Before Explicit State:**
- Nurse knows Mrs. Chen's discharge status
- When asked, nurse provides update
- Nurse makes judgments about what matters

**After Explicit State:**
- Nurse must update dashboard continuously
- Each checklist item requires explicit status
- Ambiguous situations require classification decisions
- Time spent on data entry instead of patient care

**The Trade-off:**
- Better visibility for others (physicians, case managers)
- But more work for nurse
- Is the benefit worth the cost?

### Why This Happens

Explicit state externalizes knowledge that was internal. This requires:
- Deciding what to record
- Choosing how to classify it
- Updating systems
- Training people
- Handling exceptions

All of this is **new work**.

### Consequences

- Resistance from staff ("more bureaucracy")
- Incomplete or low-quality data (if rushed)
- Time taken from core work (patient care, customer service)
- Benefits only realized if everyone participates

## 5. Gaming and Performance Theater

### The Problem

When explicit state becomes a measured metric, people optimize for the metric rather than the underlying goal.

### Example: Healthcare Discharge Timing

**The Model:**
```
Metric: Percentage of patients in READY_TO_DISCHARGE moved to DISCHARGED within 4 hours
```

**The Intent:**
Reduce delays once discharge is ready.

**The Gaming:**
- Don't mark as READY_TO_DISCHARGE until patient is literally walking out
- Keep in AWAITING_CONFIRMATIONS even when actually ready
- Mark as DISCHARGED before patient leaves (to hit metric)

**The Result:**
- Metric looks good
- But actual process hasn't improved
- State model becomes inaccurate

### Why This Happens

Goodhart's Law: "When a measure becomes a target, it ceases to be a good measure."

People adapt to metrics:
- Optimize for what's measured, not what matters
- Find shortcuts to show compliance
- Avoid triggering unfavorable classifications

### Consequences

- Loss of data integrity (state doesn't reflect reality)
- Misaligned incentives (game the system vs improve outcomes)
- Difficulty detecting gaming (looks like success)
- Need for increasingly complex rules (arms race)

## 6. State Explosion

### The Problem

As exceptions and variations are encountered, the model grows. More states, more transitions, more rules. Eventually it becomes unwieldy.

### Example: Loan Application States (Over Time)

**Initial Model (10 states):**
```
SUBMITTED, CREDIT_REVIEW, COLLATERAL_REVIEW, COMPLIANCE_REVIEW,
PENDING_CONDITIONS, PRICING, FINAL_APPROVAL, CLOSING, FUNDED, DECLINED
```

**After 1 Year (23 states):**
```
SUBMITTED, ASSIGNED,
CREDIT_REVIEW, CREDIT_REVIEW_ADDITIONAL_INFO, CREDIT_REVIEW_MANAGER_ESCALATION,
COLLATERAL_REVIEW, COLLATERAL_REAPPRAISAL, COLLATERAL_PARTIAL,
COMPLIANCE_REVIEW, COMPLIANCE_REVIEW_ENHANCED,
PENDING_CONDITIONS, PENDING_CONDITIONS_PARTIAL, PENDING_CONDITIONS_OVERDUE,
PRICING, PRICING_EXCEPTION, PRICING_MANAGER_APPROVAL,
FINAL_APPROVAL, FINAL_APPROVAL_EXECUTIVE,
CLOSING, CLOSING_DELAYED,
FUNDED, DECLINED, WITHDRAWN
```

**Now:**
- Staff confused about which state to use
- Training is complex
- System maintenance is burdensome
- Benefits of explicit state are lost in complexity

### Why This Happens

Every real-world variation can trigger:
- "We need a state for this special case"
- "This transition should be tracked separately"
- "This status needs its own category"

Without discipline, models grow unbounded.

### Consequences

- Cognitive overload (can't remember all states)
- Classification ambiguity (which state applies?)
- Maintenance burden (changing the model is hard)
- Loss of utility (too complex to be useful)

## 7. Boundaries and Handoffs

### The Problem

Explicit state works within a system or organization. But real processes cross boundaries. State at boundaries remains implicit.

### Example: Healthcare Equipment Delivery

**Hospital's State Model:**
```
Equipment Status:
- IDENTIFIED
- ORDERED
- DELIVERY_CONFIRMED
- DELIVERED
```

**Equipment Company's Internal States:**
```
Order Received → Inventory Check → Picking → Packing → Dispatch → Out for Delivery → Delivered
```

**The Problem:**
- Hospital sees "DELIVERY_CONFIRMED" (binary)
- Company knows order is in "Picking" stage
- Hospital has no visibility into company's actual state
- When delay occurs, hospital doesn't know why

**The Reality:**
Explicit state in one organization doesn't extend to partners. Handoffs still rely on implicit coordination.

### Why This Happens

- Different organizations have different systems
- Integration is expensive and complex
- Partners may not want to share detailed state
- No standard models across organizations

### Consequences

- State visibility ends at organizational boundary
- Coordination across boundaries remains implicit
- Dependencies on external parties are opaque
- Benefits of explicit state are limited to internal processes

## 8. Resistance to Reality

### The Problem

Explicit models can make it harder to acknowledge that reality has changed in ways the model doesn't accommodate.

### Example: Finance Industry Changes

**Original Model:**
```
Loan Types: [Commercial Real Estate | Equipment | Working Capital | Term Loan]
```

**New Reality:**
Fintech companies offer revenue-based financing (repayment tied to revenue, not fixed schedule). This is:
- Not a term loan (no fixed schedule)
- Not working capital (secured by future revenue)
- Not equipment (no physical collateral)
- Sort of commercial, but different structure

**The Resistance:**
"We don't have a category for that, so we don't offer it" or "Force it into Working Capital even though it doesn't fit"

**The Problem:**
Model becomes a constraint on adaptation.

### Why This Happens

Changing state models is work:
- Update system code
- Retrain staff
- Modify reports
- Test changes

Easier to keep the existing model, even if reality has moved on.

### Consequences

- Organization becomes less adaptive
- Innovation is discouraged (doesn't fit the model)
- Workarounds proliferate (shadow state tracking)
- Competitive disadvantage (can't offer what market wants)

## 9. Loss of Judgment and Intuition

### The Problem

Experienced staff develop intuition. Explicit state models can override this judgment.

### Example: Healthcare Nurse Judgment

**Nurse's Intuition:**
"Mrs. Chen's checklist is complete, but she seems anxious and confused about the discharge. Something doesn't feel right. Maybe we should wait."

**The System:**
```
State: READY_TO_DISCHARGE
All checklist items: ✓
Recommendation: Proceed with discharge
```

**The Pressure:**
System says ready. Physician expects discharge. Bed is needed for new admission.

**The Risk:**
Nurse's intuition is overridden by explicit model. Patient is discharged. Falls at home two days later.

### Why This Happens

Models capture what can be formalized:
- Measurable conditions
- Binary checklist items
- Documented confirmations

Models can't capture:
- Subtle concerns
- Pattern recognition from experience
- Holistic assessment
- "Something doesn't feel right"

### Consequences

- Valuable expertise is discounted
- Experts become rule-followers
- Situations that "technically" meet criteria but shouldn't proceed
- When things go wrong, blame the expert ("you approved it") not the model

## 10. Premature Standardization

### The Problem

Creating explicit state models too early, before understanding the domain fully, leads to poor models that don't match reality.

### Example: New Process Modeling

**Scenario:**
Organization starts a new service line. After two weeks, someone says "We should create a state model for tracking these cases."

**The Model (premature):**
```
States: INTAKE → REVIEW → DECISION → COMPLETE
```

**What's Learned Over Next 6 Months:**
- 40% of cases need multiple rounds of review
- 25% go on hold waiting for external information
- 15% require escalation to senior staff
- Some cases are fast-tracked (different flow)
- Some cases are exploratory (no decision needed)

**The Result:**
- Initial model doesn't fit actual patterns
- Workarounds and exceptions proliferate
- Must redesign model (disruptive change)
- Or, live with poor model (low data quality)

### Why This Happens

Impulse to "get organized" and "create structure" before understanding what structure is appropriate.

### Consequences

- Model redesign is disruptive (change systems, retrain, migrate data)
- Premature commitment to wrong structure
- Staff cynicism ("we're changing the model again?")
- Better to observe the implicit patterns first, then model explicitly

## When Explicit State Makes Things Worse

Explicit state modeling is likely to make things worse when:

### 1. Processes Are Still Evolving

- New domain, limited experience
- Patterns not yet clear
- Frequent changes expected

**Better approach:** Let process run implicitly, observe patterns, model later.

### 2. Situations Are Highly Variable

- Every case is unique
- Standard categorization doesn't fit
- Judgment dominates rules

**Better approach:** Light-touch tracking, focus on outcomes not state.

### 3. Overhead Exceeds Benefits

- Low-volume processes (few cases)
- Low-stakes outcomes (errors don't matter much)
- Already working well (no coordination problems)

**Better approach:** Don't model what doesn't need modeling.

### 4. Culture Resists Structure

- Staff value autonomy and judgment
- Organization has informal, flexible culture
- Explicit rules are seen as bureaucracy

**Better approach:** Start with minimal model, demonstrate value before expanding.

### 5. Cross-Organizational Dependencies Dominate

- Critical state is external (partner organizations)
- Handoffs are infrequent but complex
- No leverage to impose standards on partners

**Better approach:** Model internal state, but acknowledge external dependencies remain implicit.

## Designing to Avoid Failure Modes

When explicit state modeling is appropriate, design choices can mitigate failure modes:

### 1. Start Minimal

- Few states (5-10, not 50)
- Focus on critical transitions
- Grow model only based on demonstrated need

### 2. Include Escape Valves

- "Other" categories with notes
- "Escalation" states for unusual cases
- Manual override capability (with logging)

### 3. Acknowledge Uncertainty

- States for "pending confirmation"
- Flags for "low confidence"
- Separate "what we know" from "what we think"

### 4. Measure Reality, Not Just Compliance

- Audit: does state match reality?
- Check: are people gaming the model?
- Ask: is the model still useful?

### 5. Revisit and Evolve

- Scheduled model reviews (annually?)
- Simplification efforts (remove unused states)
- Adaptation to new realities

## Implications

1. **Explicit state models can fail** - and when they do, they can make things worse
2. **Failure modes are predictable** - rigidity, overhead, gaming, complexity growth
3. **Not everything should be modeled** - some processes are better left implicit
4. **Design choices matter** - thoughtful design can mitigate (not eliminate) failure modes
5. **Models should be revisited** - what worked initially may not work as context evolves

The goal is not to avoid explicit state modeling because of failure modes. The goal is to **apply it thoughtfully**, **acknowledge its limitations**, and **design for adaptation**.

---

**Explore:** [Healthcare Scenario](/scenarios/healthcare) | [Finance Scenario](/scenarios/finance) | [Core Concepts](/concepts/core) | [Uncertainty](/concepts/uncertainty) | [Back to Home](/)