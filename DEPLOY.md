# Jobbudy Prototypes — deploy notes

Three clickable discovery prototypes (V1 Reasoning-Kompass · V2 Anschreiben-Werkstatt · V3 Ein-Ort-Workspace) + a hub `index.html`. Static HTML, no build, no backend.

**Verified 2026-06-16:** hub + all three render over HTTP with **zero console errors**. **No localStorage/backend → each visitor's browser is fully isolated** ⇒ safe for many concurrent candidates (remote, moderated). Only external dependency is Google Fonts.

## Preview locally
- Config `jbproto` in `~/.claude/launch.json` serves this folder on :8766, or:
- `python3 -m http.server 8766 --directory ~/jobbudy-prototypes` → open http://localhost:8766/

## Publish to GitHub Pages (MohamedAliGH/jobbudy)

Keep the prototypes **out of the app's way** — they will share the repo with the Epic-0 scaffold later. Recommended: put them under `prototypes/` and serve Pages from there, or use a dedicated branch.

**Web UI (no token):**
1. In the `jobbudy` repo → **Add file → Upload files** → drag this folder's contents into a new `prototypes/` path → commit.
2. **Settings → Pages →** Source = `Deploy from a branch`, branch = `main`, folder = `/ (root)` (or `/docs`). Save.
3. Live at: `https://mohamedaligh.github.io/jobbudy/prototypes/` → V1 at `…/prototypes/V1-Reasoning-Kompass/`, etc.

**Or git (needs auth):**
```bash
git clone https://github.com/MohamedAliGH/jobbudy.git
cp -R ~/jobbudy-prototypes jobbudy/prototypes
cd jobbudy && git add prototypes && git commit -m "add discovery prototypes for user testing" && git push
# then enable Pages in repo Settings
```

`.nojekyll` is included so Pages serves all files verbatim (no Jekyll processing).

## GDPR note (test-time)
Google Fonts is loaded from Google's CDN → a participant's IP reaches Google on load. Low-risk for a throwaway moderated test (covered by session consent), but the **production app self-hosts fonts** (documented German GDPR posture). If you want zero third-party calls even for the test, self-host the fonts in these prototypes too — say the word.
