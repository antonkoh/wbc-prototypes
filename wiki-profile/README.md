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

**Resetting, both variants**

Both variants carry a "Reset prototype" button in the top-right of the prototype banner. It
is demo scaffolding, deliberately outside the simulated product UI, and it puts the page
back to how it opens - in the full variant that means the consent lock is released and the
two seeded submissions come back.

This is the **only** reset. The profile card used to have its own Reset button next to Save;
it was removed because two resets side by side read as the same action, and the difference
between them (one respects the consent lock, one does not) was invisible. The profile card
now offers Save alone.

**Wording, both variants**

- Never say "suspended" in copy the manager reads. It is the internal term for what happens
  to an instance, and it belongs in the admin prototype, not here. Say what the manager
  gets instead: "Your instance will stay online while we are reviewing your submission."

**Left pane, the questionnaire**

- The intended-purpose and knowledge-equity questions keep their lead paragraphs above the field,
  but their examples live **inside the field as its placeholder**, one per line, under "Here are
  examples of the information we are looking for:". They vanish the moment the manager types,
  which is the point - they are a prompt, not standing instructions.
- All question fields are 13px with `line-height: 1.55` (`.q-block textarea`), matching `.q-sub`
  around them. The rule is on the textarea, not on `::placeholder`, so prompt and typed answer
  share it - Vuetify's default 16px/28px left the prompt lines looking double-spaced.
- The two fields carrying examples get their height from `min-height` (`.purpose-field` 224px,
  `.equity-field` 164px), sized to the prompt plus one spare line at the narrowest width the
  two-pane layout reaches; `rows` stays at 4 like the others. `rows` alone was not reliable -
  auto-grow recomputes the height and lands a few pixels short. Auto-grow still works above the
  floor. If the example text changes, re-measure.
- The contributors question still carries its two examples as `<ul class="q-examples">` above the
  field. It was left alone deliberately; only purpose and knowledge equity moved.
- Four answers are mandatory: the reuse question, the intended purpose, contributors, and
  the GDPR consent checkbox. `requiredCount: 4`.
- The dataset question, the intended-audience question and the knowledge-equity question
  are **optional**, and carry no asterisk. The first two open with a bolded condition ("If
  the Wikibase is intended to host a real dataset...") because they only apply to some
  instances.
- The knowledge-equity question is a single optional free-text field asking whether the
  Wikibase represents knowledge or a community facing barriers to inclusion, with three
  examples of what the committee is looking for. It replaced an earlier yes/no/unsure radio
  group plus conditional follow-up, which was mandatory.
- The profile cannot be saved until all four mandatory answers are given. Until then the
  footer counts what is missing rather than showing an error.
- The reuse question is kept verbatim from the `reuse/` prototype, including its long
  explanatory text and the bolded stability sentence in the first option.
- The GDPR consent covers processing "for the purpose of **reviewing** my Wikibase Cloud
  instance" - not creating it. The profile is filled out for the review committee, and the
  instance already exists by then.
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
- Below the four confirmations sits an **optional free-text field**. Its prompt - "Feel free to
  pass on any additional information to the review committee to support your submission." - is the
  field's own `placeholder`, not a label above it, matching how the live survey renders its
  optional questions. It is A7 in
  `applicant-questionnaire.md`, moved out of the external survey and into the submission itself.
  It carries no asterisk and does not gate the submit button. Capped at **1,000 characters** with
  a live counter: A7 maps to no grading criterion, so the limit keeps it a note rather than a
  second route for the case that belongs in the graded questions. The cap is `maxlength`, so the
  field stops accepting input - it never truncates a message the manager thinks was sent.
- The MVP variant only. The full variant does not have this field yet.
- A submission either exists or it does not. No states beyond `SUBMITTED`, no history, no
  cancelling, and the card is titled "Your submission", singular.
- The submit card and the submission card are **mutually exclusive** - exactly one is on
  screen at any time. There is no empty state for the submission card, because a manager
  who has not submitted is looking at the form instead.
- Nothing is seeded. The prototype always opens on the empty form.
- Since there is no cancelling, a "Reset prototype" button sits in the top-right of the
  prototype banner so the flow can be demoed twice without a page reload. It is demo
  scaffolding, outside the simulated product UI on purpose.
- The questionnaire link points at the real survey,
  `https://wikimedia.sslsurvey.de/WBC-Hosting-Policy-Review-Submission/?<wiki_id>`. The
  `<wiki_id>` stays literal in the prototype - the platform would substitute the instance id
  so the committee knows which Wikibase an answer belongs to.
- A free-text field under the confirmations lets the manager pass additional information to
  the review committee. Optional, capped at 1000 characters.

## Stack

Vue 2.7 + Vuetify 2.6 from CDN, single self-contained `index.html` plus `assets/`.

Watch out for the in-DOM template gotcha: HTML lowercases attribute names, so a
`v-slot:item.camelCase` never matches its column. Use snake_case header values.

Second gotcha, found the hard way: never set `line-height` inside a `::placeholder` rule on an
`auto-grow` textarea. It feeds back into Vuetify's height measurement and the field grows to
thousands of pixels on load. `font-size` alone is safe.
