# create-wiki-purpose

Step 2 of the "Create a wiki" dialog, reworked: the purpose question with options aligned to
the hosting-policy categories, plus two new follow-ups that tell us whether a new Data Hub's
data was first meant for Wikidata, and whether a new Data Staging instance prepares data for
Wikidata.

Source of truth: the PRD tab of the
[WD-to-WBC redirects KR doc](https://docs.google.com/document/d/19-P-3hexEFaAVdPDiIkTmukKjTjk52VZ4FQhiDvUcL0/edit?tab=t.g70l3gv8dssq)
("PRD - Track instances redirected from Wikidata to establish the baseline"), R1-R4.
The live component it reworks is
[`AudienceAndPurposeWizardStep.vue`](https://github.com/wbstack/ui/blob/main/src/components/Cards/AudienceAndPurposeWizardStep.vue).

Built to review behaviour and copy, not visual design.

## Decisions baked into this prototype

- **Copy comes from the PRD.** Option wording and bold phrases from R1, the follow-ups from R2
  and R3, and the "why we ask" line under the title from R4. The audience question is unchanged
  from the live dialog.
- **Follow-ups sit below the options** and appear only while their option is selected (R4). For
  Data Hubs the audience question comes first and the Wikidata question second, as R2 says.
- **Nothing is pre-selected, and every visible question is required** (R4). Validation runs on
  NEXT. The messages ("Please select an option.", "Please provide a response.", "Text must be 200
  characters or less.") and the 200-character counter are copied from the live component.
- **Hidden answers are dropped on NEXT, not on the switch.** R4 says changing the purpose discards
  answers to hidden follow-ups "as the audience question does today", and the live component
  clears them when the person presses NEXT. So switching to another option and straight back still
  shows the earlier answers; once NEXT is pressed, they are gone.
- **< PREVIOUS does nothing, and NEXT only validates.** Steps 1 and 3 are not part of this
  prototype (step 3 is `create-wiki-disclaimer`). Do not add a confirmation state or a next screen
  here.
- **The readout panel is prototype scaffolding, not product UI.** It sits outside the dialog,
  dashed and in the banner font, and shows how each answer is read internally: the category from
  R1 and the "Read as" value from R2. It updates live and leaves out hidden follow-ups, which is
  what makes the discard rule visible. The dialog itself never shows category names (R1).
- **No stored field names are shown.** The PRD fixes only that `data_lab` is not reused (R5); the
  values behind the radios in `index.html` are placeholders, not a schema proposal.

## Stack

Vue 2.7 + Vuetify 2.6 from CDN, single self-contained `index.html` - the same versions the live
dialog uses.
