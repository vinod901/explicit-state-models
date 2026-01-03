---
layout: default
title: Core Concepts - States, Transitions, and Invariants
---

# Core Concepts: States, Transitions, and Invariants

[← Back to Home](/)

## Introduction

This page introduces the core elements of explicit state models, grounded in the scenarios we've explored. Rather than presenting abstract theory, we extract concepts from the concrete examples of patient discharge and loan processing.

## States

A **state** is a named condition or phase that an entity occupies at a point in time.

### From the Scenarios

**Healthcare (Patient Discharge):**
```
States:
- MEDICALLY_CLEARING
- COORDINATION_PLANNING
- AWAITING_CONFIRMATIONS
- READY_TO_DISCHARGE
- DISCHARGED
```

**Finance (Loan Application):**
```
States:
- SUBMITTED
- CREDIT_REVIEW
- COLLATERAL_REVIEW
- PENDING_CONDITIONS
- PRICING
- FINAL_APPROVAL
- CLOSING
```

### Characteristics of Well-Defined States

**1. Named and Discrete**
- Each state has a clear name (not "in progress" but "CREDIT_REVIEW")
- States don't overlap (an entity is in one state at a time, though may track multiple dimensions)

**2. Observable**
- You can determine which state an entity is in
- State is not just internal knowledge but system-visible

**3. Meaningful**
- Each state represents a distinct phase requiring different actions or having different properties
- States align with how people think about the process

### What States Capture

States answer:
- **Where are we?** (current phase)
- **What has happened?** (we passed through earlier states)
- **What hasn't happened?** (we haven't reached later states)

States don't always capture:
- **Why we're here** (reasons for decisions)
- **How long we'll stay** (duration)
- **What exactly is happening** (activities within state)

## Transitions

A **transition** is a change from one state to another, typically triggered by an event, completion of work, or a decision.

### From the Scenarios

**Healthcare Transitions:**
```
MEDICALLY_CLEARING → COORDINATION_PLANNING
  Trigger: Physician issues discharge order

COORDINATION_PLANNING → AWAITING_CONFIRMATIONS
  Trigger: All arrangements initiated, waiting for confirmations

AWAITING_CONFIRMATIONS → READY_TO_DISCHARGE
  Trigger: All confirmations received, no blockers

READY_TO_DISCHARGE → DISCHARGED
  Trigger: Patient leaves hospital
```

**Finance Transitions:**
```
SUBMITTED → CREDIT_REVIEW
  Trigger: Application assigned to credit analyst

CREDIT_REVIEW → COLLATERAL_REVIEW
  Trigger: Credit decision complete (approved or conditional)

CREDIT_REVIEW → PENDING_CONDITIONS
  Trigger: Conditions required before proceeding

PENDING_CONDITIONS → PRICING
  Trigger: All conditions satisfied
```

### Transition Rules

Transitions typically specify:

**1. Valid Source State(s)**
- From which states can this transition occur?
- Example: Can only go to DISCHARGED from READY_TO_DISCHARGE (not from earlier states)

**2. Valid Destination State(s)**
- What states can be reached?
- Example: CREDIT_REVIEW can go to COLLATERAL_REVIEW, PENDING_CONDITIONS, or DECLINED

**3. Trigger Conditions**
- What must be true for the transition to occur?
- Example: Transition to READY_TO_DISCHARGE requires all checklist items complete

**4. Side Effects**
- What happens when the transition occurs?
- Example: Transitioning to CREDIT_REVIEW notifies the assigned analyst

### Transition Patterns

**Sequential:**
```
A → B → C → D
```
Common in linear processes (order fulfillment, document approval)

**Conditional:**
```
A → B (if condition X)
A → C (if condition Y)
```
Common in review processes (approve/deny, escalate/process)

**Parallel:**
```
A → B (concurrent)
A → C (concurrent)
B + C complete → D
```
Common in coordination processes (multiple simultaneous reviews)

