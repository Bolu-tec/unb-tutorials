# Authoring Course Tutorials for the UNB IDE

This guide provides end-to-end instructions for course instructors, instructional designers, and junior developers creating and publishing interactive tutorials in the UNB IDE.

Every course collection in the UNB IDE repository is structured as a **MakeCode extension**, with tutorials authored as Markdown files within a dedicated `lessons/` folder.

---

## 1. Design the Course Folder Structure (Course as an Extension)

Each course in `unb-tutorials` is structured as a self-contained extension package so that students and instructors can import the entire course into the editor.

### Folder Layout

For a course named `eng1102`, the directory tree must be structured as follows:

```text
eng1102/
├── pxt.json                  # Course extension manifest
├── main.ts                   # Empty authoring program entry point
├── test.ts                   # Course extension test file
├── catalog.md                # Home-page gallery cards registration
├── README.md                 # Course overview and instructor documentation
├── lessons/                  # Directory containing all lesson tutorial Markdown files
│   ├── getting-started.md    # Interactive lesson 1
│   ├── micro-led.md          # Interactive lesson 2
│   └── main.blocks           # Optional default starter workspace
└── static/                   # Card icons and media assets
    └── getting-started.svg   # Lesson card image referenced in catalog.md
```

### Manifest Contract (`pxt.json`)

The `pxt.json` manifest must reside at the root of the course folder. It defines the extension name, target dependencies, and lists all files that comprise the course package:

```json
{
    "name": "eng1102",
    "description": "UNB IDE ENG 1102 course extension and interactive lessons",
    "dependencies": {
        "core": "*",
        "radio": "*",
        "microphone": "*",
        "unbdev-board": "github:UNB-ECE/pxt-unbdev-board#22ed7d76838be66d456de13efc1604b1037dbefe"
    },
    "files": [
        "README.md",
        "catalog.md",
        "main.ts",
        "lessons/getting-started.md",
        "lessons/micro-led.md"
    ],
    "testFiles": [
        "test.ts"
    ],
    "targetVersions": {
        "target": "9.1.1",
        "targetId": "microbit"
    },
    "supportedTargets": [
        "microbit"
    ],
    "preferredEditor": "tsprj"
}
```

> [!IMPORTANT]
> The starter files (`pxt.json`, `main.ts`, and `test.ts`) must reside at the **root of the course folder**, while all tutorial Markdown files reside under `lessons/`. This ensures the course can be imported both as an authoring project and as a package dependency.

---

## 2. Fork or Sync the Tutorials Repository

