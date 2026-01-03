# Healthcare Scenario: Patient Discharge Coordination

[← Back to Home](../index.md)

## The Scenario: Mrs. Chen's Discharge

Mrs. Chen, 78, has been hospitalized for five days following a fall that resulted in a hip fracture. Surgery was successful. She is medically stable and ready for discharge. But "ready for discharge" is where things get complicated.

## Day 5, 10:00 AM - The Implicit State Problem

### What the System Shows

The electronic health record (EHR) displays:
- **Status**: "Medically cleared for discharge"
- **Last note**: "Patient stable, pain controlled, PT cleared for home"
- **Discharge order**: Pending

### What the System Doesn't Show

But multiple critical questions have no visible answers:

**About Home Care:**
- Has someone confirmed a family member will be home the first 48 hours?
- Has home health been arranged? Are they notified of discharge timing?
- Has the home been assessed for fall risks?

**About Medications:**
- Has the pharmacy confirmed they can fill the new prescriptions today?
- Does Mrs. Chen understand the new medication schedule?
- Has someone checked for drug interaction with her existing medications at home?

**About Follow-up:**
- Has the orthopedic follow-up appointment been scheduled?
- Does she have transportation to that appointment?
- Has her primary care physician been notified?

**About Equipment:**
- Has durable medical equipment (walker, raised toilet seat) been ordered?
- Has it been delivered to her home?
- Has someone confirmed insurance will cover it?

### Where This State Lives

This state is fragmented across:
- **The nurse's memory**: "I think the daughter said she'd be there, but I should double-check"
- **Voicemail**: A message from the home health agency, not yet returned
- **An email**: From the equipment company, sent to the wrong department
- **A sticky note**: On the case manager's desk, about the pharmacy issue
- **Assumption**: "Someone must have scheduled the follow-up by now"

### What Happens: The 2:00 PM Conversation

**Attending physician**: "Why hasn't Mrs. Chen been discharged? I cleared her this morning."

**Nurse**: "We're waiting on... I think it's the home health? Or maybe the equipment? Let me check."

**Case manager** (15 minutes later): "The home health is set up, but they need 24-hour notice. Equipment was ordered but sent to her old address. The daughter can't be there until tomorrow afternoon. And I just found out the pharmacy is out of one of her medications."

**Result**: Discharge delayed. Mrs. Chen spends another night in the hospital (cost: ~$2,000). Coordination calls take 2 hours of staff time. Risk increases (hospital-acquired infections, fall risk).

## The Implicit State Problem: Detailed Examination

### Ambiguity

At any moment, the answer to "Can Mrs. Chen be discharged now?" requires:
1. Asking multiple people
2. Checking multiple systems
3. Making phone calls to external parties
4. Piecing together information from memory and notes

Different people have different views:
- The physician thinks: "She's medically ready, discharge her"
- The nurse thinks: "I'm not sure everything is arranged"
- The case manager thinks: "I'm still working on three things, but I haven't told anyone they're blockers"

### Fragmentation

Critical state information exists in:
- **EHR**: Medical status only
- **Case management system**: Some care coordination notes
- **Pharmacy system**: Medication availability (not checked until someone calls)
- **Home health system**: Their scheduling (not visible to hospital)
- **Email**: Equipment company communications
- **Phone logs**: Attempted calls to family
- **Staff memory**: Verbal commitments and concerns

No single person or system has the complete picture.

### Implicit Responsibility

Who is responsible for confirming each element? It's unclear:
- "The nurse usually checks on family availability"
- "Case management typically arranges home health"
- "Someone should probably call the pharmacy, but who?"

This ambiguity leads to:
- Duplicated effort (two people calling the same place)
- Missed steps (nobody calls the pharmacy)
- Blame shifting when things go wrong

### Temporal Uncertainty

When should things happen?
- Should home health be arranged when discharge is first discussed, or when it's formally ordered?
- When should the pharmacy be contacted—today or when discharge is imminent?
- How much advance notice does equipment delivery need?

These timing dependencies are known implicitly ("experienced nurses know to call early") but not captured in any system.

## Attempting Explicit State: The Discharge Readiness Model

A hospital attempts to address this by introducing an **explicit discharge readiness state model**.

### The Model

```
Discharge Readiness States:
1. MEDICALLY_CLEARING
2. COORDINATION_PLANNING
3. AWAITING_CONFIRMATIONS
4. READY_TO_DISCHARGE
5. DISCHARGED

Required elements for READY_TO_DISCHARGE:
- Medical clearance: Yes/No
- Home care plan: [None | Family | Home Health | Facility]
  - If Home Health: Arranged + Notified
- Medications: Reconciled + Available + Education Complete
- Follow-up: PCP Notified + Specialist Scheduled + Transportation Confirmed
- Equipment: Identified + Ordered + Delivery Confirmed
- Family: Contacted + Available + Education Complete
```

