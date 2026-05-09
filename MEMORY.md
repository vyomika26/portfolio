# Project Memory

Working notes and context for Claude Code sessions on this repo.

---

## People

- **Vyomika Parikh** — AI Product Designer, this is her portfolio
- **Akshay Shah** — builds and maintains the site for her (`akshay.vjti11@gmail.com`)

---

## Reference Site

**https://www.vyomikaparikh.com** — Vyomika's original Wix portfolio.
Each case study page here should mirror the corresponding Wix page as closely as possible.

| Local file | Reference URL |
|---|---|
| `zs-dashboard.html` | https://www.vyomikaparikh.com/zs-zaidyn |
| `zs-design-system.html` | https://www.vyomikaparikh.com/zs-design-system (TBD) |
| `habit-coach-ai.html` | https://www.vyomikaparikh.com/habit-coach (TBD) |
| `tata-motors.html` | https://www.vyomikaparikh.com/tata-motors (TBD) |

---

## Case Study Status

### `zs-dashboard.html` ✅ Rebuilt — 2026-05-09
Rebuilt from scratch to match https://www.vyomikaparikh.com/zs-zaidyn.

**Design:**
- Accent color: orange `#E8820C`
- Fonts: Poppins (headings) + Inter (body)
- Hero image + all product screenshots pulled from Wix CDN (public URLs, no local copies yet)

**Section order:**
1. Hero tags (HEALTH TECH · DASHBOARD · WEB) + bold title
2. Full-width ZAIDYN dashboard hero image (Wix CDN)
3. Meta two-column — Duration / Role / Team | Platform description
4. Problem Statement
5. Project Impact — 3 peach cards (45,000+ / $2.5B+ / 75%)
6. My Focus Areas — 2×2 image grid + NDA disclaimer
7. Multi-Platform Incentives Dashboard heading
8. The Challenge — two-column label + bullets
9. Orange full-width quote block
10. Design Process — research grid + 4 friction point cards
11. Design Explorations — italic heading + wireframes image
12. Final Solution — 5 alternating image/text blocks (peach bg)
13. Project Impact — repeated
14. My Contribution — bullet list
15. Lavender footer — "Thank you for the visit" + email

**Still to do / brainstorm:**
- [ ] Review content depth — expand any sections?
- [ ] Before/after comparison in Design Explorations?
- [ ] Download Wix CDN images locally so they don't depend on Wix
- [ ] Add "Next project" nav link at bottom
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
