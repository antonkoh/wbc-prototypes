# create-wiki-purpose

Step 2 of the "Create a wiki" dialog, before and after the rework. Left: the live dialog today.
Right: the reworked step, with purpose options aligned to the hosting-policy categories and two
new follow-ups that tell us whether a new Data Hub's data was first meant for Wikidata, and
whether a new Data Staging instance prepares data for Wikidata.

Source of truth for the "after" pane: the PRD tab of the
[WD-to-WBC redirects KR doc](https://docs.google.com/document/d/19-P-3hexEFaAVdPDiIkTmukKjTjk52VZ4FQhiDvUcL0/edit?tab=t.g70l3gv8dssq)
("PRD - Track instances redirected from Wikidata to establish the baseline"), R1-R4.
The "before" pane reproduces the live component,
[`AudienceAndPurposeWizardStep.vue`](https://github.com/wbstack/ui/blob/main/src/components/Cards/AudienceAndPurposeWizardStep.vue).

Built to review behaviour and copy, not visual design.

## Decisions baked into this prototype

- **Two independent panes.** Each dialog keeps its own answers and validates on its own NEXT. The
  "Before" and "After" labels above them are prototype scaffolding, not product UI. On narrow
  screens the panes stack, before above after.
- **"Before" copies the live dialog**: options, bold phrases, the audience follow-up for Data Hubs,
  validation messages and the NEXT behaviour of `nextStep()`.
- **"After" copy comes from the PRD.** Option wording and bold phrases from R1, the follow-ups from
  R2 and R3. The audience question is unchanged from the live dialog.
- **The "why we ask" line under the title deviates from the PRD's example on purpose.** Anton chose
  "We ask to better understand why people create Wikibases. Your answers don't change how your
  Wikibase works." (20261110); R4 gives "We ask to understand what people create Wikibases for" only
  as an example.
- **Follow-ups sit below the options** and appear only while their option is selected (R4). For
  Data Hubs the audience question comes first and the Wikidata question second, as R2 says.
- **Nothing is pre-selected, and every visible question is required** (R4). The messages ("Please
  select an option.", "Please provide a response.", "Text must be 200 characters or less.") and the
  200-character counter are copied from the live component.
- **Hidden answers are dropped on NEXT, not on the switch.** R4 says changing the purpose discards
  answers to hidden follow-ups "as the audience question does today", and the live component
  clears them when the person presses NEXT. So switching to another option and straight back still
  shows the earlier answers; once NEXT is pressed, they are gone.
- **Red markup between the panes** names the internal category of each purpose option and points to
  it: Data Hub, Tool Lab and Test Drive to both panes, Data Lab only to "before", Data Staging and
  Classroom only to "after". It is scaffolding outside the product UI, drawn from the live layout,
  and hidden when the panes stack on narrow screens. Labels stack in the fixed order of the
  `MARKUP` list, which follows the option order of both panes at once, so no two lines cross. Keep
  that list in order if options are added or moved. Data Hub, Data Lab, Tool Lab and Test Drive sit
  level with their options; Data Staging sits halfway between Data Lab and Tool Lab, Classroom
  halfway between Tool Lab and Test Drive (Anton, 20261111). Markers: Data Lab (removed), Data
  Staging (new), Classroom (new).
- **"Reset prototype" sits in the banner**, as in `wiki-profile`: it clears both panes, answers and
  validation errors, and is scaffolding rather than product UI.
- **< PREVIOUS does nothing, and NEXT only validates.** Steps 1 and 3 are not part of this
  prototype (step 3 is `create-wiki-disclaimer`). Do not add a confirmation state or a next screen
  here.
- **No stored field names are shown.** The PRD fixes only that `data_lab` is not reused (R5); the
  values behind the "after" radios in `index.html` are placeholders, not a schema proposal.
- **Removed 20261110:** a prototype-only readout of how each answer is read internally. The
  before/after panes replaced it.

## Stack

Vue 2.7 + Vuetify 2.6 from CDN, single self-contained `index.html` - the same versions the live
dialog uses.