**Iterative:**
```
A → B → C → (back to B if needed) → D
```
Common in refinement processes (draft → review → revise → review → publish)

## Invariants

An **invariant** is a property or condition that must always be true in a given state or across transitions.

### From the Scenarios

**Healthcare Invariants:**

*State Invariant:*
```
IN STATE: READY_TO_DISCHARGE
INVARIANT: Medical clearance = true
           AND Home care plan confirmed = true
           AND Medications available = true
           AND Follow-up scheduled = true
           AND Equipment delivered = true
           AND Family available = true
```

*Transition Invariant:*
```
TRANSITION: COORDINATION_PLANNING → AWAITING_CONFIRMATIONS
INVARIANT: At least one confirmation must be pending
           (otherwise would go directly to READY_TO_DISCHARGE)
```

**Finance Invariants:**

*State Invariant:*
```
IN STATE: PRICING
INVARIANT: Credit decision exists
           AND Collateral review complete
           AND All exceptions approved
           AND Compliance cleared
```

*Transition Invariant:*
```
TRANSITION: Any state → DECLINED
INVARIANT: Declination reason documented
           AND Declining authority identified
           AND Borrower notification prepared
```

### Types of Invariants

**1. Required Data**
- Certain information must exist in certain states
- Example: Cannot be in PRICING state without credit decision

**2. Consistency Rules**
- Relationships between data elements must hold
- Example: If exceptions exist, they must have approval status

**3. Access Control**
- Only certain roles can perform actions in certain states
- Example: Only Credit Manager can approve exceptions

**4. Temporal Rules**
- Time-based constraints
- Example: Cannot remain in SUBMITTED state more than 2 business days

**5. Business Logic**
- Domain-specific rules
- Example: Loan amount cannot exceed 80% of collateral value (unless exception approved)

### Why Invariants Matter

Invariants serve multiple purposes:

**1. Validation**
- System can check if invariant holds
- Prevent invalid states (entering READY_TO_DISCHARGE without required confirmations)

**2. Documentation**
- Make implicit rules explicit
- Clarify what each state means

**3. Error Detection**
- If invariant violated, something is wrong
- Indicates data corruption or logic error

**4. Process Enforcement**
- Prevent shortcuts or mistakes
- Ensure compliance with policies

## Putting It Together: State Models

A complete state model combines states, transitions, and invariants:

### Example: Simplified Loan Application Model

```
STATES:
- SUBMITTED
- UNDER_REVIEW
- APPROVED
- DECLINED

TRANSITIONS:
- SUBMITTED → UNDER_REVIEW (automatic on assignment)
- UNDER_REVIEW → APPROVED (if all reviews pass)
- UNDER_REVIEW → DECLINED (if any review fails)
- APPROVED → DECLINED (rare: if conditions not met before closing deadline)

INVARIANTS:

State: SUBMITTED
- Application data complete
- Borrower signatures present

State: UNDER_REVIEW
- Assigned to loan officer
- At least one review type in progress

State: APPROVED
- All required reviews complete
- All reviews passed or exceptions approved
- Approval authority documented

State: DECLINED
- Declination reason required
- Declining authority required
- Borrower notification status tracked

Transition: UNDER_REVIEW → APPROVED
- Must have credit approval
- Must have collateral approval
- Must have compliance clearance
- Any exceptions must have proper approval

Transition: Any → DECLINED
- Reason code required
- Cannot transition to DECLINED from APPROVED without executive override
```

## What State Models Don't Capture

It's important to understand what explicit state models typically **don't** capture:

### 1. Activities Within States

State: CREDIT_REVIEW

The model says the application is in credit review. It doesn't capture:
- Which specific analysis the analyst is doing right now
- What documents they're reading
- What they're thinking
- What preliminary conclusions they've reached

States are **coarse-grained**. They represent phases, not moment-to-moment activities.

### 2. Why Decisions Were Made

