# create-wiki-disclaimer

Live: https://wbc-temporary-by-default.netlify.app

The last step of the "Create a wiki" dialog, with the notice telling the person that every
Wikibase is temporary by default, plus the mandatory confirmation checkbox.

Built to review the copy, not the visual design.

## Decisions baked into this prototype

- **One notice for everyone.** The text does not vary with the retention answer above it.
  Only two of the four options could carry a variant anyway, and the tempting variant for
  "temporary/disposable" ("nothing to do") would be told to exactly the group most likely
  to change their mind later.
- The retention question is reproduced above the notice so that the clause "whatever you
  answered above" has something to refer to.
- The date is computed at page load as today + 3 months, and phrased in the future tense
  throughout, because the instance does not exist yet at this point in the dialog.
- The date is bold in the checkbox. In the body the notice says "in 3 months" instead, to
  save vertical space - so the specific date the person agrees to appears exactly once.
- The confirmation is worded as understanding ("I understand that this Wikibase will go
  offline on...") rather than acceptance. The hosting policy was already accepted at
  signup; wording this as a second consent would muddy the record.
- The checkbox is mandatory and gates CREATE WIKI.
- The third paragraph names concrete disqualifiers (commercial or promotional use,
  non-free licence) and links to the policy. Kept to a few examples on purpose - the
  policy stays the single source of truth and the list must not drift from it.
- After creation, a confirmation card shows the scheduled date and the date the person
  confirmed it. That is a prompt for the team about what actually needs storing.

## Known issue

The notice roughly doubles the height of the dialog step - around 770px against the ~330px
of the current dialog. On a modal that size the checkbox and the CREATE WIKI button can
end up below the fold. Needs a designer.

## Stack

Vue 2.7 + Vuetify 2.6 from CDN, single self-contained `index.html`.
