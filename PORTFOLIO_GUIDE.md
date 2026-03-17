# Portfolio Site Guide
> A reference for maintaining and growing this Hugo portfolio site, especially after a long break.

## Quick Reference

| What I want to do | Where to go |
|---|---|
| Add a new project | `content/portfolio/<project-name>/index.md` |
| Write a blog post | `content/post/<post-name>.md` |
| Add a tech note | `content/notes/<topic>.md` |
| Update my bio/summary | `content/about.md` |
| Update homepage tagline | `config/_default/languages.en.toml` |
| Change site colors/layout | `config/_default/params.toml` |
| Add/remove nav menu links | `config/_default/menus.en.toml` |
| Update pagination size | `config/_default/config.toml` → `[pagination] pagerSize` |
| Add a profile photo | `assets/img/photo-profile.jpg` |
| Add a resume PDF | `static/resume.pdf` |

---

## Tech Stack

- **Framework**: [Hugo](https://gohugo.io/) (static site generator)
- **Theme**: [Congo v2](https://jpanther.github.io/congo/) by James Panther
- **Hosting**: GitHub Pages (`alyamutiara.github.io`)
- **Module manager**: Go modules (`go.mod`)

---

## Local Development

```bash
# Start local dev server with live reload
cd alyamutiara.github.io
hugo server

# Build site to /public (for deployment)
hugo build

# Update Congo theme to latest
hugo mod get github.com/jpanther/congo/v2@latest
hugo mod tidy
```

The site is served at `http://localhost:1313/` by default.

> **Note:** If you ever get `paginate` deprecation errors or shortcode errors after upgrading Hugo, see the fixes already applied:
> - `paginate` is now `[pagination] pagerSize = 10` in `config/_default/config.toml`
> - `layouts/_shortcodes/figure.html` overrides the removed built-in Hugo figure template

---

## Site Structure

```
content/
├── _index.md           ← Homepage text
├── about.md            ← About / Resume page
├── portfolio/
│   ├── _index.md       ← Portfolio section intro
│   └── <project>/
│       ├── index.md    ← Project write-up (leaf bundle)
│       └── *.jpg/webp  ← Images co-located with the post
├── post/
│   ├── _index.md       ← Blog section intro
│   └── <post>.md       ← Blog post
├── notes/
│   └── <topic>.md      ← Tech notes / learning logs
└── draft/              ← WIP content (not published)
```

---

## Adding a New Portfolio Project

1. Create a new folder: `content/portfolio/<your-project-name>/`
2. Inside it, create `index.md` — this is a **leaf bundle** (Hugo term for a folder with `index.md` + local assets)
3. Put images directly in the same folder

### Portfolio front matter template

```markdown
---
title: "Your Project Title"
date: 2026-01-01
tags: ["Python", "dbt", "BigQuery"]   # tech stack tags
---

![Thumbnail](thumb-yourproject.jpg)

### Introduction
Brief description of why you did this project.

### Problem Statement
What problem were you solving?

### Approach
How did you solve it? Mention tools and architecture.

### Results
Key outcomes, metrics, or learnings.

### Links
- [GitHub Repository](https://github.com/alyamutiara/...)
- [Live Demo / Dashboard](...)
```

> **Tip:** The thumbnail image filename must match what you reference in the markdown. Keep it in the same folder as `index.md`.

---

## Adding a Blog Post

Create a file in `content/post/`:

```markdown
---
title: "Post Title"
date: 2026-01-01
tags: ["tag1", "tag2"]
draft: false
---

Post content here...
```

Set `draft: true` to save a WIP post that won't be published until you change it to `false`.

---

## Adding a Tech Note

Tech notes in `content/notes/` are good for learning logs and reference material (like the existing `hadoop-notes.md`):

```markdown
---
title: "Topic Name"
date: 2026-01-01
---

## Contents
- [Section 1](#section-1)

## Section 1
...
```

---

## Updating the About Page (`content/about.md`)

This page is your live resume. The key sections to keep updated:

- **Summary** — adjust the years of experience and career direction
- **Experience** — add new roles; keep bullet points impact-focused (metric + action + outcome)
- **Skills** — keep this current; recruiters scan this fast

---

## Updating Site Metadata

These two files control the author identity that appears across all pages:

**`config/_default/languages.en.toml`**
```toml
[params.author]
  name = "Alya Mutiara F"
  image = "img/photo-profile.jpg"   # path relative to assets/
  headline = "Aspiring to be a Data Professional"
  links = [
    { linkedin = "https://linkedin.com/in/..." },
    { github = "https://github.com/alyamutiara" },
  ]
```

**`content/_index.md`** — the homepage blurb shown below your photo.

---

## Navigation Menu (`config/_default/menus.en.toml`)

```toml
[[main]]
  name = "About"
  pageRef = "/about"
  weight = 10        # lower weight = appears first

[[main]]
  name = "Portfolio"
  pageRef = "/portfolio"
  weight = 20

[[main]]
  name = "Resume"
  url = "/resume.pdf"   # PDF must exist in static/
  weight = 30
```

---

## Deployment

This site auto-deploys to GitHub Pages when you push to `main`. The built output lives in `/public/`, which should be committed (or handled by a GitHub Action if you set one up).

```bash
# Typical update workflow
hugo build
git add .
git commit -m "add: <project name> portfolio entry"
git push origin main
```

---

## Data Engineer Portfolio: Recommendations

Right now the portfolio has one project (MovieNow SQL analysis). Here's a roadmap to make it stand out to DE hiring managers.

### Priority Projects to Add

These cover the key areas hiring managers look for:

| Priority | Project Idea | Technologies | Why It Matters |
|---|---|---|---|
| 🔴 High | **End-to-end batch pipeline** | Airflow + dbt + BigQuery or Postgres | Shows orchestration + transformation, the core DE skill loop |
| 🔴 High | **Streaming pipeline** | Kafka + Flink or Spark Streaming + a sink | Streaming is increasingly expected even for batch-focused roles |
| 🟡 Medium | **Data Lake + warehouse architecture** | GCS/S3 → BigQuery/Redshift with partitioning | Shows you understand storage layers, not just pipelines |
| 🟡 Medium | **dbt project with tests + docs** | dbt Core + BigQuery | dbt is a default tool now; showing test coverage and documentation stands out |
| 🟢 Nice to have | **Real-time dashboard** | Kafka → BigQuery → Looker Studio | Closes the loop — stakeholders care about visibility |
| 🟢 Nice to have | **Data quality / observability layer** | Great Expectations or dbt tests | Shows production-readiness mindset |

You already have the **attendance ETL pipeline**, **bq-data-ingestion**, and **dbt-transformation-nyc-taxi-data** repos — those should be written up and published here!

### How to Write Up Each Project

A strong DE project post answers these 5 questions:

1. **What is the data source?** (API, CSV, database, Kafka topic?)
2. **What pipeline architecture did you choose and why?** (Include a diagram if possible)
3. **What transformations or logic did you apply?** (business logic, deduplication, type casting)
4. **How would this work at scale?** (partitioning strategy, incremental loads, scheduling)
5. **What would you do differently?** (shows engineering maturity)

### Content to Prioritize in the About Page

These are commonly evaluated by DE recruiters:

- **Specific tools with context** — not just "Airflow" but "Managed Airflow pipelines processing X records daily"
- **Scale indicators** — GB/TB processed, number of tables, pipeline frequency
- **Collaboration signals** — "led team of 5", "worked with analytics team to define SLAs"
- **Problem-solving story** — the 500 vulnerability findings → 70% reduction is a great example; write like this for data work too

### Sections Worth Adding to the Site

- **`/notes/`** — You have one note (Hadoop). Expand this — notes on dbt, Airflow gotchas, BigQuery optimization tips, etc. This signals continuous learning and helps SEO
- **Tags page** — Congo supports tag-based browsing automatically once you have enough tagged content
- **Resume PDF** — Link it in the menu (already configured, just needs the file at `static/resume.pdf`)

---

## Useful Congo Theme Links

- [Congo Docs — Configuration](https://jpanther.github.io/congo/docs/configuration/)
- [Congo Docs — Front Matter](https://jpanther.github.io/congo/docs/front-matter/)
- [Congo Docs — Shortcodes](https://jpanther.github.io/congo/docs/shortcodes/)
- [Congo Docs — Thumbnails](https://jpanther.github.io/congo/docs/thumbnails/)