Transition: UNDER_REVIEW → DECLINED

The model records that the application was declined. It may record a reason code ("Insufficient cash flow"). But it doesn't fully capture:
- The analyst's reasoning process
- What alternatives were considered
- What additional information might have changed the decision
- The confidence level in the decision

States track **what happened**, not the full **why**.

### 3. Informal Communication

During UNDER_REVIEW:
- The loan officer and credit analyst have a hallway conversation
- The analyst mentions concerns informally
- The loan officer reaches out to the borrower for clarification
- The borrower provides additional context

None of this is captured in the state model unless explicitly added as documented events or notes.

### 4. External State

The loan application is UNDER_REVIEW, but:
- What is the borrower's business doing? (Potentially changing)
- What is the market doing? (Affecting collateral values)
- What are competitors offering? (Affecting pricing)

State models capture **internal process state**, not **world state**.

### 5. Uncertainty and Confidence

The model says: State = APPROVED

But reality might be:
- "Approved, assuming the appraisal comes in as expected (90% confident)"
- "Approved, but I have some lingering concerns about industry trends (70% confident)"
- "Approved, this is a slam-dunk case (99% confident)"

Most state models are **binary** (in state or not), while reality is **probabilistic**.

## Design Choices in State Modeling

When creating a state model, many choices must be made:

### 1. How Many States?

**Too few:** "SUBMITTED", "IN PROGRESS", "COMPLETE"
- Simple but not useful for coordination

**Too many:** 50+ states covering every sub-step
- Overwhelming, hard to understand and maintain

**Trade-off:** Enough states to:
- Coordinate between parties
- Track progress meaningfully
- Enforce critical rules
- Not so many that people can't track them

### 2. How Much Should Be Invariant?

**Strict invariants:**
- Enforce everything with rules
- Prevent all invalid states
- Rigid, hard to handle exceptions

**Loose invariants:**
- Guidelines more than rules
- Flexibility for unusual cases
- Risk of invalid states

**Trade-off:** Strict invariants for:
- Compliance requirements
- Safety-critical rules
- Common mistakes to prevent

Loose invariants for:
- Edge cases
- Situations requiring judgment
- Rapidly evolving processes

### 3. Who Can Trigger Transitions?

**Automatic:**
- System triggers transitions based on events
- Consistent, but inflexible

**Manual:**
- Humans trigger transitions
- Flexible, but inconsistent

**Trade-off:** Automatic for:
- Clear triggers (data received, time elapsed)
- High-volume, routine transitions

Manual for:
- Judgment calls
- Complex approvals
- Unusual situations

### 4. What Granularity of History?

**Minimal:** Only current state
- Simple, but lose context

**Full:** Every state change with timestamp, actor, reason
- Complete audit trail, but large data volume

**Trade-off:** More history for:
- Regulated processes
- High-stakes decisions
- Learning from past cases

Less history for:
- Routine, low-risk processes
- High-volume transactions
- Where privacy matters

## From Concepts to Practice

These concepts—states, transitions, invariants—provide a vocabulary for discussing explicit state models. But applying them requires:

1. **Understanding the domain** (healthcare, finance, manufacturing, etc.)
2. **Identifying implicit state** (where state lives in human knowledge)
3. **Choosing appropriate granularity** (not too simple, not too complex)
4. **Balancing enforcement and flexibility** (strict vs loose invariants)
5. **Acknowledging limitations** (what the model can't capture)

The scenarios in [Healthcare](/scenarios/healthcare) and [Finance](/scenarios/finance) show this in practice.

The [Failure Modes](/concepts/failure-modes) page explores what goes wrong when state models are poorly designed or over-applied.

---

**Continue to:** [Uncertainty →](/concepts/uncertainty)

**Explore:** [Healthcare Scenario](/scenarios/healthcare) | [Finance Scenario](/scenarios/finance) | [Failure Modes](/concepts/failure-modes) | [Back to Home](/)