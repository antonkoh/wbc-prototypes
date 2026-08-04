# Wikibase Cloud prototypes

Static HTML prototypes built for product discovery on [Wikibase Cloud](https://www.wikibase.cloud/),
the managed hosting platform from [Wikimedia Deutschland](https://www.wikimedia.de/).

These are throwaway, single-page concept demos used to test ideas with users and
stakeholders - not production code.

## Prototypes

| Folder | What it explores | Live |
|---|---|---|
| [`license-declaration/`](license-declaration/) | A footer that declares the data license / copyright of a Wikibase instance, to encourage reuse. | [link](https://majestic-sundae-48e4ed.netlify.app) |
| [`reuse/`](reuse/) | An "intended for reuse" treatment surfacing how an instance's data can be reused. | - |
| [`create-wiki-disclaimer/`](create-wiki-disclaimer/) | The last step of the creation dialog, telling the person their Wikibase is temporary by default and asking them to confirm they understood. | [link](https://wbc-temporary-by-default.netlify.app) |
| [`wiki-profile/`](wiki-profile/) | The manager's side of the hosting policy review: describing the use case in the wiki profile, and submitting the instance for review. | [link](https://wbc-hp-submissions.netlify.app) |
| [`hosting-policy-admin/`](hosting-policy-admin/) | The review committee admin view: triage submissions, change their status, set or remove scheduled suspension dates, record decision explanations, and suspend or unsuspend instances out of process. | [link](https://bucolic-pegasus-82a9af.netlify.app) |

Each is a self-contained `index.html`, sometimes with an `assets/` folder. Open
`index.html` in a browser to view.

The three hosting-policy prototypes carry their own `README.md` recording what the
prototype demonstrates and the decisions baked into it. Read that before changing one -
several details that look arbitrary are not.

## Live versions

Deployment to Netlify is handled separately from this repo.
