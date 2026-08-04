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

## Two variants

The submissions story has an open capacity decision: build the wiki profile in the
platform, or keep the questionnaire in an external tool for now. Each option has its own
page, and the prototype banner switches between them.

| URL | File | Scope |
|---|---|---|
| `/` | `index.html` | Landing page, picks one of the two |
| `/full` | `full/index.html` | Full scope - wiki profile in the platform, full submission model |
| `/mvp` | `mvp/index.html` | MVP scope - external questionnaire, one submission or none |

The two variants are **separate files on purpose**. They are expected to diverge as the
decision is discussed, and the full one must not break while the MVP one is edited. Fixes
that apply to both have to be made twice.

`assets/` stays at the folder root; both variants reference it as `../assets/`.

## Decisions baked into the full variant

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

## Decisions baked into the MVP variant

- There is **no Profile tab** in the tab bar. In this option the wiki profile does not exist
  in the platform at all, so showing the tab would misrepresent the scope.
- The left pane is kept, but holds only the sentence "The manager fills out a questionnaire
  in an external tool." The two-pane frame is what makes the two variants comparable.
- The gate on the wiki profile is replaced by a **fourth mandatory checkbox**, "I confirm
  that I have filled out the questionnaire for this Wikibase in the external tool." That is
  the only signal available once the profile is out of the platform.
- The external questionnaire is introduced as a lead-and-checkbox pair, in the same shape
  as the other three confirmations, rather than as a call-out box. The double work is
  stated in the lead text, with an external-link icon on the link.
- The other three confirmations (licensing, project disclaimer, manager commitment) are
  unchanged from the full variant.
- A submission either exists or it does not. No states beyond `SUBMITTED`, no history, no
  cancelling, and the card is titled "Your submission", singular.
- The submit card and the submission card are **mutually exclusive** - exactly one is on
  screen at any time. There is no empty state for the submission card, because a manager
  who has not submitted is looking at the form instead.
- Nothing is seeded. The prototype always opens on the empty form.
- Since there is no cancelling, a "Reset prototype" button sits in the top-right of the
  prototype banner so the flow can be demoed twice without a page reload. It is demo
  scaffolding, outside the simulated product UI on purpose.
- The questionnaire link points at `https://example.org/...` - a placeholder. Swap it for
  the real form URL before showing this to anyone outside the team.

## Stack

Vue 2.7 + Vuetify 2.6 from CDN, single self-contained `index.html` plus `assets/`.

Watch out for the in-DOM template gotcha: HTML lowercases attribute names, so a
`v-slot:item.camelCase` never matches its column. Use snake_case header values.