### Implementation

The case manager now maintains a structured checklist in the EHR. Each item must be explicitly marked complete with timestamp and responsible person.

## Day 5, 10:00 AM - With Explicit State

### What the System Shows

**Discharge Readiness Dashboard:**
```
Patient: Chen, Margaret
Status: COORDINATION_PLANNING → AWAITING_CONFIRMATIONS (automatically updated 09:45)

✓ Medical clearance (Dr. Martinez, 09:30)
✓ Home care plan: Home Health (Case Mgr. Johnson, 08:15)
  ✓ Agency contacted: ComfortCare (Johnson, 08:15)
  ⏳ Agency confirmed ready: PENDING (awaiting callback)
✓ Medications reconciled (Pharmacist Chen, 09:00)
  ⏳ Pharmacy availability: CHECKING (called 09:30, waiting)
  ⏳ Patient education: SCHEDULED (nurse visit at 11:00)
✗ Follow-up appointments
  ✓ PCP notified (automated, 08:30)
  ⏳ Ortho appointment: SCHEDULING (waiting for scheduler callback)
  ✗ Transportation: NOT STARTED
⏳ Equipment
  ✓ Items identified: Walker, raised toilet seat (PT, 09:15)
  ✓ Ordered: MedEquip Co. (Johnson, 09:20)
  ⚠ Delivery confirmation: ISSUE - wrong address on file
✓ Family contacted (Nurse Park, 08:00)
  ⚠ Availability: Daughter available tomorrow PM, not today

EARLIEST DISCHARGE: Tomorrow 2:00 PM
BLOCKERS: 
- Equipment delivery to correct address
- Transportation arrangement
- Home health 24hr notice requirement
```

### What This Reveals

The explicit state model makes visible:
1. **Seven** distinct elements must be confirmed
2. **Three** are blocking discharge
3. **One** has an active problem (wrong address)
4. **Earliest realistic discharge time** is tomorrow afternoon, not today

This information was always true—but now it's visible and actionable.

### The 2:00 PM Conversation (Different)

**Attending physician**: "Why hasn't Mrs. Chen been discharged?"

**Nurse** (checking dashboard): "The discharge board shows we're blocked on three items: equipment being sent to her old address, her daughter can't be there until tomorrow afternoon, and home health needs 24-hour notice. Earliest discharge is tomorrow 2 PM."

**Physician**: "Can we do anything about the equipment address?"

**Case manager** (already notified by the system): "I corrected it an hour ago. They're delivering tomorrow morning."

**Result**: Clear understanding. No wasted coordination time. Realistic expectations set. Mrs. Chen and her family are informed.

## What Changed? Analysis

### Benefits of Explicit State

**1. Visibility**
- All stakeholders see the same complete picture
- No need to ask multiple people or piece together information
- Blockers are obvious, not hidden

**2. Accountability**
- Each element has a responsible person and timestamp
- No ambiguity about who should handle what
- Easier to identify where processes break down

**3. Realistic Timing**
- System calculates earliest discharge based on actual constraints
- Expectations are managed proactively
- Downstream systems (home health, pharmacy) get appropriate notice

**4. Reduced Coordination Load**
- Fewer "status check" conversations
- Automated notifications to relevant parties
- Staff can focus on solving problems, not discovering them

### Failure Modes and Limitations

But explicit state models are not perfect. Here's what they cannot solve:

**1. Reality is Messier Than Models**

The model says: "Home care plan: [None | Family | Home Health | Facility]"

Reality: Mrs. Chen's daughter can be there part-time. A neighbor (who is a retired nurse) offered to check in daily. The home health agency can come three times a week but not daily. Which category is this?

**The model forces a choice** that doesn't match reality. Staff must either:
- Pick the "closest" option (losing nuance)
- Add a workaround note (defeating the purpose)
- Expand the model (increasing complexity)

**2. False Precision**

Marking something "✓ Complete" suggests certainty that may not exist:

- "✓ Patient education complete" - But did Mrs. Chen really understand? Will she remember tomorrow?
- "✓ Family available" - The daughter said "probably", but what if her work situation changes?
- "✓ Home assessed for fall risks" - Someone called and asked questions, but no one visited the home

**The explicit state provides confidence that may be unwarranted.** Implicit state at least acknowledged uncertainty ("I think the daughter will be there").

**3. Coordination Overhead**

