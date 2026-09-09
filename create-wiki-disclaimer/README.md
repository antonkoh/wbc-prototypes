# create-wiki-disclaimer

Live: https://wbc-temporary-by-default.netlify.app

The last step of the "Create a wiki" dialog: the notice telling the person that every
Wikibase is temporary by default, plus the mandatory confirmation checkbox.

Built to review the copy, not the visual design.

## Decisions baked into this prototype

- **No retention question.** The step used to open with "How long do you plan to use this
  Wikibase?" (permanent / temporary / other / decide later). It was dropped: the answer
  changed nothing - every instance is temporary by default and the notice was identical
  for all four options - so it only asked people to state a preference the platform would
  then ignore. The paragraph that started "Whatever you answered above" went with it; the
  3-month horizon now sits in the "keep this Wikibase longer than 3 months" clause and in
  the checkbox.
- The date is computed at page load as today + 3 months, and phrased in the future tense
  throughout, because the instance does not exist yet at this point in the dialog.
- The date is bold in the checkbox. In the body the notice says "in 3 months" instead, to
  save vertical space - so the specific date the person agrees to appears exactly once.
- The confirmation is worded as understanding ("I understand that this Wikibase will go
  offline on...") rather than acceptance. The hosting policy was already accepted at
  signup; wording this as a second consent would muddy the record.
- The checkbox is mandatory and gates CREATE WIKI.
- **CREATE WIKI does nothing when clicked, on purpose.** This prototype is one dialog step,
  and nothing beyond it has been designed. Do not add a success screen, a confirmation
  state, or any other follow-on screen here - inventing screens nobody asked for confuses
  the team about what is actually decided.
- **No "Terms of Use" heading at the bottom.** The closing line covers both documents in one
  sentence - "Previously accepted terms of use and hosting policy still apply." - so a heading
  naming only one of them would be wrong, and a heading naming both would be longer than the
  sentence it introduces.
- All three policy links point at the real pages: `https://www.wikibase.cloud/terms-of-use` and
  `https://www.wikibase.cloud/hosting-policy`. They open in a new tab so the dialog state survives.
- The third paragraph names concrete disqualifiers (commercial or promotional use,
  non-free licence) and links to the policy. Kept to a few examples on purpose - the
  policy stays the single source of truth and the list must not drift from it.

## Known issue

The notice still makes the dialog step taller than the ~330px of the current dialog -
around 600px now that the retention question is gone, down from ~770px. Less likely to
push the checkbox and CREATE WIKI below the fold, but still worth a designer's eye.

## Stack

Vue 2.7 + Vuetify 2.6 from CDN, single self-contained `index.html`.
