# EL8 Discovery — Gated Development Plan

Status: governing development plan for the Discovery policy rearchitecture
Baseline: Discovery v2.6 is frozen as the experimental reference.

## Purpose

Prevent the previous cycle of implementation -> obvious human failure -> patch -> repeat. Development proceeds through explicit gates. A phase cannot advance merely because code exists; its acceptance evidence must pass first.

## Review roles

- **Internal engineering review:** architecture contract, implementation, tests, traceability, repository hygiene.
- **Automated evaluation:** canonical deterministic scenario matrix and regression suite.
- **Third-party architecture review:** independent critique at high-leverage design points before expensive implementation or downstream integration.
- **Human validation:** member-experience testing only after automated gates pass. Human testing is for coherence, naturalness, omissions, and unforeseen behavior—not basic defects the simulator should catch.

## Gate 0 — Baseline Freeze & Evidence Capture

Deliverables:
- Freeze v2.6 behavior as experimental baseline.
- Preserve all human-discovered failures as permanent regressions.
- Record third-party review conclusions and architecture decisions.

Pass criteria:
- No further feature patches to v2.6.
- Human regressions represented in the eval specification.
- Architecture decision explicitly states what is retained, modified, removed, and deferred.

Review: internal + completed third-party review.

## Gate 1 — Architecture Contract

Deliverables:
- Stage boundaries: Gateway, Discovery, Structured Understanding, Prioritization/Readiness, Planning.
- Active Signal, Candidate Driver, Resolution Status and evidence provenance contracts.
- Legal interview move types.
- Cross-domain authorization rule.
- Uncertainty, contradiction, correction, healthy-route and stopping policies.
- Deterministic safety boundary.

Pass criteria:
- No known human regression is left without a policy-level expected behavior.
- No readiness/intervention logic remains inside Discovery truth-seeking.
- Every member-raised material signal has a terminal-state requirement.

Review: internal architecture review.

### Third-Party Review Gate A — Architecture Checkpoint

Trigger: after Gate 1 artifacts are complete, before replacing the routing engine.

Ask an independent reviewer to inspect the architecture contract + eval specification, specifically for:
- hidden complexity or re-creation of the old scoring problem;
- missing epistemic states;
- unsafe or invalid safety assumptions;
- weak stopping/sufficiency rules;
- over-constrained routing that prevents legitimate cross-domain discovery;
- whether the planned evals actually test the architecture.

Pass criteria:
- No unresolved critical architecture objection.
- Material recommendations are accepted, explicitly rejected with rationale, or deferred with rationale.

## Gate 2 — Evaluation Specification Before Router

Deliverables:
- Machine-readable eval schema.
- Minimum 30 canonical cases.
- Permanent human-regression cases.
- Case expectations based on outcomes/policy, not exact question order except where order is critical.
- Metrics and release thresholds.

Pass criteria:
- Coverage includes healthy, single concern, multi-concern, uncertainty, contradiction, correction, cross-domain positive/negative, unknown driver, opt-out, budget, and safety.
- Correct `unresolved` outcomes are rewarded rather than treated as failures.
- Zero-silent-abandonment and zero-unauthorized-branch rules are machine-checkable.

Review: internal eval review.

## Gate 3 — Repository & Question-Bank Refactor

Deliverables:
- Modular `discovery/` source tree.
- Question modules separated from routing policy.
- Normal line lengths; no hand-authored monolithic/minified source.
- Schema validation for question definitions and eval cases.
- Generated artifacts isolated from source.

Pass criteria:
- A single domain question can be edited without replacing the full question bank.
- Question definitions cannot directly mutate global interview state.
- Existing baseline can still be reproduced or archived for comparison.
- CI validates schemas and source integrity.

Review: internal engineering review.

## Gate 4 — Policy Engine Skeleton

Deliverables:
- Active Signal Queue.
- Evidence store with provenance.
- Terminal resolution enforcement.
- Legal-move policy layer.
- Question selector constrained to legal moves.
- No uncontrolled global-score authorization.

Pass criteria:
- Policy unit tests pass.
- Member-raised signals cannot silently disappear.
- A selector cannot choose a forbidden branch even if its heuristic score is high.
- Unknown driver can terminate as unresolved.

Review: internal engineering + automated unit tests.

## Gate 5 — Edge-State Policies

Deliverables:
- Healthy / positive-goal route.
- `Not sure` causal-to-observable pivot.
- Contradiction clarification/clearing.
- Explicit member correction override.
- Multi-signal scheduling.
- Evidence-backed cross-domain bridge.
- Per-concern soft burden + global fatigue ceiling.