Someone must now:
- Update the dashboard continuously
- Train all staff on the new system
- Handle exceptions ("What if the pharmacy says maybe?")
- Deal with incomplete data ("I called the equipment company but haven't heard back")

This is **new work** that didn't exist before. If staff are already overwhelmed, adding explicit state tracking may make things worse, not better.

**4. Gaming and Shortcuts**

When explicit state becomes a metric:
- Staff check boxes to show progress, even when uncertain
- "✓ Pharmacy availability" gets marked based on a quick call, not confirmed availability
- The model becomes a performance theater rather than reality capture

**5. Rigidity**

Experienced nurses develop judgment: "Mrs. Chen seems anxious about going home. Even though everything checks out, maybe we should wait."

An explicit model might push for discharge: "All items complete → discharge ready."

**The model doesn't capture human intuition** about factors it can't measure.

**6. False Reporting and Status Manipulation**

When discharge readiness becomes a measured performance metric, staff may manipulate the system to meet targets rather than accurately represent reality.

**The Scenario:**

Hospital administration implements a new metric: "Percentage of medically cleared patients discharged within 24 hours." Department performance reviews and bonuses are tied to this metric.

**The Pressure:**

- Case managers feel pressure to mark items as complete even when confirmation is pending
- Nurses check boxes based on assumptions rather than verified facts
- "✓ Family available" gets marked when daughter says "I'll try to be there" (not a firm commitment)
- "✓ Medication education complete" gets checked after a rushed 5-minute conversation
- Status updates become optimistic rather than realistic to avoid appearing as a bottleneck

**The Reality:**

```
What the Dashboard Shows:
✓ All items complete
Status: READY_TO_DISCHARGE
Target: Within 24-hour window

What Actually Happened:
⚠ Home health called but didn't confirm - assumed they'll be ready
⚠ Equipment delivery "should arrive" but no tracking number provided
⚠ Family "probably" available but hasn't confirmed specific time
⚠ Patient seemed confused during medication review but time was limited
```

**The Consequences:**

- Dashboard shows false readiness while real blockers remain hidden
- Patients discharged prematurely, leading to:
  - Higher readmission rates (patient returns within 72 hours)
  - Safety incidents (falls, medication errors at home)
  - Family complaints (unprepared for care responsibilities)
- Loss of trust in the system ("the dashboard is useless, we can't believe it")
- Blame shifting when problems occur ("but you marked it complete!")

**Why This Happens:**

When explicit state becomes a performance metric rather than a coordination tool:
- Incentives misalign (look good vs. be accurate)
- Fear of negative consequences (bad performance reviews)
- Time pressure (easier to check the box than verify thoroughly)
- Normalization of shortcuts ("everyone does it")
- Target fixation (hit the metric, ignore the underlying goal)

This is a specific instance of **Goodhart's Law**: "When a measure becomes a target, it ceases to be a good measure."

### Possible Guardrails to Prevent False Reporting

While no system can completely prevent gaming, certain design choices and organizational practices can reduce false reporting:

**1. Separate Tracking from Performance Measurement**

- Use the state model for coordination, not performance evaluation
- Don't tie individual performance reviews or bonuses to state completion rates
- Measure outcomes (readmission rates, patient satisfaction) not process compliance
- Make clear: the dashboard is a tool for coordination, not a scorecard

**2. Require Evidence for Critical Items**

Instead of binary checkboxes:
```
❌ Bad:  ✓ Home health arranged
✓ Good: Home health arranged: ComfortCare confirmed, Start date: 1/15, Contact: Jane Smith 555-0123
```

For high-risk items, require:
- Specific confirmation details (who, when, confirmation number)
- Timestamp of verification
- Evidence reference (confirmation email, phone log)
- This makes false reporting harder and more detectable

**3. Build in Uncertainty States**

Allow staff to represent partial or uncertain information:
```
Status Options:
✓ CONFIRMED (verified with evidence)
⏳ PENDING (initiated but awaiting confirmation)
~ LIKELY (based on reasonable expectation but not confirmed)
⚠ CONCERN (potential issue identified)
✗ NOT STARTED
```

This reduces pressure to mark things "complete" when they're actually "probably okay."

**4. Audit and Spot-Check Reality**

- Randomly audit cases: does dashboard match reality?
- Call home health agencies to verify what was marked "confirmed"
- Interview patients post-discharge: was the preparation adequate?
- Track correlation between dashboard completion and actual outcomes
- Make results visible: "Dashboard accuracy rate: 73%" signals the problem

**5. Safe Reporting of Blockers**

