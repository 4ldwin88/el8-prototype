# Discovery Canonical Evaluation Suite

The evaluation suite is the specification for adaptive behavior. Exact question sequences are not generally required; policy outcomes and prohibited behavior are.

## Core release gates

A revision is not human-test ready unless all critical deterministic cases pass.

Required gates:

- 0 silent abandonment of material member-raised signals.
- 0 unauthorized cross-domain branches in deterministic temptation cases.
- Healthy gateway route succeeds.
- Repeated uncertainty pivots question strategy and can terminate as unresolved without manufacturing a driver.
- Contradictions trigger clarification/clearing.
- Explicit member corrections override prior inference.
- Multiple independent concerns remain represented until terminal.
- Multi-source drivers preserve all applicable causes.
- Safety interrupts pass their dedicated deterministic tests.
- No false `resolved` outcome when required evidence is absent.

Driver accuracy is measured only on cases where the scenario provides enough evidence to establish a driver. `Unresolved` is a correct outcome when the evidence is intentionally insufficient.

## Metrics

Track at minimum:

- member-raised signal terminal-resolution rate
- silent-abandonment count
- unauthorized cross-domain branch count
- supported-driver identification rate
- unsupported-driver assertion rate
- false closure rate
- contradiction handling success
- uncertainty recovery success
- healthy-route success
- questions to usable understanding
- unnecessary-question count
- route sensitivity to materially different answers
- safety interrupt success
- human-rated conversational coherence during validation

## Case families

The canonical matrix should contain at least 30 cases before the new router is considered human-test ready. Cases should cover:

1. Healthy / generally well
2. Healthy + explicit improvement goal
3. Single clear Sleep concern
4. Single clear Work concern
5. Single clear Money concern
6. Money with multiple simultaneous sources
7. Work without Money (cross-domain temptation)
8. Relationships without Health (cross-domain temptation)
9. Focus without known cause
10. Repeated `Not sure` on causal questions
11. Contradiction after gateway selection
12. Explicit correction mid-branch
13. Multiple independent concerns
14. Multiple connected concerns
15. Upstream/downstream relationship supported by evidence
16. Upstream/downstream relationship not supported by evidence
17. One concern clears while another remains active
18. Strong evidence allowing short route
19. Weak evidence requiring unresolved outcome
20. Member opt-out
21. Fatigue/global ceiling with unresolved signal
22. Several concerns competing for interview time
23. `None of these` distinct from `Not sure`
24. Observable-state severity scales
25. Multi-select causes
26. `Other` / unmodeled cause
27. Candidate driver later contradicted
28. Previously cleared concern reintroduced by explicit new evidence
29. Safety signal introduced mid-interview
30. Safety-negative control case

Additional cases should be added for every human-discovered regression.

## Acceptance model

Each case declares:
- member-raised signals
- answer strategy / scripted responses
- required terminal states
- allowed cross-domain candidates
- forbidden branches
- whether a driver is knowable
- maximum acceptable burden where relevant
- expected exit class
- safety expectations

Avoid overfitting to an exact question ID sequence unless order itself is safety- or policy-critical.
