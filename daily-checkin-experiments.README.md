# Daily check-in experiment selector

`daily-checkin-experiments.js` isolates controlled question experiments from the normal scheduled-question engine.

Integration contract for `daily-checkin.html`:

1. Import `selectDailyCheckinExperiment` and `recordQuestionExposure`.
2. Call the selector only after safety/required/carry-forward and scheduled questions have been determined.
3. Render at most one returned experimental question, after those higher-priority questions.
4. Require an answer only when the experimental question was actually rendered.
5. Include the experimental question and answer in the submitted check-in snapshot.
6. After the daily check-in row is created, call `recordQuestionExposure` with its `checkinId`.

The selector currently uses balanced rotation among active variants and respects experiment eligibility such as the T0006-only `m0_plan_fit_v1` experiment.
