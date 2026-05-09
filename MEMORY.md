# Project Memory

Working notes and context for Claude Code sessions on this repo.

---

## People

- **Vyomika Parikh** — AI Product Designer, this is her portfolio
- **Akshay Shah** — builds and maintains the site for her (`akshay.vjti11@gmail.com`)

---

## Reference Site

**https://www.vyomikaparikh.com** — Vyomika's original Wix portfolio.
Content is sourced from these Wix pages, but the **design language must match the portfolio site** (Inter, greyscale, habit-coach style) — NOT the Wix aesthetic.

| Local file | Reference URL |
|---|---|
| `zs-dashboard.html` | https://www.vyomikaparikh.com/zs-zaidyn |
| `zs-design-system.html` | https://www.vyomikaparikh.com/zs-design-system (TBD) |
| `habit-coach-ai.html` | https://www.vyomikaparikh.com/habit-coach (TBD) |
| `tata-motors.html` | https://www.vyomikaparikh.com/tata-motors (TBD) |

---

## Portfolio Design Language (apply to ALL case study pages)

`habit-coach-ai.html` is the canonical reference for look and feel.

- **Font:** Inter only (no Poppins)
- **Palette:** Greyscale base (`--black`, `--g1`–`--g6`, `--white`) + per-project accent used sparingly
- **Layout:** `cs-wrap` max-width 820px, editorial
- **Hero showcase:** full-bleed block with project accent bg color, image inside with border-radius + shadow
- **Sections:** numbered dividers (`01 — Section Name` + horizontal rule)
- **HMW block:** black bg container, italic text, contained (not full-bleed)
- **Impact cards:** dark black bg — NOT peach/colored
- **Footer:** minimal 2-col (name + links/social), NOT centered lavender
- **Lightbox:** yes, on wireframe/exploration images
- **Next project:** cs-next bordered card at bottom

Per-project accent colors (used for `cs-exp-label`, `cs-solution-label`, `--zs` etc.):
- Habit Coach: `#6366F1` (indigo)
- ZS Dashboard: `#E8820C` (orange)

---

## Case Study Status

### `zs-dashboard.html` ✅ Redesigned — 2026-05-09
Rebuilt to match portfolio design language (Inter, greyscale, habit-coach style). Content sourced from vyomikaparikh.com/zs-zaidyn but design is independent.

**Design:**
- Accent: `--zs: #E8820C` used sparingly (labels, solution labels, exp-label)
- Font: Inter only
- Hero showcase: full-bleed orange bg block
- All product screenshots: Wix CDN (public URLs, no local copies yet)

**Section order:**
1. Hero — eyebrow, thin-weight title, subtitle, meta 2×2
2. Hero showcase — full-bleed orange, dashboard hero image
3. Product context — italic quote box
4. Scale — dark impact cards ("The scale I was designing for")
5. My Focus Areas — cs-exp-card grid (1 clickable, 3 NDA)
6. DIVIDER
7. 01 — Multi-Platform Incentives Dashboard (section-divider)
8. The Challenge — prose + black HMW box
9. Design Process — cs-steps (Discovery / Analysis / Friction Mapping)
10. Friction Points — 4 cs-theme cards 2×2
11. Design Explorations — before placeholder + tradeoff cards + wireframe iter-frame
12. Final Solution — 5 cs-solution blocks (label + title + resolves + image + decisions)
13. DIVIDER
14. After Launch — dark impact cards (payoff)
15. My Contribution — cs-steps (mentoring first)
16. Learnings — cs-steps numbered
17. DIVIDER
18. Next project → Tata Motors
19. Footer (minimal 2-col)

**Still to do:**
- [ ] Before screenshot of old ZAIDYN dashboard (placeholder in Design Explorations)
- [ ] Verify/refine Learnings copy with Vyomika
- [ ] Download Wix CDN images locally so they don't depend on Wix
- [ ] Commit to git

---

### `habit-coach-ai.html` — Status unknown
Has media in `assets/habit-coach/`. Not yet reviewed against Wix reference.

### `tata-motors.html` — Status unknown
Not yet reviewed against Wix reference.

### `zs-design-system.html` — Status unknown
Has `assets/zs-design-system/video.mp4`. Not yet reviewed against Wix reference.

---

## Dev Notes

- Local server: `python3 -m http.server 9090`
- Screenshots/comparison: Playwright via `node -e "..."` using `/Users/a0s0f2q/.local/lib/node_modules/playwright`
- Use `waitUntil: 'domcontentloaded'` + `waitForTimeout(3000)` — not `networkidle` (times out)
- Force reveal animations before full-page screenshots: `page.evaluate(() => { document.querySelectorAll('.reveal').forEach(el => el.classList.add('in')); })`
- **Never use agent-browser skill** for screenshots — use Playwright directly

---

## Git / Push Rules

See `CLAUDE.md` for full rules. Key points:
- Never use Git LFS
- Push in batches when adding large binary files
- Always use `.mp4`, never `.mov`
- `http.postBuffer` must be set to `524288000`
