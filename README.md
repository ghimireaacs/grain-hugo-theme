# grain-resume

A dark, single-page Hugo theme for IT/tech portfolios and resumes. Floating card layout, static grain background, sticky two-column panel, tabbed skills section.

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
    ├── skills_support.yaml        # IT Support tab
    ├── skills_homelab.yaml        # Homelab tab (monospace)
    ├── experience.yaml            # work history
    ├── certifications.yaml        # certs and education
    └── projects.yaml              # project cards
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

All personal content is set in `[params]` — the theme templates contain no hardcoded content.

```toml
baseURL = "https://yoursite.com/"
languageCode = "en-us"
title = "Your Name"
theme = "grain-resume"
disableKinds = ["taxonomy", "term"]

[params]
  name      = "Your Name"           # rendered as two-line heading — expects "First Last"
  role      = "Job Title"           # shown below name in muted text
  available = "Open to work"        # accent badge — remove line to hide it entirely
  bio       = "One or two sentences. Personal voice works best here."
  email     = "you@example.com"
  linkedin  = "https://linkedin.com/in/your-handle"
  github    = "https://github.com/your-handle"
  # avatar      = "/img/photo.jpg"  # optional — initials shown if omitted
  # description = "..."             # optional — overrides meta description
```

### `content/_index.md`

Hugo requires this file to invoke the homepage template. Create it with empty front matter:

```
---
---
```

---

## Data files

All content lives in `data/`. See [`exampleSite/data/`](exampleSite/data/) for full working samples with inline comments.

### `data/skills_support.yaml` — IT Support tab

`category` is the left column label. `skills` is free text in the right column. Use `·` (middle dot, U+00B7) to separate items.

```yaml
- category: Windows & Desktop
  skills: "Windows 10/11 · Windows Server · OS imaging · Hardware & printer support"

- category: Microsoft Cloud
  skills: "Microsoft 365 · Exchange Online · Azure AD · Intune · MFA"
```

### `data/skills_homelab.yaml` — Homelab tab

Same structure, rendered in monospace. Suits tool names and stack components.

```yaml
- category: Containers
  skills: "Docker · k3s · Helm"

- category: Networking
  skills: "OPNsense · VLANs · Pi-hole · Traefik"
```

### `data/experience.yaml`

Each entry is a heading row (title + date) with a bullet list below.

```yaml
- title: IT Support Lab
  date: Ongoing
  items:
    - "Built Active Directory environment with 50+ simulated users, DNS, DHCP, GPO"
    - "Worked through 40+ simulated help desk tickets"

- title: Customer Service — Retail
  date: "2021 – 2024"
  items:
    - "High-volume customer interactions requiring patience and clear communication"
```

`date` is a plain string — use a year range, `"Ongoing"`, `"Mar 2023"`, whatever fits.

### `data/certifications.yaml`

Two columns: cert name on the left, org or year on the right.

```yaml
- name: CompTIA A+
  org: CompTIA

- name: Bachelor of Information Technology
  org: "2024"

- name: Some Training Program
  org: "Training completed"
```

### `data/projects.yaml`

Each project is a card with a name, description paragraph, and tag chips.

```yaml
- name: Home Network
  description: "VLAN-segmented network — OPNsense, 802.1Q trunking, Pi-hole DNS."
  tags: [OPNsense, VLANs, Pi-hole]
```

`tags` renders as small monospace chips. Keep them short — one word or hyphenated.

---

## Profile photo

1. Place your image at `static/img/photo.jpg` in your site root
2. Add to `hugo.toml`:
   ```toml
   avatar = "/img/photo.jpg"
   ```

If `avatar` is not set, a circle with your initials (derived from `params.name`) is shown instead.

---

## Netlify deployment

Set this environment variable in Netlify site settings so the theme submodule is pulled during build:

| Key | Value |
|---|---|
| `GIT_SUBMODULE_STRATEGY` | `recursive` |

Use [`exampleSite/netlify.toml`](exampleSite/netlify.toml) as your `netlify.toml` — it pins the Hugo version and sets the build command.

---

## Customisation

Colors and radius are CSS custom properties at the top of `static/css/style.css`. Fork the theme and edit them directly.

| Variable | Default | Purpose |
|---|---|---|
| `--accent` | `#c45c1a` | Badges, labels, active nav |
| `--card-bg` | `#181512` | Right content panel |
| `--panel-bg` | `#141210` | Left sidebar |
| `--page-bg` | `#0c0a09` | Page background |
| `--radius` | `14px` | Card corner radius |
