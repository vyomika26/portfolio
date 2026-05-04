# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static HTML portfolio site for Vyomika Parikh (AI Product Designer), hosted on GitHub Pages at `vyomika26.github.io/portfolio`. No build system, no framework, no npm — plain HTML, CSS, and JS files served directly.

## File Structure

- `index.html` — homepage with hero, ticker, project cards, about section
- `habit-coach-ai.html`, `tata-motors.html`, `zs-dashboard.html`, `zs-design-system.html` — case study pages
- `assets/habit-coach/` — all Habit Coach case study media
- `assets/tata-motors/`, `assets/zs-dashboard/`, `assets/zs-design-system/` — per-project media
- `assets/shared/` — shared assets (e.g. `profile-pic.png` used across all pages)
- `assets/unused/` — archived/unused assets

## Styling Conventions

CSS custom properties defined in `index.html` and reused across pages:
- Colors: `--black`, `--white`, `--g1` through `--g6` (greyscale scale)
- Motion: `--ease`, `--ease-out`
- Font sizing: use `clamp()` for responsive type (e.g. `clamp(28px, 3.2vw, 46px)`)
- Scroll animations: `.reveal` class with `IntersectionObserver` — elements fade/slide in on scroll

## Development

No build step — open HTML files directly in a browser or use a local server:

```bash
# Simple local server (Python)
python3 -m http.server 9090

# Or use VS Code Live Server extension
```

To view changes: refresh the browser. No compilation, no hot reload needed.

---

## Git & Push Rules — READ BEFORE EVERY COMMIT/PUSH

### Always use Homebrew git, never Apple git

```bash
# Check which git you're using
which git          # should be /opt/homebrew/bin/git
git --version      # should be 2.50+ (Homebrew), NOT 2.39.2 (Apple)

# If wrong, fix for this session:
export PATH="/opt/homebrew/bin:$PATH"

# Or call explicitly:
/opt/homebrew/bin/git push origin main
```

Apple's bundled git 2.39.2 has an HTTP/2 bug that sends only 14 bytes of the pack file — GitHub rejects the push silently. Always use Homebrew git.

### Required git config (set once, persists)

```bash
git config http.postBuffer 524288000   # 500MB buffer for large binary pushes
git config http.version HTTP/1.1       # avoid HTTP/2 chunked transfer issues
```

Verify these are set before pushing large files:
```bash
git config --list | grep http
```

### NEVER use Git LFS

Git LFS is **incompatible with GitHub Pages**. When LFS is enabled, GitHub Pages serves the raw 133-byte pointer text file instead of the actual media — images and videos will appear broken on the live site.

**Before every push, verify LFS is not active:**
```bash
git config --list | grep lfs     # must return nothing
git lfs status 2>&1              # should say "not in a git repository" or similar
ls .git/hooks/ | grep lfs        # must return nothing
```

If you find LFS config remnants in `.git/config`, remove them:
```bash
git config --unset lfs.repositoryformatversion
git config --remove-section lfs.https://github.com/vyomika26/portfolio.git/info/lfs
```

### Push in batches — never push all large files at once

The remote will reset the connection if a single push is too large (~50MB+ is risky).

**Safe approach:**
1. Stage 2–4 media files at a time
2. Commit each batch
3. Push after each batch (or after 2–3 commits)
4. If a push fails mid-way, retry — git is resumable and already-uploaded objects are skipped

```bash
# Example batch push workflow
git add assets/habit-coach/video1.mp4 assets/habit-coach/image1.png
git commit -m "Add habit coach hero media"
/opt/homebrew/bin/git push origin main

git add assets/habit-coach/video2.mp4 assets/habit-coach/image2.png
git commit -m "Add habit coach core experience media"
/opt/homebrew/bin/git push origin main
```

---

## Media Rules — READ BEFORE ADDING ANY MEDIA FILE

### Always use .mp4, never .mov

`.mov` (QuickTime) files only play in Safari. Chrome and Firefox on GitHub Pages receive `Content-Type: video/quicktime` and refuse to play. The live site will have broken videos for most users.

**Before committing any video:**
```bash
file assets/path/to/video.mp4    # should say "ISO Media, MP4 Base Media"
```

If you have a `.mov` file, convert it first:
```bash
ffmpeg -i input.mov -c:v libx264 -preset fast -crf 22 -c:a aac -movflags +faststart output.mp4
```

Then verify the output plays in Chrome before committing.

### Check file sizes before staging

```bash
du -sh assets/**/*     # see sizes of all assets
ls -lh assets/path/    # check specific folder
```

Aim to keep individual video files under 20MB where possible. Large files slow pushes and page load.

### HTML video elements

Always include both type attributes and a fallback:
```html
<video autoplay muted loop playsinline>
  <source src="assets/folder/video.mp4" type="video/mp4">
</video>
```

Never use `type="video/quicktime"` — use `type="video/mp4"` for all `.mp4` files.

---

## Pre-Commit Checklist

Before staging and committing:

- [ ] All new video files are `.mp4` (not `.mov`)
- [ ] Video files verified with `file` command — "ISO Media, MP4 Base Media"
- [ ] `git config --list | grep lfs` returns nothing
- [ ] Using `/opt/homebrew/bin/git` (not Apple git)
- [ ] `http.postBuffer` set to `524288000`
- [ ] Planning to push in batches if adding multiple large files
- [ ] Tested media in Chrome (not just Safari) on localhost

## Pre-Push Checklist

- [ ] No LFS hooks in `.git/hooks/`
- [ ] Batch size is reasonable (< ~50MB of new binary content per push)
- [ ] `git config http.version` is `HTTP/1.1`

---

## GitHub Pages Constraints

- No server-side processing — everything must work as static files
- No build step on deploy — what's in the repo is what gets served
- Git LFS **does not work** — GitHub Pages cannot resolve LFS pointers
- Large files pushed correctly as real git objects will be served correctly
- Changes are live within ~1–2 minutes of a successful push to `main`
- Check the live site at `https://vyomika26.github.io/portfolio` after pushing

## Known Issues & How They Were Solved

| Problem | Root Cause | Fix |
|---|---|---|
| Push sends only 14 bytes | Apple git 2.39.2 HTTP/2 bug | Use `/opt/homebrew/bin/git` |
| Push fails "unable to rewind rpc post data" | `http.postBuffer` too small | `git config http.postBuffer 524288000` |
| GitHub Pages shows broken images/videos | Git LFS was enabled | Remove LFS entirely; store as real git objects |
| Videos broken on Chrome/Firefox | `.mov` files, `Content-Type: video/quicktime` | Convert to `.mp4` with ffmpeg |
| Push resets mid-way | Too many large files in one push | Push in batches of 2–4 files |
| 0-byte pack generated | Stale LFS config in `.git/config` | Remove LFS config entries manually |
