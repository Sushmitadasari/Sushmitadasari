# GitHub Jet Heatmap

An animated jet flies over a GitHub-style contribution grid and "hits" your
busiest days with a bullet + flash + blast effect. The grid colors and
targeted days are pulled from your **real** GitHub contribution calendar.

## Setup (one-time)

1. **Put these files in a repo GitHub Actions can push to.**
   The simplest option is your special profile repo — the one named exactly
   the same as your username (e.g. `github.com/yourname/yourname`), since
   that's the repo whose README shows on your profile.

   Copy into that repo:
   - `generate.mjs`
   - `package.json`
   - `.github/workflows/jet-heatmap.yml`

2. **No extra secret needed for your own public contributions.** The workflow
   uses the built-in `GITHUB_TOKEN`, which is enough to read public
   contribution calendars via the GraphQL API.

   > If you ever want to show a **private** contribution count (the "include
   > private contributions" toggle on your GitHub profile), you'll need a
   > personal access token with `read:user` scope instead, saved as a repo
   > secret (e.g. `GH_PAT`), and referenced in the workflow in place of
   > `secrets.GITHUB_TOKEN`.

3. **Enable Actions write permissions.** In the repo:
   `Settings -> Actions -> General -> Workflow permissions` -> select
   "Read and write permissions".

4. **Run it once manually** to generate the first version:
   `Actions` tab -> "Update jet heatmap SVG" -> "Run workflow".
   This creates `dist/github-jet.svg` and commits it.

5. **Embed it in your README:**

   ```md
   ![GitHub jet heatmap](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_USERNAME/main/dist/github-jet.svg)
   ```

   Replace `YOUR_USERNAME` in both places, and swap `main` for your default
   branch name if different.

That's it — the Action re-runs daily and repaints the grid with fresh data.

## Running it locally

```bash
npm install     # nothing to install really, but sets things up
GH_USERNAME=yourname GH_TOKEN=ghp_yourtoken node generate.mjs
```

The token just needs to be a valid GitHub token (a classic PAT with no
special scopes works, since contribution calendars are public data).

## Customizing

Open `generate.mjs` — the constants near the top control everything:

- `MAX_TARGETS` — how many of your busiest days get the bullet/blast effect
- `LOOP_DUR` — how long one flight (there and back) takes, in seconds
- `FLASH_COLOR`, `BULLET_COLOR`, `BLAST_COLOR` — the accent colors
- `COLS` / `ROWS` — grid size (34x7 matches GitHub's own layout; changing
  this changes how many weeks of history are shown)
