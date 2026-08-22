# EL8 Discovery Policy Architecture

Status: architecture revision working contract
Baseline: Discovery v2.6 is frozen as the experimental reference. Do not patch v2.6 routing further.

## Objective
Discovery answers: **What is going on, and what appears to be driving it?**

Discovery does not decide intervention intensity, readiness, plan commitment, or final prioritization. Those belong downstream.

## High-level flow

Gateway -> Active Signal Queue -> Constrained Signal Interview -> Structured Understanding -> downstream Prioritization / Readiness -> Planning

Safety is a deterministic interrupt that may operate across stages.

## Non-negotiable invariants

1. Every material concern explicitly raised by the member must reach a terminal status before ordinary Discovery closure.
2. A high-confidence hypothesis in one domain cannot silently abandon another member-raised signal.
3. Cross-domain candidates require evidence from member answers. Graph association alone cannot authorize a new interview branch.
4. Contradictions trigger clarification or clearing; they do not authorize unrelated topic switching.
5. `Not sure` is an epistemic state, especially for causal questions. It changes questioning strategy rather than merely lowering a score.
6. Observable-state questions should normally use experiential scales. `Not sure` should be reserved mainly for causal or interpretive questions where uncertainty is genuinely valid.
7. `None of these / Generally well` is distinct from uncertainty and enters a positive-goal/maintenance Discovery route.
8. Question budgets are fatigue ceilings, not quotas.
9. Short routes are valid when evidence is sufficient.
10. Safety routing, signal permanence, terminal resolution, budget enforcement, and core state transitions remain deterministic and auditable.

## Control model

### Active Signal
A concern explicitly raised by the member. Active signals cannot disappear implicitly.

### Candidate Driver
A plausible explanation supported by evidence obtained while investigating an authorized signal. Candidate drivers may be cross-domain, but must have member-answer provenance.

### Resolution Status
Routing uses a deliberately small terminal vocabulary:
- `confirmed`
- `cleared`
- `unresolved`
- `deferred`

A contradiction is an event requiring clarification. After clarification it must produce an appropriate terminal/current state rather than remaining an ambiguous hidden score.

## Evidence record
The control model stays simple, but the evidence store should preserve:
- signal / driver id
- kind: state or driver
- provenance: question + answer that created/changed it
- direct evidence
- inferred evidence
- uncertainty
- confidence/calibration data where useful
- current status
- related active signal(s)

State certainty and driver certainty are separate. Example: `focus impairment = confirmed`, `driver = unresolved` is valid.

## Interview policy

The policy layer determines which moves are legal before any scoring occurs. Legal move types include:
- clarify
- scope
- impact / severity
- frequency / recent behavior
- driver discriminate
- confirm
- contradiction clarification
- healthy-goal route
- stop / defer

The selector may rank questions only among currently legal moves. Global hypothesis score does not control authorization.

### Cross-domain rule
A Work signal does not authorize Money merely because Work and Money are associated. A Work answer that explicitly indicates lost income, unstable employment, or financial impact may create a Money candidate with provenance and authorize a relevant bridge/clarification.

### `Not sure` rule
Causal/interpretive question -> `Not sure` -> stop asking equivalent causal questions -> pivot to observable impact, frequency, behavior, or recent-experience questions. If bounded questioning still cannot identify a driver, close the driver as `unresolved`; do not guess.

### Contradiction rule
If a member raises a concern and then gives evidence that appears to negate it, ask a low-friction clarification. Allow the member to confirm it is still a concern, narrow what they meant, or clear it.

### Healthy route
Gateway includes `None of these / Generally well`. This bypasses problem investigation, not EL8 itself. Follow with a positive route such as:
- maintain what is going well
- improve a selected wellness area
- build a habit/routine
- pursue a specific goal
- explore where there may be room to improve
- decide what EL8 should help with

## Stage boundaries

### Discovery
Truth-seeking: states, drivers, uncertainty, open/closed signals.

### Structured Understanding
Canonical handoff containing confirmed states/drivers, unresolved areas, evidence provenance, and confidence without pretending unresolved causes are known.

### Prioritization / Readiness
Answers: what matters now, how ready the member is to change, capacity, realistic effort, adherence barriers, preferred intensity.

### Planning
Selects tasks/interventions using the structured understanding + prioritization/readiness output.

## AI boundary
Deterministic core first. Future bounded LLM uses may include free-text interpretation, wording variation, summaries, and ambiguity flagging. LLMs do not independently alter safety state, authorize branches, silently change evidence, or make final signal-resolution decisions.

## Repository direction
Human-authored source must be modular and normally formatted. Do not recreate monolithic long-line question-bank files.

Target structure:

```text
discovery/
  engine/
  policies/
  questions/
  schemas/
  evals/
    cases/
    regressions/
  telemetry/
  ui/
```

Generated/minified artifacts, if any, belong in build output and are never hand-edited.

## Release philosophy
Architecture contract -> canonical eval cases -> implementation -> automated gates -> human validation.

Do not return to build -> human finds obvious routing defect -> patch -> repeat as the primary development method.
