# Engineering Portfolio

A self-contained project portfolio, hosted on GitHub Pages. No database, no server, no build step — all content lives in a single JSON file in this repository, and the site republishes itself automatically on every change.

**Live site:** `https://lukeq-mich.github.io/portfolio/`

## How it works

| File | Role |
|---|---|
| `index.html` | The public portfolio — an editorial "drawing register" of projects, each listing the skills applied, tools used, and artefacts produced. |
| `admin.html` | A private, password-locked editor with full create/read/update/delete for projects, skills, tools, artefact links, and site details. |
| `data/data.json` | The single source of truth for all content. |
| `.github/workflows/deploy.yml` | Deploys the site to GitHub Pages on every push to `main`, injecting SEO and link-preview metadata from `data.json` at publish time. |
| `favicon.svg` | Browser-tab icon. |

Edits made in the editor are committed straight back to `data/data.json` through the GitHub API, which triggers the deploy workflow — changes are live in about a minute. Site-wide customisation (introduction, SEO description, accent colour) is edited the same way, from the editor's Site details tab.

## Content model

Projects, Skills, and Tools are separate records linked many-to-many: each project references the skills and tools it used by ID. Artefacts (reports, drawings, posters, code, photos, video) belong to projects; files are uploaded through the editor (drag-and-drop) into the `artefacts/` folder, or linked externally, typed semantically and numbered on the site like drawing sheets. Each project can also carry a cover image, uploaded the same way into the `cover-images/` folder.

## Editing

The editor lives at `/admin.html`, reachable from the small **Editor login** button in the site footer. It authenticates against GitHub with a fine-grained personal access token scoped to this repository only, encrypted locally with a password — the token never appears in this repository. 
---

Set in Fraunces & Newsreader · Built with plain HTML, CSS, and JavaScript
