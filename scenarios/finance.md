# Finance Scenario: Commercial Loan Approval

[← Back to Home](../index.md)

## The Scenario: TechStart Manufacturing's Loan Application

TechStart Manufacturing, a 15-person company making specialized components, applies for a $500,000 commercial loan to purchase equipment. The application enters a multi-stage approval process involving credit analysis, collateral review, compliance checks, and pricing.

This scenario examines how state information about the application's progress—which reviews are complete, what exceptions exist, what decisions have been made—is captured (or not captured) in the lending system.

## Day 1 - Application Submitted

### What the System Shows

**Loan Origination System (LOS):**
```
Application ID: CL-2024-3847
Borrower: TechStart Manufacturing LLC
Amount: $500,000
Purpose: Equipment purchase
Status: SUBMITTED
Assigned to: Commercial Lending Queue
Date: March 15, 2024
```

### What the System Doesn't Show

But critical questions have no visible answers:

**About Review Progress:**
- Has anyone looked at the application yet?
- If yes, who? How far did they get?
- Are they waiting for something?

**About Decisions:**
- Has credit reviewed it? What did they think?
- Were any concerns raised?
- Did someone make a preliminary approval/denial decision?

**About Exceptions:**
- Does anything fall outside standard guidelines?
- If yes, has an exception been requested?
- Who needs to approve the exception?

**About Next Steps:**
- What should happen next?
- Who is responsible for making it happen?
- Is anything blocking progress?

### Where This State Lives

This state is fragmented:

**In credit analyst Sarah's email:**
> "FYI, reviewed TechStart Manufacturing. Cash flow is decent but debt service coverage is 1.15x, just below our 1.25x guideline. Could work with personal guarantee. Sending to collateral review." (March 16, 2:30 PM)

**In collateral analyst Mike's notes** (paper notepad):
> "TechStart - equipment appraisal came back at $420K, they want to borrow $500K. LTV is 119%. Need to either reduce loan amount or get additional collateral."

**In compliance officer Janet's memory:**
> "I remember seeing TechStart's application. There was something about the ownership structure that looked complicated. I should follow up on that, but I've got five other things marked urgent."

**In loan officer Tom's assumption:**
> "I submitted it days ago. It's probably working through credit. I'll check in next week if I don't hear anything."

### Day 8 - The Borrower Calls

**TechStart CFO**: "We submitted our loan application over a week ago. What's the status? We need to close on the equipment purchase by end of month."

**Tom** (loan officer): "Let me check... The system shows it's still 'in review.' Let me track down where exactly it is."

