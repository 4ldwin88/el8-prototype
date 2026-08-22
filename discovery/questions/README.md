# Discovery Question Modules

Question content is separated from interview policy.

Each domain module should export ordinary structured question definitions. The policy engine decides whether a move is authorized; a question definition does not independently open another branch.

## Proposed question shape

```js
{
  id: 'WORK_SCOPE_01',
  signal: 'work',
  move: 'scope',
  mode: 'single',
  text: 'What part of work or school has been weighing on you?',
  options: [
    { id: 'workload', label: 'Workload or demands', evidence: [...] },
    { id: 'schedule', label: 'Schedule or hours', evidence: [...] },
    { id: 'people', label: 'People or environment', evidence: [...] },
    { id: 'stability', label: 'Stability or security', evidence: [...] },
    { id: 'direction', label: 'Direction or fit', evidence: [...] },
    { id: 'nothing-major', label: 'Actually, nothing major', event: 'contradiction' },
    { id: 'other', label: 'Something else', event: 'other' }
  ]
}
```

## Rules

- Keep normal line lengths and one domain/concern family per file.
- Prefer structured data over executable routing logic in question files.
- Question definitions may emit evidence/events; they may not directly mutate global state.
- Cross-domain evidence must carry provenance and is evaluated by policy before a candidate branch is authorized.
- Multi-select is used where causes can coexist.
- Single-select is used only when choices are genuinely exclusive.
- `None of these / Generally well` is a gateway semantic, not an uncertainty answer.
- `Not sure` should generally appear only on causal/interpretive questions where uncertainty is truthful.
- Impact/frequency/severity questions should use graded observable answers and normally omit `Not sure`.
- `Other` should preserve the fact that the model is incomplete rather than forcing a known driver.

## Initial modules

- `gateway.js`
- `work.js`
- `money.js`
- `sleep.js`
- `focus.js`
- `energy.js`
- `relationships.js`
- `home.js`
- `activity.js`
- `stress.js`
- `safety.js`

Readiness is intentionally not part of Discovery question modules. It belongs downstream.
