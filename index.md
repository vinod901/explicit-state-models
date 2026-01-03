---
layout: default
title: Home
---

# Explicit State Models

## A Documentation Exploration

This repository examines a recurring structural pattern across industries: **real-world state is rarely modeled explicitly in software systems**. Instead, state remains implicit—fragmented across events, metrics, documents, and human judgment.

Through detailed, end-to-end scenarios from healthcare and finance, we explore how this absence of explicit state models creates ambiguity, inconsistency, and systemic problems—and what changes when state is made explicit.

This is not a solution catalog or a technology pitch. It is an exploratory documentation project examining both the benefits and failure modes of explicit state modeling.

## What We Mean by "State"

State describes **where something is** in a process or condition:
- A loan application is "pending credit review"
- A patient is "awaiting test results"
- An order is "shipped but not delivered"

When state is **implicit**, this information exists only in human knowledge, scattered documents, or must be inferred from events and metrics.

When state is **explicit**, it is formally represented in systems as a named, trackable value with defined transitions.

## The Pattern We're Exploring

Across domains, we observe:

1. **State exists implicitly** - known by humans but not captured in systems
2. **Ambiguity emerges** - different interpretations of "what's happening now"
3. **Coordination fails** - handoffs lose context, responsibilities blur
4. **Modeling is attempted** - state is made explicit through formal models
5. **Trade-offs appear** - new problems alongside benefits

This repository documents concrete scenarios showing this pattern, including where explicit state modeling helps—and where it creates new complications.

## Scenarios

### [Healthcare: Patient Discharge Coordination](/scenarios/healthcare)
A patient is ready for discharge, but implicit state about home care arrangements, medication availability, and follow-up scheduling creates delays and risks. We examine how explicit state models might help—and what they cannot solve.

### [Finance: Commercial Loan Approval](/scenarios/finance)
A loan application moves through multiple review stages, but fragmented state about approvals, exceptions, and pending documentation creates confusion and compliance risk. We explore explicit state modeling and its limitations.

## Core Concepts

### [States, Transitions, and Invariants](/concepts/core)
What explicit state models include: named states, valid transitions, and invariants that must hold.

### [Confidence and Uncertainty](/concepts/uncertainty)
State models are approximations. We examine how to represent confidence levels and acknowledge what cannot be captured.

### [Failure Modes](/concepts/failure-modes)
When explicit state models create new problems: over-specification, false precision, coordination overhead, and resistance to reality.

## What This Project Is Not

- **Not a platform or tool** - no code, no UI, no vendor solutions
- **Not a standard proposal** - premature standardization obscures learning
- **Not optimization-focused** - efficiency gains are secondary to understanding
- **Not advocating state models everywhere** - sometimes implicit is appropriate

## About This Project

This is **living documentation**, published via GitHub Pages, continuously evolving as we explore more scenarios and deepen our understanding of when and how explicit state modeling matters—and when it doesn't.

The goal is clarity, not completeness. We favor concrete scenarios over abstract theory, and acknowledge trade-offs over claiming solutions.

---

**Start with:** [Healthcare Scenario](/scenarios/healthcare) | [Finance Scenario](/scenarios/finance) | [Core Concepts](/concepts/core)
