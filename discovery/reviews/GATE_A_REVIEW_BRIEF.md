# Third-Party Review Gate A — Discovery Architecture Checkpoint

## Reviewer instruction

Review the proposed EL8 Discovery rearchitecture independently before implementation of the new routing engine. Do not assume the design is correct because it incorporates prior third-party feedback. Look specifically for hidden complexity, missing states, invalid assumptions, and places where the design could recreate v2.6's failure modes under different names.

## Context

Discovery v2.6 used a deterministic weighted hypothesis/scoring engine. Human validation exposed premature closure, irrelevant cross-domain branching, weak contradiction handling, repeated `Not sure` wandering, incomplete multi-source driver capture, and an under-designed healthy-member route.

Two independent reviews converged on a substantial modification rather than replacement: preserve deterministic evidence/auditability, state-vs-driver separation, member-raised signals, short adaptive routes, and fatigue ceilings; replace global scoring as the routing control plane with a constrained policy layer.

The proposed architecture is documented in:
- `discovery/ARCHITECTURE.md`
- `discovery/POLICY_SPEC.md`
- `discovery/DEVELOPMENT_PLAN.md`
- `discovery/evals/README.md`
- `discovery/evals/cases/canonical.json`
- `discovery/evals/cases/human-regressions.md`

## Questions requiring explicit verdicts

1. Does the Active Signal / Candidate Driver / terminal Resolution model remain simple enough to avoid recreating the old graph complexity?
2. Is the richer evidence/provenance store appropriately separated from routing control?
3. Are the legal move types sufficient without becoming a disguised giant state machine?
4. Is `member answer evidence required for cross-domain authorization` too strict, too loose, or correct for this stage?
5. Can the uncertainty policy distinguish `I do not know why` from `I do not know whether there is a problem`?
6. Does the contradiction policy risk annoying users with unnecessary clarification? How should materiality be defined?
7. Are `confirmed`, `cleared`, `unresolved`, and `deferred` adequate terminal states?
8. Are per-signal soft budgets + global ceiling the right burden model?
9. Is the healthy/positive-goal route still appropriately considered part of Discovery?
10. Are readiness and plan calibration correctly excluded from Discovery?
11. Are the deterministic/heuristic boundaries correct?
12. Does the structured handoff preserve uncertainty strongly enough to prevent downstream false certainty?
13. Is any safety assumption unsafe, incomplete, or improperly placed?
14. Does the 30-case eval matrix test the architecture rather than merely mirror implementation ideas?
15. What critical cases are missing before implementation begins?
16. What would you remove or simplify before code is written?

## Required response format

- Gate A verdict: PASS / PASS WITH REQUIRED CHANGES / FAIL
- Critical blockers
- Required changes before implementation
- Recommended but non-blocking changes
- Missing evaluation cases
- Safety concerns
- Complexity concerns
- Final recommendation: proceed to implementation or return to architecture

A PASS should mean the reviewer believes it is reasonable to invest in implementing the new router, not that the eventual product is validated.