**Tom spends 45 minutes** calling Sarah (out today), Mike (in a meeting), and Janet (who doesn't remember the details). Finally pieces together:
- Credit approved with condition (personal guarantee)
- Collateral has issue (high LTV)
- Compliance hasn't completed ownership review
- No one documented these findings in the system

**Tom to CFO**: "We're still reviewing. I'll get back to you in a couple days."

**Result**: Borrower frustrated. Loan officer wastes time playing detective. Deal at risk of falling through.

## The Implicit State Problem: Detailed Examination

### Ambiguity About Progress

The system status "SUBMITTED" or "IN REVIEW" reveals almost nothing:

- **Which reviews have occurred?** Unknown without asking people
- **What were the findings?** Scattered in emails, notes, memory
- **Are we stuck?** No way to tell
- **Who should act next?** Unclear

This ambiguity means:
- Loan officer can't answer borrower questions
- Management can't identify bottlenecks
- Reviews might be duplicated or missed
- Timeline predictions are guesses

### Fragmented Decision Context

When decisions are made, their context is lost:

**Sarah's credit decision** "Could work with personal guarantee" is:
- In an email, easily missed
- Not formally recorded as a condition
- Not visible to downstream reviewers
- May be forgotten or misremembered

**Mike's collateral concern** about LTV is:
- On paper, might be lost
- Not flagged as a blocker in the system
- Not visible to the loan officer or borrower
- Not tracked toward resolution

### Invisible Exceptions

The application has two exceptions to guidelines:
1. Debt service coverage below minimum (1.15x vs 1.25x)
2. Loan-to-value above maximum (119% vs 80%)

But these exceptions are:
- Not formally documented
- Not assigned to anyone for resolution
- Not tracked with approval status
- Not visible to management for oversight

In a regulated environment, this creates compliance risk. Examiners ask: "How do you ensure exceptions are properly reviewed and approved?"

Answer: "We rely on our experienced staff to catch them."

Not a satisfying answer.

### Coordination Failures

Multiple parties need to coordinate:
- Credit review completion should trigger collateral review
- Collateral issues should go back to loan officer to discuss with borrower
- Compliance clearance is required before pricing
- Pricing needs final approval before closing

But without explicit handoff states:
- Things sit waiting for someone to notice them
- Responsibilities are unclear
- Work happens in random order
- Some steps are forgotten

## Attempting Explicit State: The Loan Application State Model

The bank implements an explicit state model for commercial loan applications.

### The Model

```
Loan Application States:
1. SUBMITTED
2. CREDIT_REVIEW
3. COLLATERAL_REVIEW
4. COMPLIANCE_REVIEW
5. PENDING_CONDITIONS
6. PRICING
7. FINAL_APPROVAL
8. CLOSING
9. FUNDED
10. DECLINED (terminal state)

Each state tracks:
- Who is responsible
- When state was entered
- What must be completed to advance
- Any blockers or exceptions

Transitions:
- SUBMITTED → CREDIT_REVIEW (automatic on assignment)
- CREDIT_REVIEW → COLLATERAL_REVIEW (on credit approval)
- CREDIT_REVIEW → PENDING_CONDITIONS (if conditions required)
- CREDIT_REVIEW → DECLINED (if credit rejected)
- (similar rules for other transitions)

Exceptions Registry:
- Exception type
- Guideline requirement
- Actual value
- Justification
- Approver required
- Status (Pending/Approved/Declined)
```

### Implementation

The loan system now enforces state transitions and captures exception approvals.

## Day 1 - Application Submitted (With Explicit State)

### What the System Shows

**Loan Application Dashboard:**
```
Application: CL-2024-3847 - TechStart Manufacturing LLC
Amount: $500,000
Current State: CREDIT_REVIEW (entered March 16, 08:00)
Assigned to: Sarah Chen, Credit Analyst
Expected completion: March 18

Status History:
- SUBMITTED (March 15, 14:22) → CREDIT_REVIEW (March 16, 08:00)

Active Tasks:
✓ Financial analysis (Sarah Chen, March 16, 14:15)
✓ Cash flow review (Sarah Chen, March 16, 14:15)
⏳ Credit decision (Sarah Chen, in progress)
```

### Day 2 - Credit Review Complete

**Sarah completes review and updates system:**

```
Current State: PENDING_CONDITIONS (entered March 16, 14:30)
Assigned to: Tom Rodriguez, Loan Officer

Credit Decision: APPROVED WITH CONDITIONS
Conditions:
1. Personal guarantee from primary owner (John Smith)
2. Debt service coverage exception approval required

Exceptions Registered:
Exception #1:
- Type: Debt Service Coverage Ratio
- Guideline: Minimum 1.25x
- Actual: 1.15x
- Justification: Strong cash flow trend (+15% YoY), owner has significant personal liquidity
- Approver Required: Credit Manager (Level 2)
- Status: PENDING APPROVAL

Next: Loan officer must obtain personal guarantee commitment and present exception to credit manager.
```

**System automatically:**
- Notifies Tom (loan officer) of conditions
- Notifies Credit Manager of exception requiring approval
- Updates dashboard for management visibility
- Adds task to Tom's queue

## Day 3 - Multi-Party Coordination

**Tom contacts borrower:**

**Tom**: "Good news, credit has approved your application with one condition: we'll need a personal guarantee from the primary owner. Can you provide that?"

**TechStart CFO**: "Yes, John expected that. We can have that signed this week."

**Tom**: "Great. I'll also present the debt service coverage exception to our credit manager for approval, and then we'll move to collateral review."

**Tom updates system:**
```
Condition #1 Status: IN PROGRESS
- Personal guarantee commitment received (verbal, March 17)
- Documents requested from borrower
- Expected completion: March 19

Exception #1: Submitted to Credit Manager Linda Foster (March 17, 10:30)
```

### Meanwhile, Collateral Review Begins

Because state transitions are explicit, the system knows:
- Credit review is complete (with conditions)
- Collateral review can start in parallel
- Final approval requires both credit conditions AND collateral clearance

```
Current State: COLLATERAL_REVIEW (parallel track, March 17, 11:00)
Assigned to: Mike Thompson, Collateral Analyst

Note: Application is in both PENDING_CONDITIONS and COLLATERAL_REVIEW states
- PENDING_CONDITIONS: Awaiting personal guarantee docs + exception approval
- COLLATERAL_REVIEW: In progress

Both must complete before advancing to PRICING.
```

## Day 4 - Collateral Issue Discovered

**Mike completes appraisal review:**

```
Collateral Analysis Complete (March 18, 15:20)

Finding: ISSUE IDENTIFIED
- Equipment appraised value: $420,000
- Requested loan amount: $500,000
- Loan-to-Value: 119%
- Guideline maximum: 80% LTV

Exception Required:
Exception #2:
- Type: Loan-to-Value Ratio
- Guideline: Maximum 80% LTV
- Actual: 119% LTV
- Recommendation: Reduce loan to $336,000 OR obtain additional collateral
- Approver Required: Credit Committee
- Status: PENDING LOAN OFFICER ACTION

Application State: PENDING_CONDITIONS (updated March 18, 15:20)
Assigned to: Tom Rodriguez

Blocker: LTV exception must be resolved before proceeding.
```

**System automatically:**
- Alerts Tom of collateral issue
- Updates dashboard to show blocker
- Notifies Credit Committee of potential exception
- Recalculates timeline (closing delayed pending resolution)

## Day 5 - The Borrower Calls (Different Outcome)

**TechStart CFO**: "What's the status?"

**Tom** (checking dashboard in real-time): "I can give you a detailed update. Credit has approved with a personal guarantee condition, which you're providing. We have one issue: the equipment appraisal came in at $420K, which puts the loan-to-value at 119%, above our 80% guideline. We have two options: reduce the loan amount to about $336K, or you could provide additional collateral to secure the full $500K."

**CFO**: "We have accounts receivable we could pledge. Would that work?"

**Tom**: "Potentially yes. Let me have our collateral team evaluate that. I'll get back to you by tomorrow with the details."

**Tom updates system:**
```
Exception #2: RESOLUTION IN PROGRESS
- Borrower offering AR as additional collateral
- Requested AR aging report and customer concentration analysis
- Expected completion: March 20
```

**Result**: Borrower has clear information. Issue is being actively resolved. Timeline is transparent.

## Day 10 - Application Status

### Explicit State Dashboard View

```
Application: CL-2024-3847 - TechStart Manufacturing LLC
Current State: PRICING (entered March 22, 09:00)
Assigned to: Tom Rodriguez, Loan Officer

Progress:
✓ CREDIT_REVIEW (Complete March 16)
  - Approved with conditions
✓ PENDING_CONDITIONS (Complete March 21)
  - Personal guarantee executed (March 19)
  - Exception #1 approved by Credit Manager (March 18)
✓ COLLATERAL_REVIEW (Complete March 21)
  - Equipment: $420K value
  - AR pledge: $200K value
  - Total collateral: $620K
  - LTV: 81% (within guidelines with AR)
  - Exception #2 approved by Credit Committee (March 21)
✓ COMPLIANCE_REVIEW (Complete March 20)
  - Ownership structure verified
  - BSA/AML clearance obtained
⏳ PRICING (In progress)
  - Risk rating: B+ (calculated based on credit + collateral)
  - Rate calculation: In progress
  - Expected completion: March 23

Next: Final approval required from Regional VP (loans >$250K)
Timeline: On track for March 28 closing
```

**What's visible:**
- Complete history of decisions
- All exceptions with approvals
- Current responsibility and next steps
- Realistic timeline based on actual progress

## What Changed? Analysis

### Benefits of Explicit State

**1. Visibility and Accountability**

Every stakeholder can see:
- Where the application is in the process
- Who is responsible for current state
- What has been decided and by whom
- What exceptions exist and their status

Management can identify:
- Bottlenecks (applications stuck in certain states)
- Workload distribution (how many applications per analyst)
- Exception patterns (frequent guideline violations)

**2. Compliance and Auditability**

Regulators ask: "How do you ensure exceptions are properly approved?"

Answer now: "Every exception is registered in the system with justification, required approver, and documented approval. We can produce a report of all exceptions by type, approver, and outcome."

The state model creates an **audit trail** that implicit state cannot provide.

**3. Predictable Coordination**

State transitions trigger actions:
- Credit completion notifies collateral team
- Exception registration notifies required approvers
- Blockers alert loan officer and management
- Timeline updates reflect actual constraints

Work flows systematically rather than randomly.

**4. Better Borrower Communication**

Loan officer can provide:
- Specific status (not vague "in review")
- Clear explanation of any issues
- Realistic timeline based on actual state
- Proactive updates when state changes

This builds trust and reduces borrower anxiety.

### Failure Modes and Limitations

But explicit state in lending has problems too:

**1. Process Rigidity**

The model defines linear stages: CREDIT → COLLATERAL → COMPLIANCE → PRICING

Reality is messier:
- Sometimes collateral review reveals something that changes credit decision
- Sometimes compliance finds ownership issues that affect collateral
- Sometimes pricing considerations feed back into structure

**Forcing complex reality into linear stages** can:
- Delay decisions ("We can't price until compliance completes" even if it's a minor issue)
- Miss opportunities ("Credit would approve at a different structure" but we don't discover that until later)
- Create artificial handoffs (passing back and forth between states)

**2. Exception Theater**

Once exceptions require formal approval, incentives change:

**Before explicit state:**
- Analyst: "This is 1.15x coverage instead of 1.25x, but it's close enough given the strong trends. Approved."

**After explicit state:**
- Analyst: "This triggers an exception. I need to write a justification, route it to my manager, wait for approval. That's extra work. Maybe I can present the numbers differently to avoid triggering the exception..."

**The model can encourage:**
- Gaming the classification to avoid exception routing
- Over-lengthy justifications (covering all bases)
- Conservative decisions (avoid exceptions = avoid work)
- Shadow approvals (verbal OK before formal submission)

**3. False Precision About Progress**

Marking an application "CREDIT_REVIEW COMPLETE" suggests clarity:
- Credit decision is final
- No further credit questions
- Safe to proceed to next stage

But reality:
- Credit decision might be "approved subject to seeing the appraisal"
- Analyst might have lingering concerns they didn't formalize
- New information could change the credit view

**The state model implies more certainty than exists.**

**4. State Explosion**

As exceptions and variations emerge, the model grows:

```
Original states: 10
After 6 months: 
- Added CREDIT_REVIEW_ADDITIONAL_INFO (for information requests)
- Added COLLATERAL_REVALUATION (for disputed appraisals)
- Added PENDING_CONDITIONS_PARTIAL (some conditions met, some pending)
- Added PRICING_EXCEPTION (for rate exceptions)
- Added FINAL_APPROVAL_ESCALATED (for special cases)

Now: 20 states, complex transition rules, staff confused about which state to use
```

**The model becomes complicated,** losing the simplicity that made it useful.

**5. Organizational Boundaries**

TechStart's loan requires:
- External appraisal company (provides equipment value)
- Personal guarantee (borrower must execute)
- Insurance quote (for equipment coverage)
- Legal review (for AR security agreement)

The bank can model its internal state, but:
- The appraiser has their own process state
- The borrower's execution of documents is outside bank control
- Legal review happens in another system

**State at organizational boundaries remains implicit** no matter how explicit internal state becomes.

**6. Pressure to Conform**

Experienced loan officers develop judgment:

**Before**: "This application is unusual. The borrower's business model is innovative, and our standard analysis doesn't quite fit. Let me spend extra time understanding it and present a custom approach."

**After**: "The system is asking me to complete fields that don't apply. The state model doesn't have a category for this business type. I could request a special exception process, but that's a lot of overhead. Maybe I'll just make it fit the standard model."

**The explicit state model can discourage handling of unique situations** that don't fit its categories.

**7. False Reporting and Approval Gaming**

When loan processing time and approval rates become performance metrics, staff may manipulate state indicators to meet targets rather than reflect actual risk assessment.

**The Scenario:**

Bank executives implement new metrics: "Average time in CREDIT_REVIEW" and "Approval rate by loan officer." Performance bonuses and promotion decisions are tied to these metrics. Loan officers are expected to maintain "high throughput" and "reasonable approval rates."

**The Pressure:**

A loan officer, Sarah, is reviewing TechStart's application:
- She has doubts about cash flow projections
- Industry research shows the sector is volatile
- The company's account receivables aging report shows concerning patterns
- Her gut says "this needs deeper investigation"

But Sarah also knows:
- She already spent 3 hours on this application (above average)
- If she marks "ADDITIONAL_INFO_REQUIRED," it extends her review time metric
- Her approval rate is already lower than peers (being cautious)
- Last performance review noted she was "slower than average"
- The applicant is a referral from a senior relationship manager

**What the System Shows:**
```
Status: CREDIT_REVIEW → APPROVED
Credit Decision: PASS
Approval Date: March 16, 2024, 2:47 PM
Time in Review: 2.8 hours (within target)
```

**What Actually Happened:**

Sarah thought: "This is borderline. I have concerns, but they're not strong enough to reject. If I request more information, I'll miss my throughput target. The company has been operating for 5 years, so maybe I'm being too conservative. I'll approve it and add a note about the concerns - if something goes wrong, I can point to my note."

She marks it APPROVED and adds a note buried in the file: "Monitor AR aging closely, sector headwinds noted."

**The Consequences:**

- Loan appears thoroughly reviewed (all checklists complete, reasonable time spent)
- Risk is hidden in unstructured notes that may never be read
- Downstream processes (collateral review, pricing) assume credit decision is solid
- If the loan defaults, the question will be "Why did Sarah approve this?" and she'll say "I documented my concerns"
- Pattern repeats across the bank: loan officers gaming metrics leads to systemic risk accumulation

**Similar Gaming Patterns in Lending:**

1. **Rushed Approvals**: Mark complete without thorough analysis to hit time targets
2. **Selective Information Gathering**: Don't request additional info that might reveal problems
3. **Classification Gaming**: Categorize higher-risk loans in lower-risk buckets
4. **Condition Waiving**: Mark "PENDING_CONDITIONS" as resolved without full verification
5. **Exception Minimization**: Avoid triggering exception workflows that slow things down
6. **Documentation Theater**: Check all boxes but with minimal actual review

### Possible Guardrails for Lending Context

**1. Separate Process Tracking from Performance Evaluation**

- Use state model for coordination and transparency, not individual scorecarding
- Measure portfolio outcomes (default rates, profitability) over time, not approval speed
- Reward thorough risk assessment, not fast throughput
- Make it safe to slow down for complex cases

**2. Confidence and Risk Scoring**

Instead of binary APPROVED/DECLINED:
```
❌ Bad:  Status: APPROVED
✓ Good: Status: APPROVED (Risk Score: 6.5/10, Confidence: Medium, 3 concerns noted)
```

Allow loan officers to express:
- Their confidence level in the decision
- Specific concerns that didn't rise to rejection level
- Conditions they'd want to monitor closely
- Whether they'd want second review on similar cases

**3. Mandatory Justification for Borderline Cases**

For applications in certain ranges:
- Require explicit reasoning for approval
- Mandate second-reviewer sign-off
- Force structured risk acknowledgment
- Cannot mark APPROVED without completing risk narrative

**4. Track Leading Indicators of Gaming**

Monitor patterns that suggest gaming:
- Loan officers whose approval times are consistently at exactly the target
- Officers whose approval rates significantly exceed peers in same portfolio
- Rapid status transitions (all reviews completed in minimum time)
- Low rates of "ADDITIONAL_INFO_REQUIRED" (nobody ever needs more info?)
- Pattern of approvals near end of bonus measurement periods

**5. Outcome-Based Validation**

- Track which loans default and trace back to approval process
- Correlate: faster approvals with higher default rates?
- Analyze: did officers who spent more time have better outcomes?
- Feedback loop: "Applications you approved in under 2 hours had 8% default rate vs. 3% for longer reviews"
- Make outcomes visible to reveal cost of gaming

**6. Peer Review and Sampling**

- Randomly sample "approved" applications for independent review
- Peer review: "Would you have approved this given the information?"
- Identify cases where approval seems questionable
- Discuss patterns in team meetings without blame
- Use to calibrate what "thorough review" means

**7. Protect Prudent Slowness**

- Explicitly reward loan officers who identify high-risk applications early
- Positive recognition for "you caught something others might have missed"
- Make it safe to take extra time on complex cases
- Override throughput metrics for legitimately complex applications
- Narrative field: "Why did this take longer?" with valid answers protected

**8. Structured Risk Escalation**

For borderline cases, provide safe escalation path:
- "I need senior review" option that doesn't count against metrics
- Committee review for applications meeting certain criteria
- Shared responsibility for difficult decisions
- Make escalation normal, not a sign of failure

**9. Industry Benchmark Reality Checks**

- Compare internal approval rates to industry for similar portfolios
- If bank's approval rate is significantly higher: investigation needed
- If specific officers are outliers: examine their decisions
- Cross-bank data (when available) to identify unusual patterns

**10. Regulatory Oversight and Compliance Checks**

- Internal audit samples loan approvals against documentation
- Compliance reviews state transitions for reasonableness
- Regulatory exams look at loan quality vs. approval processes
- Public scrutiny (for public companies) on loan quality metrics

### The Finance-Specific Challenge

Banking faces unique pressures:
- **Competitive pressure**: "Competitors approve faster, we're losing deals"
- **Relationship dynamics**: "This borrower brings $5M in deposits, don't lose them"
- **Volume targets**: "We need to grow the loan portfolio 15% this year"
- **Regulatory requirements**: Must document and justify decisions thoroughly
- **Career implications**: Slow/conservative officers seen as "not business-minded"

These pressures exist regardless of state models. Making state explicit can either:
- **Help**: Reveal when gaming is happening, create accountability
- **Hurt**: Provide specific metrics to game, make false reporting more systematic

**The difference depends on how leadership responds to the data.**

If leadership sees:
- "Officer X takes 5 hours per review vs. 3 hour average" → Pressure to speed up → Gaming
- "Officer X's loans have 2% default rate vs. 5% average" → Reward thoroughness → Quality

### The Fundamental Trade-off in Finance

Unlike healthcare (where patient safety is paramount), banking involves explicit risk-taking for profit:
- Some loans will default (expected)
- Must balance thoroughness with volume
- Speed can be competitive advantage
- Being "too conservative" has costs (missed opportunities)

This makes guardrails harder:
- Some "gaming" might be appropriate business judgment ("good enough" analysis)
- Distinguishing "efficient review" from "reckless approval" requires judgment
- What looks like gaming might be expertise (experienced officer knows what to focus on)

The guardrails must preserve space for judgment while preventing systemic risk accumulation from perverse incentives.

## Deeper Questions

### When Should Lending State Be Explicit?

**High value:**
- Multi-party coordination (credit, collateral, compliance, pricing)
- Regulatory requirements (audit trail, exception tracking)
- Management oversight (portfolio risk, processing metrics)
- Complex applications (many conditions, multiple exceptions)

**Lower value:**
- Simple, routine applications (fit standard guidelines, no exceptions)
- Expedited processing (explicit state adds overhead)
- Applications requiring creative structuring (model constrains options)

### What Granularity of State?

**Too coarse:**
- "IN REVIEW" (which review? how far along?)
- Not useful for coordination

**Too fine:**
- "CREDIT_REVIEW_FINANCIAL_ANALYSIS_INCOME_STATEMENT_ANALYSIS_IN_PROGRESS"
- Nobody can track this many states

**The right level** depends on:
- Who needs to coordinate based on state?
- What decisions depend on state?
- What must be auditable?

### How to Model Uncertainty?

The scenario showed:
- "⏳ In Progress" (started but not complete)
- "⚠ Issue Identified" (problem discovered)

Could we go further?
- Confidence levels on decisions ("75% likely to approve")
- Soft vs hard blockers ("Should resolve but not required" vs "Must resolve")
- Provisional approvals ("Approved assuming appraisal comes in as expected")

This would capture reality better—but increase complexity.

### State as Communication vs Control

Is explicit state primarily for:
- **Communication** (letting everyone know status)?
- **Control** (enforcing process compliance)?

In practice, it's both—but tension exists:
- Communication benefits from simplicity
- Control benefits from precision
- Optimizing for one can hurt the other

## Implications

This scenario demonstrates:

1. **Implicit state in lending creates real problems** - coordination failures, compliance risk, borrower frustration
2. **Explicit state provides significant benefits** - visibility, auditability, systematic workflow
3. **Explicit state creates new problems** - rigidity, gaming, complexity growth, overhead
4. **State models shape behavior** - people adapt to the model, sometimes in unintended ways
5. **Some state remains implicit** - especially at organizational boundaries and in unique situations

The question is not "should we make state explicit?"

The question is: **For which aspects of the lending process, at what granularity, with what flexibility, does explicit state modeling improve outcomes more than it constrains them?**

And the answer depends on the specific institution, its processes, its regulatory environment, and its culture.

---

**Continue to:** [Core Concepts →](../concepts/core.md)

**Explore:** [Healthcare Scenario](healthcare.md) | [Failure Modes](../concepts/failure-modes.md) | [Back to Home](../index.md)