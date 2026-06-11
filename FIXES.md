# Theme changes — 2026-06-11

This folder is now the **canonical local copy** of grain-hugo-theme (the GitHub main-branch tarball plus the fixes below). It is not a git checkout; to publish, connect it to the GitHub repo yourself (clone elsewhere and copy these files over, or `git init` here and add the remote), then push and bump the submodule pin in `prashantghimire-dev`.

Changed files: `theme.toml`, `layouts/index.html`, `layouts/_default/baseof.html`, `static/js/script.js`, `static/css/style.css`, `exampleSite/hugo.toml`, `README.md`.

## Hugo themes directory blockers (the likely rejection reasons)

1. **`theme.toml` had no `min_version`.** Required field for themes.gohugo.io. Added `min_version = "0.110.0"` (matches the README).
2. **exampleSite declared `theme = "grain-resume"` but the repo is `grain-hugo-theme`.** The directory's demo build clones the repo under its own name and builds `exampleSite` with `--themesDir ../..`, so theme lookup by folder name failed. Changed to `theme = "grain-hugo-theme"`.
3. **Hard crash on single-word `name` param.** `index $nameParts 1` errors when `name` has one word; a reviewer testing with their own config hits a build failure immediately. Now guarded (verified: `name = "Cher"` builds, shows "C" avatar and one-line heading).

Images already met spec: `images/screenshot.jpg` 1500×1000, `images/tn.jpg` 900×600.

## Bug fixes

- **Nav highlight on scroll**: observer watched a nonexistent `.intro` class and used `threshold: 0.3`, so sections taller than ~3 viewport heights never highlighted. Now a viewport mid-band (`rootMargin: '-35% 0px -55% 0px'`) decides the active section at any height.
- Tab clicks guard against a missing pane; observer only attaches when sections exist.

## Improvements

- All sections + nav links render only when their data file exists; lone skills file auto-activates and hides the toggle.
- Tab labels configurable: `skillsTabPrimary` / `skillsTabSecondary` (defaults unchanged).
- Email/LinkedIn/GitHub/available are each optional.
- `rel="noopener noreferrer"` on external links; `<html lang>` from site config; canonical link; optional `favicon` param; `<title>` includes role.
- Print stylesheet: white background, both skill tabs expanded, nav hidden, sections avoid page breaks.

## Life pass — 2026-06-11 (second round)

Goal: remove the generic dark-template feel while staying formal. Design language borrowed from the owner's existing brand taste (Playfair surname, accent rule, Swiss numbering, mono details), not from the channel brand itself (colors and fonts stay the theme's own).

- Surname renders in Playfair Display 900 (`<span class="surname">`); short accent rule under the name block
- Section labels numbered Swiss-style ("01 / Skills") via CSS counters, mono prefix
- Experience entries sit on a timeline rail with accent nodes; dates switched to mono
- Availability badge gained a softly pulsing status dot
- Project cards: accent left-rule slides in on hover; tags brighten; skill rows get a faint hover wash
- Footer: `footerNote` param on the left, "Updated <build month year>" on the right (from Hugo `now`)
- New optional params: `location` (mono line under role), `footerNote`
- Reveal-on-scroll fade (JS-added classes, so no-JS stays fully static); `prefers-reduced-motion` disables all motion; print stylesheet updated for the new elements
- `::selection` in accent; slim scrollbars on panel/content

## Paper revamp — 2026-06-11 (third round, full redesign)

User verdict on the dark card layout: AI slop. Direction chosen: warm paper editorial, homelab kept as a dedicated section.

**Layout**: the two-column card, sticky sidebar, side nav, and skills tabs are gone. The page is now a single centred sheet (max 760px) on warm paper grain (`#ede8e0`, same feTurbulence grain at 0.05), typeset like a printed CV: letterhead masthead (big Inter first name / Playfair surname, accent rule, role, mono location, status pill, bio, underlined mono contact row), then numbered sections with hairlines: 01 Experience (timeline), 02 The Lab, 03 Skills, 04 Certifications. One accent only (`#ab4a10` burnt orange ink); the teal/blue second accent system is deleted.

**Homelab section rebuilt as proof of work** (`data/lab.yaml`, new): intro paragraph plus entries of `name / what / proves / stack`. The "proves" line says what each piece demonstrates for the job. Replaces both the homelab skills tab and the seven project cards.

**Data contract changed (breaking)**: `skills_support.yaml` + `skills_homelab.yaml` → single `skills.yaml`; `projects.yaml` → `lab.yaml`. New param `labLabel` (default "The Lab"); params `skillsTabPrimary`/`skillsTabSecondary` removed. JS reduced to the reveal observer only (tabs and scrollspy nav no longer exist).

**Theme images regenerated** from the new exampleSite at spec sizes: `images/screenshot.jpg` 1500×1000, `images/tn.jpg` 900×600. Live-site preview at `images/preview-site.jpg`. theme.toml description/tags/features updated (dark → light/paper).

## Green full-width revamp — 2026-06-11 (fourth round)

User feedback on the paper sheet: too narrow, too much monospace, orange → light green, bio line "not just to pad a CV" too aggressive, wants SVG decoration and more life.

- **Full-width editorial grid**: page max-width 1180px. Masthead is two columns (text left, animated network-constellation SVG right). Each section is a `220px + 1fr` grid: sticky label rail (large Playfair number in light green + small caps title) with wide content beside it. Lab items are a 2-column card grid; certifications a 2-column list. Collapses to single column under 980px.
- **Palette**: light sage green `#8fbe96` for decoration (rules, nodes, contours, chips), deeper `#46734f` for readable accents. Ink shifted to green-tinted warm black. Orange removed entirely.
- **Monospace removed**: IBM Plex Mono dropped from the font load; everything is Inter except the Playfair name and section numbers.
- **SVG decoration**: fixed topographic contour rings top-right and bottom-left (light green, behind content), and a masthead network constellation with slowly flowing dashed links and gently drifting nodes. All animation disabled under `prefers-reduced-motion`, all hidden in print, contours hidden on small screens.
- Lab cards lift on hover with green border; stack chips are now green pills; bio line softened in the site config.

## Compact pass — 2026-06-11 (fifth round)

User feedback: SVG art weird, "open to work" pill weird, name far too big, too much scrolling.

- Topographic contours and the network constellation are deleted (markup + CSS). The only decoration left: paper grain, a 3px green bar across the very top of the viewport, and the green accents in the content itself.
- Masthead collapsed from a hero into a letterhead band: avatar + name (~34px, one line, Playfair surname) with role · location beneath, contact links and a plain "● Open to work" status (no pill, no uppercase) on the right, bio as a single full-width paragraph below. Name rule removed.
- Density: section padding 56→34px, label rail 220→200px with smaller Playfair numbers, experience bullets flow into two columns on wide screens (`auto-fit minmax(380px,1fr)`), lab cards `auto-fit minmax(290px,1fr)` (3-up on full width), tighter paddings everywhere.
- Result: full page height ~2530px at 1440px wide, down from ~4000+. The `proves` field was removed from the data contract in the prior round.

## Verified

- `exampleSite` builds via the directory's method (`hugo --themesDir ../..` with repo-name folder).
- `prashantghimire-dev` builds with `hugo --themesDir .. --theme grain-hugo-theme`.

## Re-submission

Submission is a PR to `github.com/gohugoio/hugoThemesSiteBuilder` adding `github.com/ghimireaacs/grain-hugo-theme` to `themes.txt`. Push these changes first so the repo passes their checks.
