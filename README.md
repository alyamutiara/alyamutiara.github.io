# alyamutiara.github.io

A clean, minimalist portfolio built with Hugo. Features dark mode toggle, tech stack logos via DevIcon, scroll-reveal animations, and automatic deployment to GitHub Pages.

## Quick Start

### Prerequisites

- [Hugo](https://gohugo.io/installation/) (extended version, v0.160+)

### Local Preview

```bash
git clone https://github.com/alyamutiara/alyamutiara.github.io.git
cd alyamutiara.github.io
hugo server -D
# Open http://localhost:1313
```

### Build for Production

```bash
hugo --minify
```

---

## Complete Content Editing Guide

All content is managed through data files and configuration. **You never need to edit HTML templates to update your content.** Just edit the YAML/TOML files and rebuild.

### 1. Personal Info, Hero Text & Social Links

**File:** `hugo.toml`

```toml
[params]
  author = "Alya Mutiara"                    # Your display name (nav logo)
  description = "Your meta description"       # SEO meta description

  [params.hero]
    headline = "Data Engineer."              # Main headline
    subtext = "Your tagline/subtext here."    # Description below headline
    cta = "View My Work"                     # Primary CTA button text

  [params.about]
    summary = "Your about paragraph."         # About section text

  [params.social]
    linkedin = "https://linkedin.com/in/alyamf"
    github = "https://github.com/alyamutiara"
    medium = "https://medium.com/@mutiaraa"
    email = "alyafirdausyi@gmail.com"

  resumeUrl = "https://github.com/.../resume.pdf"  # Resume/CV link
```

### 2. Experience

**File:** `data/experience.yaml`

The entries appear in the order you list them. To reorder, simply move entries up or down in the file.

```yaml
- company: "Company Name"
  role: "Your Title"
  period: "Aug 2024 — Present"
  location: "City, Country"
  type: "professional"            # "professional" or "fellowship"
  description: "Brief company description."
  highlights:
    - "Achievement one"
    - "Achievement two"
```

- `type: "fellowship"` shows a small "Fellowship" badge on the timeline entry
- `location` is optional — shows next to the date

### 3. Projects

**File:** `data/projects.yaml`

The homepage shows the first 4 projects. A "See All Projects" button links to `/projects/` which lists all entries.

```yaml
- title: "Project Name"
  description: "What this project does."
  tech:
    - "Python"
    - "BigQuery"
    - "Airflow"
  impact: "Measurable result or impact."
  link: "https://github.com/yourusername/project"
```

To add a new project, append a new `- title:` block to the list. To change which 4 appear on the homepage, reorder the entries — the first 4 are shown.

### 4. Skills

**File:** `data/skills.yaml`

Organized by category. Each skill can optionally have a DevIcon class for a logo.

```yaml
- category: "Category Name"
  items:
    - name: "Python"
      icon: "devicon-python-plain"     # DevIcon class (optional)
    - name: "SQL"                       # No icon = text-only tag
    - name: "Airflow"
      icon: "devicon-apacheairflow-plain"
```

**Finding DevIcon classes:** Browse [devicon.dev](https://devicon.dev) and click any icon to see its class name. The format is `devicon-{name}-{version}` (e.g., `devicon-python-plain`). If an icon doesn't exist in DevIcon, just omit the `icon` field — the skill will render as a text-only tag.

### 5. Certifications

**File:** `data/certifications.yaml`

Each certification links to its proof page.

```yaml
- name: "AWS Data Engineer Associate"
  year: "2024"
  issuer: "Amazon Web Services"
  icon: "devicon-amazonwebservices-plain-wordmark"
  link: "https://www.credly.com/badges/..."
```

- `icon` uses DevIcon classes (same as skills)
- `link` makes the whole card clickable — point to Credly, Microsoft Learn, etc.

### 6. Education

**File:** `data/education.yaml`

```yaml
- institution: "Institut Teknologi Bandung"
  degrees:
    - degree: "Master of Science in Computational Science"
      period: "2020 — 2023"
    - degree: "Bachelor of Science in Physics"
      period: "2014 — 2018"
```

### 7. Profile Photo

Place your photo at **`static/img/photo.jpg`**. The hero section automatically displays it. If the file is missing, the photo area gracefully hides with no broken image.

Recommended size: **160x160px** minimum (displayed at 80x80px, so 2x for retina).

---

## Project Structure

```
├── .github/workflows/deploy.yml     # GitHub Actions auto-deployment
├── assets/css/style.css             # Main stylesheet (processed by Hugo)
├── content/
│   ├── _index.md                    # Homepage marker
│   └── projects.md                  # Full projects page marker
├── data/
│   ├── experience.yaml              # Work experience entries
│   ├── projects.yaml                # Project entries (all)
│   ├── skills.yaml                  # Skills with DevIcon logos
│   ├── certifications.yaml          # Certifications with proof links
│   └── education.yaml               # Education entries
├── layouts/
│   ├── _default/
│   │   ├── baseof.html              # Base HTML (dark mode init, fonts, DevIcon)
│   │   ├── projects-page.html       # Full projects page layout
│   ├── index.html                   # Homepage layout (assembles partials)
│   └── partials/
│       ├── nav.html                 # Nav with logo + dark mode toggle
│       ├── hero.html                # Hero with photo, socials, resume link
│       ├── about.html
│       ├── experience.html          # Timeline with fellowship badge
│       ├── projects.html            # Shows first 4 + see all button
│       ├── skills.html              # Skills grid with DevIcon logos
│       ├── certifications.html      # Cert cards with proof links
│       ├── education.html
│       ├── contact.html
│       └── footer.html             # Footer + JS (toggle, scroll, reveal)
├── static/img/photo.jpg             # Your profile photo (add this)
├── hugo.toml                         # Site configuration
└── README.md
```

---

## Features

- **Dark mode** — Toggle in nav, persists via localStorage, respects system preference
- **Social links at top** — LinkedIn, GitHub, Medium, Email in hero section
- **Resume link** — "Resume" button in hero header, links to your PDF
- **AM monogram logo** — Gradient-accented in the nav
- **Profile photo** — Displayed in hero section (optional)
- **Tech stack with logos** — Uses DevIcon CDN for recognizable brand icons
- **Certifications with proof links** — Clickable cards linking to Credly/Microsoft
- **Projects split** — Homepage shows 4 projects, dedicated full page at `/projects/`
- **Scroll reveal** — Subtle fade-up animations on sections
- **Frosted glass navbar** — Blurred backdrop with scroll state
- **Responsive** — Mobile hamburger menu, adapted layouts

---

## Deployment to GitHub Pages

Deployment is **automatic** via GitHub Actions.

### Setup (one-time)

1. Go to your repository → **Settings** → **Pages**
2. Under **Source**, select **GitHub Actions**
3. Push to `main` — the workflow runs automatically

Every push to `main` triggers `.github/workflows/deploy.yml`.

---

## Customization

### Change accent color

Edit `assets/css/style.css` — both light and dark themes:

```css
:root {
  --color-accent: #3b82f6;
  --color-accent-deep: #2563eb;
}

[data-theme="dark"] {
  --color-accent: #60a5fa;
  --color-accent-deep: #93bbfd;
}
```

### Update profile photo

Replace `static/img/photo.jpg` with your photo. Recommended: 160x160px minimum.

### Add a new section

1. Create `layouts/partials/your-section.html`
2. Add it to `layouts/index.html`
3. Add a nav link in `layouts/partials/nav.html`
4. Add section number (e.g., `08`) in the section header

### Change homepage project count

In `layouts/partials/projects.html`, change `{{ range first 4 hugo.Data.projects }}` to any number.

### Update DevIcon version

In `layouts/_default/baseof.html`, change the CDN URL:

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/devicon.min.css">
```

Pin to a specific version like `@v2.16.0` if needed.

---

## Tech Stack

- **Hugo** — Static site generator
- **Vanilla CSS** — Custom properties, dark mode via `data-theme`
- **DevIcon** — Technology logos via CDN
- **Inter + JetBrains Mono** — Typography via Google Fonts
- **GitHub Actions** — Automated CI/CD
- **GitHub Pages** — Hosting

## License

MIT