Start from your own fork of [UNB-ECE/unb-tutorials](https://github.com/UNB-ECE/unb-tutorials). If you already forked the repository, sync your fork with the upstream repository before you start editing. If you do not have a fork yet, create one first. The screenshots below show the GitHub website; your buttons may look slightly different as GitHub updates its interface.

### If you have not forked the repository yet

1. Open the canonical repository and click **Fork**.

   ![UNB-ECE/unb-tutorials repository page with the Fork button](screenshots/01-tutorials-repo-fork-button.jpg)

   *Figure 1. Start a fork from the upstream repository.*

2. Choose your GitHub account as the owner, enter a repository name (for example, `unb-tutorials-fork`), and click **Create fork**. Keeping **Copy the main branch only** selected is usually sufficient.

   ![GitHub create-fork form showing owner, repository name, and Create fork button](screenshots/02-create-fork-form.jpg)

   *Figure 2. Set the fork's owner and name before creating it.*

### If you already have a fork

1. Open your fork on GitHub and select its `main` branch.
2. Click **Sync fork** and review GitHub's status and available update action.

   ![An existing unb-tutorials fork with the Sync fork button](screenshots/03-existing-fork-sync-button.jpg)

   *Figure 3. Find the Sync fork control on your fork's main branch.*

3. If GitHub offers to update your branch, review the changes and proceed. If GitHub reports conflicts or offers to discard commits, do not discard your work blindly; resolve the conflict safely or ask a maintainer for help.
4. After syncing, you can edit on `main` if you have permission and that suits your workflow. Otherwise, create a topic branch from the updated `main` (for example, `author-eng1102-lessons`) and use a pull request.

### Command Equivalent

```bash
# Fork the repository via GitHub CLI
gh repo fork UNB-ECE/unb-tutorials --fork-name unb-tutorials-fork --clone

# Navigate into your fork
cd unb-tutorials-fork

# If this is an existing clone, update main from your fork before editing
git checkout main
git pull origin main

# Optional: use a topic branch for pull-request work
git checkout -b author-eng1102-lessons
git push -u origin author-eng1102-lessons
```

For a direct commit, make sure you are authorized to push to the selected branch and that branch protection permits it.

---

## 3. Generate a GitHub Developer Token with Required Scopes

To connect the UNB IDE to your GitHub repository and commit changes from within the browser, generate a GitHub Personal Access Token (PAT).

### Required Token Scopes
The token must carry two explicit scopes:
1. **`repo`** (Full control of private repositories and public repositories): Required to read repo trees, create commits, and push branches.
2. **`read:org`** (Read org and team membership): Needed for organizational repositories and fork contribution workflows.

### Token Creation Steps
1. Navigate directly to the GitHub token creation page:
   [https://github.com/settings/tokens/new?description=UNB-MakeCode&scopes=repo,read:org](https://github.com/settings/tokens/new?description=UNB-MakeCode&scopes=repo,read:org)
2. Ensure both **`repo`** and **`read:org`** checkboxes are checked.
3. Set an expiration period (e.g., 30 or 90 days).
4. Click **Generate token** and copy the resulting string (starts with `ghp_`).

> [!WARNING]
> Never commit your personal access token to Git or include it in screenshots. The UNB IDE stores your token locally in browser storage (`localStorage`).

---

## 4. Connect & Import the Course into UNB IDE

1. Open the UNB IDE in your browser: [https://d200xhrnashxy5.cloudfront.net](https://d200xhrnashxy5.cloudfront.net).
2. On the Home page, click **Import**, then select **Your GitHub Repo...**.
3. When prompted by the **Sign in with GitHub** modal:
   - Paste your developer token into the token field.
   - Click **Sign in**.
4. In the repository picker:
   - Select your fork: `Joseph-Xu-Unb/unb-tutorials-fork`.
   - Select your branch: `author-eng1102-lessons`.
   - Select your course folder: `eng1102` (or `engg1102`).
5. Click **Import**.
6. Switch to the **JavaScript** / Monaco editor and expand the **Explorer** sidebar. Confirm that `pxt.json`, `main.ts`, `test.ts`, `catalog.md`, `README.md`, and the `lessons/` folder are displayed.

---

## 5. Author the Lesson Markdown

Tutorial lessons are written in standard MakeCode tutorial Markdown. See [MakeCode's user tutorial guide](https://makecode.com/writing-docs/user-tutorials) for the complete format and publishing options.

### Markdown Structure
Create your lesson file in `lessons/<lesson-name>.md` (e.g. `lessons/getting-started.md`):

````markdown
# Getting started with UNB IDE

## Introduction @unplugged

Welcome to ENG 1102! In this tutorial, you will write a program that displays a greeting, shows an icon when button A is pressed, and clears the screen when button B is pressed.

## Step 1: Display a greeting

Place a ``||basic:show string||`` block inside the ``||basic:on start||`` block to say hello.

```blocks
basic.showString("Hello UNB!")
```

## Step 2: Button A handler

Add an ``||input:on button A pressed||`` block and place a ``||basic:show icon||`` block inside it to draw a heart.

```blocks
input.onButtonPressed(Button.A, function () {
    basic.showIcon(IconNames.Heart)
})
```

## Step 3: Button B handler

Add an ``||input:on button B pressed||`` block and place a ``||basic:clear screen||`` block inside it.

```blocks
input.onButtonPressed(Button.B, function () {
    basic.clearScreen()
})
```

## Completion

Congratulations! You have completed the first UNB IDE activity. Download your code to the micro:bit or run it in the simulator.
````

### Manifest Registration
Add your new lesson file path to the `files` array in `pxt.json`:

```json
"files": [
    "README.md",
    "catalog.md",
    "main.ts",
    "lessons/getting-started.md"
]
```

### Include the UNBdev board extension in a tutorial

When a lesson uses custom UNBdev board blocks, add a `package` fence at the end of that lesson's Markdown. Use the approved, pinned extension revision so the tutorial preview and the learner's project load the same blocks:

````markdown
```package
unbdev-board=github:UNB-ECE/pxt-unbdev-board#22ed7d76838be66d456de13efc1604b1037dbefe
```
````

Keep the package name (`unbdev-board`) and repository spelling as shown. Replace the commit only when maintainers approve a different revision; do not use a floating branch such as `main`. If the course extension itself needs the board package while it is open in the editor, also declare the same pinned dependency in that course's `pxt.json` `dependencies` object:

```json
"dependencies": {
    "core": "*",
    "unbdev-board": "github:UNB-ECE/pxt-unbdev-board#22ed7d76838be66d456de13efc1604b1037dbefe"
}
```

The Markdown `package` fence declares what the tutorial needs when it is previewed or opened. The `pxt.json` entry declares what the course extension needs when the course project is imported. Add both when the lesson and the course project both use the board extension.

---

## 6. Preview and Test the Tutorial

1. In the **Explorer** sidebar, locate `lessons/getting-started.md`.
2. Click the **flask (Preview)** icon beside the file name. The tutorial will launch in a preview modal directly in the IDE.
3. Test each step:
   - Verify that instructions render clearly.
   - Click the lightbulb hint icon on each step to verify hint blocks display properly.
   - Check that block code compiles and runs in the simulator without errors.
4. For isolated authoring and debugging, open the [UNB IDE Tutorial Tool](https://d200xhrnashxy5.cloudfront.net/tutorialtool.html), paste your Markdown, and check the step-by-step preview. Package fences are loaded by the tool, so confirm that UNBdev board blocks appear in the toolbox and that each step's blocks compile and run in the simulator.
5. You can also use the MakeCode workflow described in [User tutorials: Testing](https://makecode.com/writing-docs/user-tutorials#testing): open the Markdown file in the Explorer with the flask/lab icon to preview it as a tutorial. When previewing a GitHub-hosted draft, use a fresh private/incognito window if an older cached copy appears. MakeCode may cloud-cache GitHub-hosted tutorial content; its guide documents release-based cache refresh for that workflow.

---

## 7. Commit and Save Changes via UNB IDE GitHub View

1. Click the **GitHub** button in the bottom editor toolbar (`#githubArea`).
2. If prompted to sign in, use your developer token.
3. The GitHub dashboard displays changed files under the diff list.
4. Enter a descriptive commit message in the **Commit message** box (e.g., `feat(eng1102): add getting started lesson`).
5. Click **Commit changes**.
6. Click **Push changes** (or use the push icon in the header) to push the commit to your fork (`Joseph-Xu-Unb/unb-tutorials-fork`).

### Command Equivalent

```bash
git add eng1102/lessons/getting-started.md eng1102/pxt.json
git commit -m "feat(eng1102): add getting started lesson"
git push origin author-eng1102-lessons
```

---

## 8. Register the Home-Page Card in `catalog.md`

To make your lesson accessible from the UNB IDE home page gallery, register a codecard in `eng1102/catalog.md`.

### Card Specification
Open `eng1102/catalog.md` and add your card object inside the ````codecard JSON array:

```markdown
# ENG 1102

Introductory course lessons and lab activities.

## Activities

```codecard
[
  {
    "name": "Getting started with UNB IDE",
    "description": "Display a greeting, show a heart with button A, and clear the screen with button B.",
    "url": "/unb-tutorials/eng1102/lessons/getting-started",
    "cardType": "tutorial",
    "imageUrl": "/unb-tutorials/eng1102/static/getting-started.svg",
    "label": "Lab 1",
    "labelClass": "blue ribbon"
  }
]
```
```

> [!IMPORTANT]
> The `url` field must begin with `/unb-tutorials/<course>/lessons/<lesson-name>` (without the `.md` extension). Never use `/projects/`.

### Run Local Validator
Before opening a pull request, run the repository validator to ensure all card URLs, categories, and paths resolve cleanly:

```bash
# Run validator against the repository
node scripts/validate.mjs

# Expected output:
# Content OK: 2 collection(s) in /path/to/unb-tutorials
```

---

## 9. Propose Publication via Pull Request

You can open a pull request either from the **GitHub dashboard in the UNB IDE** or from the **GitHub website**. Both routes use GitHub's **Contribute** flow to propose changes from your fork to `UNB-ECE/unb-tutorials`.

1. **From the UNB IDE:** Open the GitHub dashboard for your project. Confirm the repository and branch shown near the top are the fork and branch you intend to submit, then click **Contribute** and choose **Open pull request**.
   ![UNB IDE Github dashboad showing the Contribute option and Open pull request action](screenshots/04-contribute.png)
    *Figure 4. Start the contribution flow from the UNB IDE GitHub dashboard.*
2. **From the GitHub website:** Open your fork's repository page, click **Contribute**, then choose **Open pull request**.

3. Verify PR targets:
   - **Base repository:** `UNB-ECE/unb-tutorials`, branch: `main`
   - **Head repository:** `Joseph-Xu-Unb/unb-tutorials-fork`, branch: `author-eng1102-lessons`
4. Enter PR Title: `feat(eng1102): add getting-started interactive lesson`.
5. Submit the pull request. All automated checks (`scripts/validate.mjs`) will run in CI.

### Command Equivalent

```bash
gh pr create \
  --repo UNB-ECE/unb-tutorials \
  --base main \
  --head Joseph-Xu-Unb:author-eng1102-lessons \
  --title "feat(eng1102): add getting-started interactive lesson" \
  --body "Adds getting-started tutorial for ENG 1102 under the lessons/ layout."
```

---

## 10. Verify Deployed Home-Page Publication

Once the pull request is reviewed and merged into `UNB-ECE/unb-tutorials:main`:

1. Open the deployed UNB IDE: [https://d200xhrnashxy5.cloudfront.net](https://d200xhrnashxy5.cloudfront.net).
2. Scroll to the **ENG 1102** gallery section.
3. Locate the **Getting started with UNB IDE** card.
4. Click the card:
   - The editor must launch the interactive tutorial directly.
   - It must **not** return a 404 or fall back to the default editor shell (`index.html`).

---

## 11. Post-Merge Cleanup: Sync Your Fork and Delete the Topic Branch

After the pull request is reviewed and merged into `UNB-ECE/unb-tutorials:main`, clean up your fork before starting the next lesson. Do this after **every** merge; it prevents the accumulated-diff problem that produced the noisy earlier fork pull requests.

### 11.1 Sync your fork's `main`

1. In the UNB IDE GitHub dashboard, click **Sync Fork**; or, on GitHub.com, open your fork and choose **Sync fork → Update branch**.
2. Confirm that your fork's `main` matches `UNB-ECE/unb-tutorials:main` and that the merge commit for your pull request is present.

After the pull request is merged, sync your fork with upstream so it has the latest lessons and catalog content.

### 11.2 Delete the completed topic branch

- Delete the topic branch on your fork. GitHub offers **Delete branch** on the merged pull request page; otherwise use the fork's **Branches** page.
- Delete the local branch and update `main`:

```bash
git checkout main
git pull
git branch -d author-eng1102-lessons
```

### 11.3 Start the next lesson

Continue on `main` when your repository permissions and contribution workflow allow it. For changes that need review, create a topic branch from the latest `main` and open a pull request. You can use the same approach in a fork or in the shared repository.

```bash
git checkout main
git pull
git checkout -b author-eng1102-lesson-2
```
