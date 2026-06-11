# grain-resume

A single-page Hugo resume theme typeset like a printed CV. Warm paper grain, serif/sans masthead, numbered sections, timeline experience, a proof-of-work lab section, and a real print stylesheet. Data-driven: all content lives in `data/*.yaml` and `hugo.toml`.

![screenshot](https://raw.githubusercontent.com/ghimireaacs/grain-hugo-theme/main/images/screenshot.jpg)

---

## Requirements

- [Hugo](https://gohugo.io/installation/) v0.110 or later

---

## Example site

A working example is in [`exampleSite/`](exampleSite/). Copy its contents as a starting point:

```
exampleSite/
├── hugo.toml                      # all params with comments
├── netlify.toml                   # Netlify build config
├── content/_index.md              # required by Hugo — leave empty
└── data/
    ├── experience.yaml            # work history (timeline)
    ├── lab.yaml                   # homelab / proof-of-work section
    ├── skills.yaml                # one merged skills table
    └── certifications.yaml        # certs and education
```

To run the example locally:

```bash
cd exampleSite
hugo server --themesDir ../..
```

---

## Installation

Add the theme as a git submodule inside your Hugo site:

```bash
git submodule add https://github.com/ghimireaacs/grain-hugo-theme themes/grain-resume
```

Then set the theme in your `hugo.toml`:

```toml
theme = "grain-resume"
```

---

## Configuration

### `hugo.toml` params

All personal content is set in `[params]`. The theme templates contain no hardcoded content.

```toml
baseURL = "https://yoursite.com/"
languageCode = "en-us"
title = "Your Name"
theme = "grain-resume"
disableKinds = ["taxonomy", "term"]

[params]
  name      = "Your Name"           # single or multi-word both work; last word is set in serif
  role      = "Job Title"
  available = "Open to work"        # pill badge with status dot — remove line to hide
  bio       = "One or two sentences. Personal voice works best here."
  email     = "you@example.com"     # optional — link hidden if omitted
  linkedin  = "https://linkedin.com/in/your-handle"   # optional
  github    = "https://github.com/your-handle"        # optional
  # avatar      = "/img/photo.jpg"  # optional — initials shown if omitted
  # description = "..."             # optional — overrides meta description
  # favicon     = "/img/favicon.png"
  # location    = "Sydney, Australia"   # mono line under the role
  # labLabel    = "The Lab"             # heading of the lab section
  # footerNote  = "Built with Hugo."    # left side of the footer
```

Sections render only when their data file exists. Delete a data file and its section disappears.

### `content/_index.md`

Hugo requires this file to invoke the homepage template. Create it with empty front matter:

```
---
---
```

---

## Data files

All content lives in `data/`. See [`exampleSite/data/`](exampleSite/data/) for working samples.

### `data/experience.yaml`

Rendered as a timeline. Each entry is a heading row (title + date) with bullets below.

```yaml
- title: IT Internship — Some Company
  date: "Nov 2025 – Feb 2026"
  items:
    - "What you did, action-verb first"
    - "Another concrete line"
```

`date` is a plain string: a year range, `"Ongoing"`, whatever fits.

### `data/lab.yaml`

The proof-of-work section. An intro paragraph plus entries that say what runs, in plain language a recruiter can follow.

```yaml
intro: "One or two sentences framing the lab: what it is, how long it has run, why it exists."

items:
  - name: Network segmentation
    what: "What is actually running, in plain language. End with what it shows about you, as prose."
    stack: [OPNsense, VLANs, Pi-hole]
```

`stack` is optional per entry. Rename the section heading with the `labLabel` param.

### `data/skills.yaml`

One merged table. `category` is the mono left column, `skills` is free text. Use `·` (middle dot, U+00B7) to separate items.

```yaml
- category: Windows & Desktop
  skills: "Windows 10/11 · Windows Server · printer support · RDP"

- category: Networking
  skills: "TCP/IP · VLANs · DNS / DHCP · VPN"
```

### `data/certifications.yaml`

Two columns: name on the left, org or year on the right.

```yaml
- name: CompTIA A+
  org: CompTIA

- name: Bachelor of Information Technology
  org: "2024"
```

---

## Profile photo

1. Place your image at `static/img/photo.jpg` in your site root
2. Add to `hugo.toml`:
   ```toml
   avatar = "/img/photo.jpg"
   ```

If `avatar` is not set, a circle with your initials (derived from `params.name`) is shown instead.

---

## Printing

The theme ships a print stylesheet: plain white background, no animation, sections kept whole across page breaks. Ctrl+P produces a usable paper CV.

---

## Netlify deployment

Set this environment variable in Netlify site settings so the theme submodule is pulled during build:

| Key | Value |
|---|---|
| `GIT_SUBMODULE_STRATEGY` | `recursive` |

Use [`exampleSite/netlify.toml`](exampleSite/netlify.toml) as your `netlify.toml`. It pins the Hugo version and sets the build command.

---

## Customisation

Colors and type are CSS custom properties at the top of `static/css/style.css`. Fork the theme and edit them directly.

| Variable | Default | Purpose |
|---|---|---|
| `--paper` | `#ede8e0` | Page background (warm paper) |
| `--ink` | `#211d18` | Headings and primary text |
| `--mid` | `#554e43` | Body text |
| `--muted` | `#8a8175` | Meta text, dates, labels |
| `--accent` | `#ab4a10` | Rules, numbering, badges, hovers |