Pass criteria:
- All corresponding canonical cases pass.
- No false resolution at budget ceiling.
- Short clear routes remain possible.

Review: automated eval suite.

## Gate 6 — Safety Integration

Deliverables:
- Deterministic safety interrupt contract.
- Direct-question escalation where required.
- Negative controls preventing ordinary distress from being incorrectly escalated.
- Safety state isolated from ordinary heuristic scoring.

Pass criteria:
- 100% pass on critical deterministic safety cases.
- No ordinary Discovery recommendation can override a safety interrupt.
- Safety-negative controls pass.

Review: internal safety review + automated tests.

### Third-Party Review Gate B — Pre-Human Technical Review

Trigger: Gates 3–6 pass, before human testing.

Provide reviewer:
- target architecture;
- implementation diff;
- automated eval report;
- failures fixed from v2.6;
- remaining known limitations.

Ask whether implementation matches the reviewed architecture and whether any defect makes human testing premature.

Pass criteria:
- No critical blocker.
- Any high-severity issue is fixed and eval coverage added before proceeding.

## Gate 7 — Automated Release Candidate

Required release gates:
- 0 silent abandonment of material member-raised signals.
- 0 unauthorized cross-domain branches in deterministic negative cases.
- 100% pass for contradiction/correction invariants.
- 100% pass for healthy-route invariants.
- 100% pass for uncertainty-pivot invariants.
- 100% pass for critical safety cases.
- 0 unsupported-driver assertions in cases intentionally lacking sufficient evidence.
- Supported-driver identification meets the agreed threshold only on cases where a driver is knowable.
- All prior human regressions pass.
- Schema/source integrity checks pass.

Only after this gate may a build be labeled **Human-Test Ready**.

## Gate 8 — Human Validation Round 1

Purpose: validate conversation quality, not basic policy correctness.

Minimum scenario set:
- healthy / generally well;
- single obvious problem;
- several unrelated problems;
- contradiction/correction;
- uncertain cause;
- supported cross-domain connection;
- short clear route.

Tester feedback captures:
- Did the questions follow what you actually said?
- Was anything important left unexplored?
- Did EL8 introduce anything you never implied?
- Did it ask you to diagnose something you could only observe?
- Was the route too short, too long, repetitive, invasive, or confusing?
- Does the final understanding match what you meant?

Pass criteria:
- No critical routing/safety defect.
- Any reproducible defect becomes a permanent automated regression before being fixed.
- Human coherence acceptance target is defined before the round and met.

## Gate 9 — Human Validation Round 2 / Broader Pilot

Trigger: Round 1 defects are fixed and automated gates pass again.

Use more diverse scenarios/testers. Validate route diversity, wording, burden, healthy-member usefulness, and ambiguity handling.

Pass criteria:
- No recurring critical architecture defect.
- No pattern of false certainty or systematic irrelevant branching.
- Burden is acceptable for intended onboarding/discovery context.

### Third-Party Review Gate C — Downstream Integration Review

Trigger: before Discovery output is allowed to drive Prioritization, Readiness, or Planning in a consequential way.

Review:
- structured handoff schema;
- confidence/uncertainty language;
- unresolved-driver handling;
- privacy/provenance retention;
- risk of downstream systems treating hypotheses as facts.

Pass criteria:
- downstream contracts preserve uncertainty and provenance;
- unresolved information cannot silently become a confirmed planning premise;
- no critical review objection remains unresolved.

## Gate 10 — Downstream Integration

Only now connect validated Discovery output to Prioritization/Readiness and Planning.

Readiness questions are developed and evaluated as their own stage. Discovery is not expanded to absorb them.

## Change-control rule

Every human-discovered defect follows this sequence:
1. capture trace;
2. define expected policy behavior;
3. add failing automated regression;
4. fix implementation;
5. run full gates;
6. human retest only when automated suite is green.

Never patch a production/test route first and add the test afterward.

## Version/readiness labels

- `Experimental` — architecture or implementation work; not for human testing.
- `Automated RC` — implementation complete; automated gates running/passed.
- `Human-Test Ready` — Gate 7 passed; explicit tester instructions may be issued.
- `Pilot Candidate` — Round 1 passed and regressions resolved.
- `Integration Candidate` — broader pilot + Third-Party Gate C passed.

The visible test page must always show the exact Discovery version/status so testers can verify what they are running.
