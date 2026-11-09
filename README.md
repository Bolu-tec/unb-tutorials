# UNB IDE tutorials

This repository is the content source for the [UNB IDE](https://d200xhrnashxy5.cloudfront.net): the interactive course lessons, UNBdev.board guides, and home-page gallery catalogs that students and instructors see in the editor.

Content is delivered independently of editor releases. Merging a lesson or catalog card into `main` publishes it to the IDE — no editor rebuild or target change is required.

## What's in the repository

| Collection | Folder | Contents | Publication status |
| --- | --- | --- | --- |
| UNBDev Board Guide | [`unbdev-board/`](unbdev-board/README.md) | BLiXel light patterns, microphone, motor control, and Wi-Fi/MQTT lessons for the integrated UNBdev.board | Instructor previews. BLiXel hardware test pending; microphone, motors, and Wi-Fi/MQTT hardware unverified. Motor operation is software-only and not authorized. |
| ENGG 1102 | [`engg1102/`](engg1102/README.md) | Introductory "Getting started" and micro LED lessons | Instructor previews. Course-instructor approval pending. |

Both collections are early-stage authoring material, not certified student lessons. Each card carries a label describing its validation state; keep that label until the stated review is complete. See each collection's `README.md` for its specific evidence and open issues.

## Repository layout

A **collection** is any top-level folder containing a `catalog.md`. There is no central registry: `scripts/validate.mjs` discovers collections by scanning the repository root.

```text
unb-tutorials/
├── README.md
├── docs/
│   ├── tutorial-development-guide.md   # End-to-end authoring and publication guide
│   └── screenshots/                    # Images used by the guide
├── scripts/
│   ├── validate.mjs            # Content contract validator (run in CI)
│   └── selftest.mjs            # Checks the validator against test/fixtures
├── test/fixtures/              # Miniature repositories used by the validator self-test
├── unbdev-board/               # Collection: UNBDev Board Guide
│   ├── pxt.json                # Extension manifest and file list
│   ├── catalog.md              # Home-page gallery cards
│   ├── README.md               # Collection overview and hardware status
│   ├── microphone-validation.md
│   ├── motor-validation.md
│   ├── lessons/                # Tutorial Markdown files
│   └── static/                 # Card icons and lesson media
└── engg1102/                   # Collection: ENGG 1102 (same layout)
```

Each collection is a self-contained MakeCode extension package, so the entire collection can be imported into the editor. A collection normally contains:

- `pxt.json` — extension manifest and file list
- `main.ts`, `test.ts` — starter program and tests
- `catalog.md` — the home-page gallery cards
- `README.md` — collection overview, maintainer, and validation status
- `lessons/` — one Markdown tutorial per card
- `static/` — card icons and lesson images

## How content is published

The editor reserves the `/unb-tutorials` path prefix, and the validator uses it for every collection. A collection's `catalog.md` supplies the home-page cards; each card's `url` points at a lesson in that same collection, using the reserved prefix and omitting the `.md` extension:

```text
/unb-tutorials/<collection>/lessons/<lesson-name>
```

A card may only be added once its lesson exists. An `imageUrl`, when present, must resolve under the same collection's `static/` directory.

## Validate before you publish

The content contract is enforced by `scripts/validate.mjs`. It checks that:

- at least one top-level folder contains a `catalog.md`, and its name is a lowercase slug
- each `catalog.md` has a `# Title` heading, at least one `## Category` heading, and exactly one `codecard`-fenced JSON array
- every card has `name`, `url`, and a supported `cardType`
- every card `url` and `imageUrl` begins with `/unb-tutorials/<collection>/` and resolves to a real file inside that collection
- every lesson Markdown file has a `# Title` heading

Run both checks locally before opening a pull request:

```bash
node scripts/validate.mjs
node scripts/selftest.mjs
```

The same two commands run in CI on every pull request and on every push to `main` (`.github/workflows/validate.yml`). Because the merge is the publication, a failing check blocks a bad card or dangling lesson from reaching students.

To roll back a published change, open a pull request that reverts the merge commit; do not rewrite repository history:

```bash
git revert -m 1 <merge-commit>
```

## Authoring a lesson

Start with the [tutorial development guide](docs/tutorial-development-guide.md). It covers the full workflow end to end:

1. Structure the course folder as a MakeCode extension.
2. Fork or sync the repository and create a topic branch for the change.
3. Generate a GitHub token with the `repo` and `read:org` scopes.
4. Import the course into the UNB IDE.
5. Write the lesson Markdown, including the pinned UNBdev.board `package` fence when needed.
6. Preview and test with the IDE and the UNB IDE Tutorial Tool.
7. Commit and push from the IDE's GitHub view.
8. Register the card in the collection's `catalog.md`.
9. Open a pull request against `UNB-ECE/unb-tutorials:main`.
10. Verify the published card after merge.
11. Sync your fork and delete the topic branch.

A few rules that apply to every change:

- Keep each collection in its own top-level folder; adding a lesson to one collection must not alter another.
- Pin extension dependencies and lesson `package` fences to a reviewed commit, never a floating branch such as `main`.
- Do not remove a card's status label to make an activity look ready.
- A catalog card is not evidence that hardware has been tested; physical validation needs separate board, firmware, program, and outcome evidence.
