# Connected Kerry Initiative – Website

A static website built with [Eleventy (11ty)](https://www.11ty.dev/), designed to be maintained with the assistance of Claude.

---

## First-time setup

### 1. Install Node.js

Download and install Node.js from https://nodejs.org — choose the **LTS** version.

To verify it installed correctly, open a terminal and run:
```
node --version
```
You should see something like `v20.x.x`.

### 2. Clone or download this project

If using Git:
```
git clone <your-repo-url>
cd connected-kerry
```

Or unzip the downloaded folder and open a terminal inside it.

### 3. Install dependencies

```
npm install
```

This installs Eleventy. It only needs to run once (or again after you update `package.json`).

### 4. Start the local development server

```
npm start
```

Open your browser at **http://localhost:8080** — you will see the site live. It will reload automatically when you save any file.

### 5. Build the site for deployment

```
npm run build
```

This generates the complete site inside the `_site/` folder. Upload the contents of `_site/` to any web host.

---

## Hosting options (all free for this scale)

| Option | Notes |
|---|---|
| **Netlify** | Recommended. Connects to GitHub and rebuilds automatically on every push. Form handling is built in. |
| **Cloudflare Pages** | Also free, fast global CDN. Similar to Netlify. |
| **GitHub Pages** | Free, reliable, but requires a small extra config step for Eleventy. |

**Netlify is strongly recommended** because it handles the contact form (`data-netlify="true"`) with no extra setup.

---

## Project structure

```
connected-kerry/
├── src/                        ← All source files (edit these)
│   ├── _data/
│   │   └── site.json           ← Global site data: title, nav, campaigns list
│   ├── _includes/
│   │   └── layouts/
│   │       ├── base.njk        ← Main page template (header + footer)
│   │       └── campaign.njk    ← Template for individual campaign pages
│   ├── css/
│   │   └── style.css           ← All styles (design system + components)
│   ├── js/
│   │   └── main.js             ← Navigation and minor JS
│   ├── campaigns/
│   │   ├── index.njk           ← /campaigns/ listing page
│   │   ├── invisible-stops.md  ← Individual campaign pages (Markdown)
│   │   ├── invisible-journeys.md
│   │   ├── invisible-routes.md
│   │   ├── invisible-places.md
│   │   ├── invisible-people.md
│   │   ├── invisible-delivery.md
│   │   └── invisible-benefits.md
│   ├── index.njk               ← Home page
│   ├── framework.njk           ← /framework/ page
│   ├── stories.njk             ← /stories/ page
│   ├── map.njk                 ← /map/ page
│   ├── representatives.njk     ← /representatives/ page
│   ├── about.njk               ← /about/ page
│   ├── submit.njk              ← /submit/ form page
│   └── submit-thanks.njk       ← Thank-you page after form submission
├── .eleventy.js                ← Eleventy configuration
├── netlify.toml                ← Netlify deployment config
├── package.json                ← Project metadata and scripts
└── _site/                      ← Generated output (do not edit – auto-generated)
```

---

## How to make common changes

### Editing page content

Most content is in `src/`. Pages that change rarely are written in **Nunjucks** (`.njk`), which is HTML with some template logic. Campaign pages are in **Markdown** (`.md`), which is much easier to read and edit.

**To edit a campaign page**, open its `.md` file in `src/campaigns/` and edit the text. The section headings (lines starting with `##`) and callout boxes use simple Markdown syntax.

**To edit the home page**, open `src/index.njk`.

**To edit the about page**, open `src/about.njk`.

### Adding a new story to the Stories page

Open `src/stories.njk` and copy one of the `<div class="story-card">` blocks. Update the tag, title, link, and description. When you have a full story page, create `src/stories/story-slug.md` and point the link there.

### Updating the campaign descriptions

Campaign titles, summaries, and definitions shown across the site (home page cards, framework page list, campaign header) are controlled by `src/_data/site.json`. Edit the `"campaigns"` array there. The page content (body text) is in the individual `.md` files.

### Changing site-wide text (title, tagline, email)

Edit `src/_data/site.json` — the `"site"` object at the top controls the name, tagline, description, URL, and email address used throughout.

### Adding a new page

1. Create a new `.njk` or `.md` file in `src/`
2. Add front matter at the top:
```
---
layout: layouts/base.njk
title: Your Page Title
permalink: /your-url/
---
```
3. Write the page content below the `---`
4. If you want it in the nav, add it to the `"nav"` array in `src/_data/site.json`

### Changing colours or fonts

All design tokens (colours, fonts, spacing) are defined as CSS variables at the top of `src/css/style.css`. The colour palette section is clearly labelled. Change a variable there and it updates everywhere.

---

## Maintenance workflow with Claude

The intended workflow for updates is:

1. **You describe the change** you want in a message to Claude
2. **Claude provides the updated file content** — either the full file or a specific edit
3. **You copy the content** into the relevant file and save
4. The local dev server reloads and you can check the result
5. Push to GitHub (or upload `_site/`) to publish

For larger changes, Claude can be given the current file content and will return the modified version.

**Files most commonly updated:**
- Campaign `.md` files — adding evidence, stories, and updates
- `src/stories.njk` — adding new story cards
- `src/_data/site.json` — updating global data
- `src/representatives.njk` — adding briefing notes and council questions

---

## Contact form

The submit form uses **Netlify Forms** (`data-netlify="true"` attribute). This works automatically when the site is deployed on Netlify — no extra setup needed. Submissions arrive in the Netlify dashboard under Forms.

If hosting elsewhere, replace the form with a [Formspree](https://formspree.io) endpoint — free tier covers low-volume usage.

---

## Map

The map page (`src/map.njk`) currently shows a placeholder. When ready to add a real interactive map:

1. Add Leaflet.js to the page (CDN link)
2. Replace the placeholder `<div class="map-container">` with a `<div id="map">` element
3. Add a `<script>` block initialising a Leaflet map centred on Kerry (approximately `52.05, -9.50`)
4. Add GeoJSON data for documented examples

This can be done incrementally — start with a few manually added markers, then move to a data file as examples grow.
