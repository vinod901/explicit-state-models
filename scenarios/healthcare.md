---
layout: default
title: Healthcare Scenario - Patient Discharge Coordination
---

# Healthcare Scenario: Patient Discharge Coordination

[← Back to Home](/)

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

**Continue to:** [Finance Scenario →](/scenarios/finance)

**Explore:** [Core Concepts](/concepts/core) | [Failure Modes](/concepts/failure-modes) | [Back to Home](/)