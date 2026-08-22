# EL8 Discovery Policy Specification

Status: Gate 1 architecture artifact

## 1. Session State

A Discovery session contains only truth-seeking state. Readiness, intervention selection and plan commitment are excluded.

```text
DiscoverySession
  version
  status: experimental | automated-rc | human-test-ready | pilot-candidate
  gateway
  activeSignals[]
  candidates[]
  evidence[]
  events[]
  asked[]
  currentSignalId?
  globalQuestionCount
  globalQuestionCeiling
  safetyState
  exit?
```

## 2. Active Signal

```text
ActiveSignal
  id
  source: member | evidence-bridge
  raisedAt
  status: active | confirmed | cleared | unresolved | deferred
  stateConfidence?
  driverStatus: unknown | candidate | confirmed | cleared | unresolved
  candidateDriverIds[]
  questionCount
  softQuestionBudget
  contradictionPending: boolean
```

Rules:
- Member-raised signals are durable until terminal.
- Evidence-bridge signals must retain provenance to the answer that authorized them.
- A signal cannot be removed because another signal has higher confidence.
- Global closure is illegal while a material signal is `active`.

## 3. Candidate Driver

```text
CandidateDriver
  id
  forSignalIds[]
  status: candidate | confirmed | cleared | unresolved
  provenanceEvidenceIds[]
  directSupport
  inferredSupport
```

A candidate driver cannot be created solely from a static graph edge. At least one member-answer evidence record must authorize it.

## 4. Evidence

```text
EvidenceRecord
  id
  questionId
  answerId | answerIds[] | freeTextRef
  timestamp/order
  kind: direct-state | direct-driver | bridge | contradiction | correction | uncertainty | safety
  targetIds[]
  polarity: support | oppose | unknown
  strength?
  observable: boolean
```

Policy consumes evidence. Question content emits evidence/events but cannot directly mutate session state.

## 5. Events

Canonical events:
- `gateway-signal-raised`
- `gateway-healthy`
- `candidate-authorized`
- `uncertainty-declared`
- `contradiction-detected`
- `member-correction`
- `signal-confirmed`
- `signal-cleared`
- `signal-unresolved`
- `signal-deferred`
- `safety-triggered`
- `member-opt-out`
- `budget-reached`

## 6. Legal Interview Moves

```text
clarify
scope
impact
severity
frequency
recent-behavior
driver-discriminate
confirm
bridge-confirm
contradiction-clarification
healthy-goal
safety-check
stop
```

The policy layer returns legal moves. A selector may rank only questions matching those moves for the authorized signal.

## 7. Signal Scheduling

Default scheduling priority:
1. safety interrupt
2. pending contradiction/correction
3. current member-raised signal needing minimal closure
4. another unresolved member-raised signal
5. evidence-authorized bridge candidate
6. stop

Heuristics may choose among equally legal options, but may not override this authorization order.

Multi-signal fairness: after a signal reaches its soft question budget without a critical reason to continue, rotate to another open member-raised signal and return later if global burden permits.

## 8. Uncertainty Policy

`Not sure` is not ordinary neutral evidence.

When a causal/interpretive question receives uncertainty:
1. record `uncertainty-declared`;
2. prohibit equivalent causal move repetition for that signal until observable evidence is gathered;
3. authorize `impact`, `severity`, `frequency`, or `recent-behavior`;
4. after bounded observable follow-up, either authorize a supported candidate or mark driver `unresolved`;
5. never infer a driver merely to avoid unresolved output.

## 9. Contradiction Policy

A contradiction occurs when new direct member evidence materially opposes a currently active interpretation or the member appears to negate a gateway signal.

Policy:
1. mark `contradictionPending`;
2. next legal non-safety move must include `contradiction-clarification`;
3. clarification may preserve/narrow the signal, clear it, or correct prior evidence;
4. unrelated branch switching is forbidden while contradiction is pending.

## 10. Correction Policy

Explicit member correction outranks prior inference and ordinary supporting evidence. Preserve provenance rather than deleting history.

A correction may:
- clear a candidate driver;
- reopen a signal;
- clear a signal;
- replace an incorrectly interpreted answer;
- authorize a different candidate if the corrected content supports it.

## 11. Cross-Domain Authorization

A cross-domain bridge requires a member answer whose emitted evidence explicitly targets or authorizes the candidate domain/driver.

Example allowed:
Work -> answer `I lost my job and have no income` -> evidence supports `no-income` and authorizes Money candidate.

Example forbidden:
Work -> answer `workload is heavy` -> static Work/Money association -> Money question.

## 12. Healthy Route

Gateway `None of these / Generally well` creates `gateway-healthy` and does not create problem signals.

Legal moves become `healthy-goal` and optional positive exploration. The route may establish desired EL8 purpose without manufacturing a deficit.

Healthy is not equivalent to `Not sure`.

## 13. Sufficiency & Terminal States

A signal may become:

`confirmed` — sufficient direct evidence establishes the experienced state/problem for Discovery's immediate purpose.

`cleared` — member evidence establishes that the initially raised concern is not currently material or was selected in error.

`unresolved` — a meaningful concern exists or may exist, but bounded evidence cannot establish necessary scope/driver detail without guessing.

`deferred` — investigation stops because of member choice, global fatigue ceiling, safety interruption, or explicit downstream deferral.

State confirmation does not require driver confirmation.

## 14. Stop Policy

Ordinary resolved exit is legal only when every material member-raised signal is terminal and no contradiction is pending.

Other legal exits:
- healthy-goal
- unresolved
- deferred
- opt-out
- safety-interrupt

At a global ceiling, remaining active signals are converted explicitly to `deferred` or `unresolved` according to evidence. They are never silently treated as resolved.

## 15. Question Burden

Use per-signal soft budgets plus a global ceiling. Budgets are configurable policy values, not proof of sufficiency.

A clear signal may terminate in very few questions. A difficult signal may consume more only while legal questions have meaningful expected value and burden remains acceptable.

## 16. Structured Understanding Handoff

Discovery output:

```text
DiscoveryUnderstanding
  confirmedStates[]
  confirmedDrivers[]
  unresolvedSignals[]
  deferredSignals[]
  clearedSignals[]
  evidenceProvenance[]
  confidence/calibration summaries
  safetyDisposition
  discoveryExitClass
```

Downstream systems must not convert `unresolved` candidates into confirmed premises.

## 17. Deterministic / Heuristic Boundary

Deterministic:
- safety interrupts
- active-signal permanence
- branch authorization
- contradiction/correction priority
- terminal-state legality
- budget enforcement
- structured handoff semantics

Heuristic/optimizable within legal space:
- ranking two legal questions of the same authorized move class
- burden-aware ordering among equally open signals
- optional information-value estimates

Future LLM assistance remains outside final authorization and resolution decisions.
