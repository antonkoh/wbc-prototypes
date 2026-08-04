# wiki-profile

Live: https://wbc-hp-submissions.netlify.app

The manager's side of the hosting policy review process, shown as two panes side by side
so both halves of the flow can be discussed in one view.

| Pane | Title | What it shows |
|---|---|---|
| Left | Manager describes the use case | The Profile tab of an instance, with the questionnaire the review committee reads |
| Right | Manager submits the instance for review | The Review tab: confirmations, the submit action, and the list of past submissions |

The two panes are separated by a dark gutter in the same colour as the prototype banner,
to make clear they are two different screens rather than one page.

## Decisions baked into this prototype

Change these only deliberately - each one came out of a round of review.

**Left pane, the questionnaire**

- Five answers are mandatory: the reuse question, the intended purpose, contributors,
  the knowledge-equity question, and the GDPR consent checkbox. `requiredCount: 5`.
- The dataset question and the intended-audience question are **optional**, and carry no
  asterisk. Both open with a bolded condition ("If the Wikibase is intended to host a real
  dataset...") because they only apply to some instances.
- The profile cannot be saved until all five mandatory answers are given. Until then the
  footer counts what is missing rather than showing an error.
- The reuse question is kept verbatim from the `reuse/` prototype, including its long
  explanatory text and the bolded stability sentence in the first option.
- The GDPR consent checkbox locks once the profile has been saved with consent given
  (`consentLocked`). It cannot be unticked from the UI. Withdrawal is out of band, in
  writing, per the fine print underneath. An earlier version modelled withdrawal in the UI
  and was cut as overengineered.
- The privacy warning sits directly above the consent checkbox, not elsewhere on the page.

**Right pane, submissions**

- All three confirmation checkboxes are mandatory and carry a red asterisk.
- The submit card is **hidden entirely** while a submission is open, because a manager
  cannot have two open submissions. It reappears once the open one is cancelled, approved
  or rejected.
- A submission in `SUBMITTED` can be cancelled by the manager.
- Two submissions are seeded so the list is not empty: a `REJECTED` one dated 2026-06-03
  with the reason "CC BY-NC is not a compliant license, since it does not allow commercial
  use", and a `CANCELLED` one dated 2026-05-12.
- The rejection reason is shown without a "Review committee" label above it.

## Deliberately out of scope

The questions about licensing, the manager commitment, the disclaimer for visitors, and
the "any further questions" section from the pilot questionnaire are all excluded.

## Stack

Vue 2.7 + Vuetify 2.6 from CDN, single self-contained `index.html` plus `assets/`.

Watch out for the in-DOM template gotcha: HTML lowercases attribute names, so a
`v-slot:item.camelCase` never matches its column. Use snake_case header values.
