# hosting-policy-admin

Live: https://bucolic-pegasus-82a9af.netlify.app

The review committee administrator's view. Two tabs:

| Tab | What it does |
|---|---|
| Submissions | See and filter submissions, change their status, set or remove the scheduled suspension date, record a decision explanation, unsuspend an instance on pick-up |
| Instances | Suspend, unsuspend or schedule any instance out of process, regardless of submissions |

The date is pinned: `const TODAY = '2026-10-19'`, so the seeded data always tells the same
story.

## Decisions baked into this prototype

- Submission states are `SUBMITTED`, `IN_REVIEW`, `APPROVED`, `REJECTED`, `CANCELLED`.
  `APPROVED`, `REJECTED` and `CANCELLED` are terminal; the other two are not.
- **Protection is per instance, not per submission.** A live instance is protected from
  suspension while *any* of its submissions is in a non-terminal state, whatever its
  scheduled date. Shown with a lock icon. An earlier version checked this per submission
  and additionally required the date to be in the past, which produced instances that
  should have been protected but showed nothing.
- `CANCELLED` counts as terminal, so cancelling releases the protection. This was decided
  explicitly: treating it as non-terminal would let a manager make an instance permanent
  by cancelling, and would block resubmission.
- Rejecting a submission proposes a new suspension date of today + 2 weeks, never earlier
  than 3 months after the instance was created.
- The column is "Suspension due", not "Suspension date", so it is not confused with the
  date an instance was actually suspended. Dates in the past are highlighted in light red.
- Icons: suspend is `mdi-close-octagon`, unsuspend is `mdi-restore`. `mdi-lock` means
  protected and nothing else - it was previously on the suspend button, which read as the
  opposite.
- Instance IDs are plain numbers.

## Deliberately out of scope

Grading, the comparison of grades across reviewers, and all communication with managers.

## Stack

Vue 2.7 + Vuetify 2.6 from CDN, single self-contained `index.html`.

Two traps this file has already hit: in-DOM templates lowercase attribute names, so
`v-slot:item.camelCase` silently never matches - use snake_case header values. And a
numeric id written `0644` is an octal literal, not the number 644.
