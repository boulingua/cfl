# cfl — Chinesisch

A Boulingua course by S. Le Boulanger for school teaching and independent study.
The interface and task instructions are German; the target language is `zh`.

**Development site:** the Hugo site and curriculum declaration are implemented.
No teaching units, examinations or recordings are published in this repository.
The existing [roadmap](ROADMAP.md) defines the intended teaching sequence.

## Intended learning experience

Units follow Activate → Input → Practise → Apply → Reflect, with differentiated
activities, solutions and teacher notes. Reviewed units will have committed PDF
slide decks and worksheets, accessible transcripts, and audio where an approved
voice or documented human recording is available. Shared implementation lives
in [kit](https://github.com/boulingua/kit); the retired generators are historical.

The manifest [curriculum.yml](curriculum.yml) declares a target, with
`conformance_status: in-progress`. It claims no achieved descriptor coverage.

Target-script enablement is deferred under organisation ADR-0018. The current German development shell uses the core font tier and contains no target-script teaching content.

## Build and preview

Install Git, Hugo **extended 0.159.2**, and Go **1.26.1**. From this repository:

```sh
hugo --minify --gc --panicOnWarning
hugo server --disableFastRender
```

Open the local URL printed by Hugo. Module versions are pinned in `go.mod` and
`go.sum`. The refinement workspace uses a local matching kit override; see
`docs/refinement-2026-10-30.md` in the organisation repository for its status.

To run course gates with sibling `kit`, `curriculum` and `.github` checkouts,
install `kit/requirements.txt` into a Python 3.12 virtual environment and use
Node 22, then run:

```sh
npm ci --prefix ../kit
npm exec --prefix ../kit -- playwright install chromium
python ../kit/bin/kit check . --owner-root curriculum=../curriculum --owner-root .github=../.github
```

A successful site build is not a teaching release. Native-language review,
unit authoring, appropriate materials, meaningful assessment and an achieved
curriculum claim remain separate milestones. See [the refinement record](REFINEMENT.md).

## Licensing

Code: MIT, see [LICENSE](LICENSE). New course content: CC BY-SA 4.0 under the
suite's content policy. Third-party material retains its own source and licence
and must be documented separately. No copyrighted source is made freely
redistributable merely by inclusion in this repository.
