# Planner — standalone web app

A single-file planner (`index.html`) with buckets, week board, day planner and overdue views; add / edit / delete / tick tasks and projects; week and month navigation; and a Sync button that keeps every device on the same data through a JSON file in your GitHub repo. No build step, no server, no framework.

## 1. Host it on GitHub Pages (5 minutes)

1. On GitHub, create a **private** repository, e.g. `planner`. (Private repos can serve GitHub Pages on paid plans; on a free plan Pages requires a public repo — in that case keep the data file in a *separate* private repo, see step 2.)
2. Upload `index.html` to the repo root (Add file → Upload files → Commit).
3. Repo → Settings → Pages → Source: **Deploy from a branch** → Branch `main`, folder `/ (root)` → Save.
4. After a minute the site is live at `https://<your-username>.github.io/planner/`. Open it on your phone and use "Add to Home Screen" for an app-like icon.

## 2. Turn on sync

The app stores `planner-data.json` in a GitHub repo through the GitHub Contents API, using a token that lives only in the browser on each device.

1. Create a token: GitHub → Settings → Developer settings → Personal access tokens → **Fine-grained tokens** → Generate. Repository access: **only** the repo that will hold the data file. Permissions: **Contents: Read and write**. Set an expiry (90 days is sensible) — the app will tell you when it's rejected.
2. In the app tap **Sync** → fill in owner, repository, branch (`main`), file path (`planner-data.json`) and paste the token → **Save and sync**. The first sync creates the file.
3. Repeat step 2 on each device (same token is fine). From then on tap **Sync** whenever you want to push/pull; the app also syncs on load.

Data repo options:
- Same repo as the site (simplest; only sensible if that repo is private).
- A separate private repo for the data while the site repo is public (recommended on a free plan).

## 3. Using it

- **Buckets** — one tile per project with this week's progress. Tap a tile to see all of that project's tasks.
- **Board** — this week's tasks grouped by project.
- **Planner** — Monday to Sunday with tasks under each day, plus anything undated.
- **Overdue** — everything past its date.
- **+ Task** — title, project, due date, priority, notes. Tap any task to edit or delete it; tick the box to complete.
- **☰** — add, rename, recolour or delete projects; export JSON, import JSON (merges), or **Export for Obsidian** (one Markdown block per project in the vault's task format, so you can paste into `Projects/` if you run both systems).
- **↻** — jump back to the current week.

## 4. How sync resolves conflicts

Each project and task carries an `updatedAt` timestamp; on sync the app pulls the remote file, keeps the newer version of each record, honours deletions, then pushes the merged result. Two devices editing different tasks never collide. If both edit the *same* task while offline, the later edit wins.

## 5. Where it differs from the Obsidian kit

| | Obsidian kit | This app |
|---|---|---|
| Data | Markdown notes you own, readable by the AI agent | JSON in your GitHub repo |
| Multi-device | Obsidian Sync | GitHub sync button |
| Agent integration | Native — reads the vault | Indirect — agent can read `planner-data.json` from the repo, or you paste the Obsidian export |
| Rich notes, links, calendar reasoning | Yes | No — tasks only |
| Works without any app installed | No | Yes, any browser |

## Security notes

- The token is stored in the browser's local storage on each device. Anyone with access to that browser profile can use it against that one repo. Use a fine-grained token scoped to a single repo, set an expiry, and use **Forget token** on shared devices.
- The site itself contains no secrets; only the data file and the token matter.