- Make it safe to report problems without blame
- Recognize staff who identify issues early (even if it delays discharge)
- Track "blockers surfaced" as a positive metric (early problem detection)
- Avoid "why wasn't this discharged?" interrogations that punish honesty

**6. Workflow Design to Reduce Pressure**

- Start discharge planning earlier (when first medically cleared, not when bed needed)
- Allow realistic timeframes (not every discharge can be same-day)
- Staff adequate resources (if case managers are overwhelmed, shortcuts happen)
- Provide tools to automate verification (system checks pharmacy inventory, not phone calls)

**7. Real-Time Validation Where Possible**

- Integrate with external systems (pharmacy inventory, home health scheduling)
- Automated confirmation (home health agency's system confirms directly)
- Family confirmation via patient portal (not just verbal commitment)
- Equipment delivery tracking (real-time status from vendor system)
- Reduce reliance on staff manually marking things "complete"

**8. Cultural and Training Approaches**

- Train on the "why": dashboard helps coordinate care, not judge performance
- Share stories of when false reporting led to patient harm
- Encourage professional judgment: "if you're not confident, don't check it"
- Leadership modeling: administrators acknowledging when systems fail, not blaming staff
- Regular discussion of ethical use of state tracking tools

**9. Design for Transparency of Gaming**

- Show edit history: who changed status and when
- Flag rapid status changes (all items marked complete within 5 minutes = suspicious)
- Track patterns: if certain staff always mark complete faster, investigate
- Make gaming visible to reduce temptation

**10. Outcome-Based Validation**

- Correlate dashboard "ready" with actual discharge success
- Track: When marked READY_TO_DISCHARGE, what % actually discharge without issues?
- Use discrepancies to identify where false reporting may be occurring
- Feedback loop: "Last month, 40% of 'ready' discharges had problems - what's wrong?"

### The Fundamental Challenge

Even with guardrails, tension remains between:
- **Coordination need**: accurate state visibility helps teams work together
- **Performance pressure**: organizations want faster, smoother discharges
- **Human factors**: staff are busy, face pressure, and make tradeoffs

Explicit state models don't eliminate these tensions - they make them visible. The guardrails don't prevent gaming entirely, but they can:
- Make false reporting harder to do undetectably
- Reduce organizational incentives to game
- Create feedback loops that reveal when gaming is happening
- Support staff who want to be accurate despite pressure

**The real guardrail is organizational culture**: if leadership rewards speed over accuracy, gaming will persist regardless of technical controls. If leadership values honest communication and patient safety over metrics, the state model can be a genuine coordination tool.

## Deeper Questions

This scenario raises questions beyond "is explicit state better?":

### When is implicit state actually preferable?

For routine, low-risk situations where:
- Experienced staff know what to check
- Context is shared naturally
- Flexibility matters more than auditability

### What should be modeled explicitly?

Priority factors:
- High consequence if missed (medications, follow-up care)
- Coordination across teams/organizations (home health, equipment)
- Timing dependencies (24-hour notice requirements)
- Compliance requirements (documentation for payers)

Less critical to model:
- Well-established routines known by all
- Judgment calls requiring nuance
- Rapidly changing situations

### How do we model uncertainty?

The model showed "⏳ PENDING" and "⚠ ISSUE" states—acknowledging that not everything is binary (complete/incomplete).

Could we go further?
- Confidence levels: "70% confident family will be available"
- Soft blockers vs hard blockers
- "Provisional" states: "Can discharge if X is confirmed by Y time"

### What happens at the boundaries?

Mrs. Chen's discharge requires coordination with:
- Home health agency (different organization, different systems)
- Equipment company (commercial vendor)
- Pharmacy (maybe hospital-owned, maybe external)
- Family (no system at all)

**How does explicit state cross organizational boundaries?** The hospital can model its own view, but:
- Home health has their own state model
- The two models may not align
- Handoffs still require translation

## Implications

This scenario demonstrates:

1. **Implicit state creates real problems** - delays, risk, coordination burden
2. **Explicit state provides real benefits** - visibility, accountability, reduced ambiguity
3. **Explicit state creates new problems** - overhead, false precision, rigidity
4. **State models are approximations** - they simplify reality to make it manageable
5. **The value depends on context** - high-stakes, multi-party coordination benefits most

The goal is not to claim "explicit state is always better."

The goal is to understand **when**, **how**, and **at what cost** making state explicit changes outcomes—and to acknowledge what remains implicit even after modeling.

---

**Continue to:** [Finance Scenario →](finance.md)

**Explore:** [Core Concepts](../concepts/core.md) | [Failure Modes](../concepts/failure-modes.md) | [Back to Home](../index.md)