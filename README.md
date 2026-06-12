# Corporate Site — Jekyll + GitHub Pages

A responsive, animated corporate website built with **Jekyll**, designed for hosting on **GitHub Pages**. Features a GSAP-powered hero animation with a Lottie JSON fallback, Formspree contact form, full SEO basics, a cookie consent banner, and a GitHub Actions CI/CD pipeline.

---

## Table of Contents

1. [Quick Start (local development)](#quick-start)
2. [Project Structure](#project-structure)
3. [Deployment to GitHub Pages](#deployment-to-github-pages)
4. [Customising Content](#customising-content)
5. [Animation System](#animation-system)
6. [Contact Form (Formspree)](#contact-form)
7. [SEO & Analytics](#seo--analytics)
8. [Cookie Consent](#cookie-consent)
9. [Swapping Images](#swapping-images)
10. [Switching to a Different Form Provider](#switching-form-provider)

---

## Quick Start

### Prerequisites

| Tool | Minimum version |
|---|---|
| Ruby | 3.1 |
| Bundler | 2.x |
| Git | 2.x |

```bash
# 1. Clone the repo
git clone https://github.com/YOUR-ORG/YOUR-REPO.git
cd YOUR-REPO

# 2. Install gems
bundle install

# 3. Serve locally with live reload
bundle exec jekyll serve --livereload

# 4. Open http://localhost:4000 in your browser
```

> **Windows users:** If you see a `wdm` or `tzinfo-data` error, run `gem install wdm tzinfo-data` first.

---

## Project Structure

```
.
├── _config.yml              # Site-wide configuration (title, URL, GA ID, Formspree)
├── Gemfile                  # Ruby dependencies
├── index.html               # Home page (uses layout: home)
│
├── _layouts/
│   ├── default.html         # Base HTML shell — head, header, footer, scripts
│   ├── home.html            # Thin wrapper for default (loads hero animation scripts)
│   └── page.html            # Inner pages — page-hero banner + content container
│
├── _includes/
│   ├── header.html          # Sticky responsive header with mobile hamburger
│   ├── footer.html          # Footer with nav columns and legal links
│   ├── cookie-banner.html   # Cookie consent banner (GDPR/CCPA stub)
│   ├── seo-extras.html      # OG tags, canonical, Schema.org JSON-LD
│   └── icons/               # SVG icon partials (gear, shield, brain, …)
│
├── _sass/
│   ├── _variables.scss      # Design tokens — colours, typography, spacing
│   ├── _reset.scss          # Modern CSS reset
│   ├── _utilities.scss      # Buttons, containers, reveal helpers
│   ├── _header.scss         # Header + mobile nav
│   ├── _hero.scss           # Hero section — orbs, particles, text, stats strip
│   ├── _sections.scss       # Cards, grids, stats, CTA banner, page-hero
│   ├── _forms.scss          # Form inputs, contact layout, success message
│   └── _footer.scss         # Footer styles
│
├── assets/
│   ├── css/main.scss        # Sass entry point (front matter required)
│   ├── js/
│   │   ├── hero-animation.js  # GSAP parallax + text reveal + Lottie init
│   │   ├── scroll-animations.js # ScrollTrigger reveal, counters, rule wipe
│   │   └── main.js          # Mobile nav, dropdowns, sticky header, smooth scroll
│   ├── images/
│   │   ├── favicon.svg      # Replace with brand favicon
│   │   └── hero-grid.svg    # SVG grid overlay used in hero background
│   └── lottie/
│       └── hero-bg.json     # Bundled abstract Lottie animation (decorative)
│
├── _data/
│   └── services.yml         # Service cards data — name, url, icon, summary
│
├── _pages/
│   ├── about.html           # About page
│   ├── services.html        # Services listing
│   ├── contact.html         # Contact page with Formspree form
│   ├── privacy.html         # Privacy Policy placeholder
│   ├── terms.html           # Terms of Use placeholder
│   └── accessibility.html  # Accessibility Statement placeholder
│
└── .github/
    └── workflows/
        └── deploy.yml       # GitHub Actions: build → deploy to Pages
```

---

## Deployment to GitHub Pages

### Option A — GitHub Actions (recommended)

The repo ships with `.github/workflows/deploy.yml` which builds Jekyll and deploys on every push to `main`.

1. Push the repo to GitHub.
2. Go to **Settings → Pages**.
3. Under **Source**, select **GitHub Actions**.
4. Make sure `_config.yml` has the correct `url` and `baseurl`:

```yaml
# Hosted at https://YOUR-ORG.github.io  (root of org/user site)
url: "https://YOUR-ORG.github.io"
baseurl: ""

# Hosted at https://YOUR-ORG.github.io/YOUR-REPO  (project site)
url: "https://YOUR-ORG.github.io"
baseurl: "/YOUR-REPO"
```

5. Commit and push to `main` — GitHub Actions will build and deploy automatically.

### Option B — Manual / `gh-pages` branch

```bash
gem install jekyll-deploy   # or use the standard method below

JEKYLL_ENV=production bundle exec jekyll build
# Then push the _site/ folder to the gh-pages branch, or use:
bundle exec jekyll build && git add _site && git subtree push --prefix _site origin gh-pages
```

---

## Customising Content

### Site-wide settings (`_config.yml`)

| Key | What to change |
|---|---|
| `title` | Organisation name |
| `description` | Meta description / footer tagline |
| `url` | Your live domain |
| `baseurl` | Subpath if not root (leave `""` for root) |
| `google_analytics` | GA4 Measurement ID (`G-XXXXXXXXXX`) |
| `formspree_endpoint` | Formspree form URL |

### Navigation & services (`_data/services.yml`)

Edit `_data/services.yml` to add, remove, or rename service entries. Each entry drives both the Services page cards and the header dropdown:

```yaml
- name: "Your Service Name"
  url: "/services/#your-anchor"
  icon: "gear"          # matches _includes/icons/<icon>.svg
  summary: "One-sentence description shown on the card."
```

**Available icons:** `gear`, `transform`, `chart`, `flask`, `shield`, `brain`  
To add a new icon, drop an SVG file into `_includes/icons/` and reference it by filename (without `.svg`).

### Page content (`_pages/`)

Each `.html` file in `_pages/` has YAML front matter at the top:

```yaml
---
layout: page
title:    "Page Title"
subtitle: "Optional sub-heading shown in the page hero banner"
description: "Meta description for SEO"
permalink: /your-url/
---
```

Replace the placeholder `<p>` content below the front matter with your real copy.

### Footer navigation

The footer nav columns are hard-coded in `_includes/footer.html`. Edit the `<a href>` elements and heading text there to match your site map.

---

## Animation System

### Architecture

```
Hero section
├── CSS keyframe orbs         (always on — pure CSS, no JS dependency)
├── CSS particle drift        (always on)
├── Lottie JSON               (always on — decorative, low-opacity background)
└── GSAP + ScrollTrigger      (on if GSAP loads AND prefers-reduced-motion: no)
    ├── Per-word headline reveal
    ├── Eyebrow / subtitle / CTA stagger
    ├── Parallax on particles & background overlay
    └── Scroll-out fade on hero content
```

### Replacing the Lottie animation

1. Export a Lottie JSON from [LottieFiles](https://lottiefiles.com) or After Effects.
2. Place it in `assets/lottie/`.
3. Update the `data-src` attribute on `#hero-lottie-container` in `index.html`:

```html
<div id="hero-lottie-container"
     data-src="{{ '/assets/lottie/your-file.json' | relative_url }}"
     aria-hidden="true"></div>
```

> **Performance tip:** Keep the Lottie file under 200 KB. Use LottieFiles' Optimiser to compress before export.

### Adjusting GSAP timings

Open `assets/js/hero-animation.js`. The main timeline is in the `initGSAP()` function. Adjust `duration`, `delay`, and `stagger` values on the timeline steps. GSAP docs: https://gsap.com/docs/v3/

### Disabling animation entirely

Add `animation: disabled` to a page's front matter (not yet wired — manual approach):  
In `default.html`, wrap the GSAP script tags in `{% unless page.no_animation %}`.

### Scroll reveal (sections below hero)

Add `data-reveal` to any element to opt it in to the scroll-triggered fade-up:

```html
<div data-reveal>Revealed on scroll</div>
<div data-reveal data-reveal-delay="0.2">Revealed 200ms later</div>
```

For staggered card groups, add `data-reveal-group` to the parent and `data-reveal-item` to each child.

### Animated counters

```html
<span data-count-up="8000" data-count-suffix="+">8,000+</span>
```

The counter animates from 0 to the target number when it enters the viewport.

---

## Contact Form

The contact form in `_pages/contact.html` submits to **Formspree**.

### Setup

1. Create a free account at [formspree.io](https://formspree.io).
2. Create a new form — copy the endpoint URL (e.g. `https://formspree.io/f/abcdefgh`).
3. Paste it into `_config.yml`:

```yaml
formspree_endpoint: "https://formspree.io/f/YOUR_FORM_ID"
```

4. The form includes:
   - A **honeypot** anti-spam field (`_honey`)
   - A **`_subject`** override for email subject lines
   - A **`_next`** redirect back to the contact page with `?success=true`
   - A **privacy consent checkbox** (required)

### Testing locally

Formspree does not process submissions from `localhost`. Use [ngrok](https://ngrok.com) to expose your local server, or test on the deployed GitHub Pages URL.

---

## Switching Form Provider

### Netlify Forms (if deploying to Netlify instead of GitHub Pages)

Replace the `<form>` opening tag in `_pages/contact.html`:

```html
<form id="contact-form" name="contact" method="POST" data-netlify="true" netlify-honeypot="_honey">
  <input type="hidden" name="form-name" value="contact">
  <!-- remove _next and Formspree hidden fields -->
```

### Basin / Web3Forms / EmailJS

Swap the `action` URL and adjust hidden fields per each provider's documentation. The form fields themselves remain unchanged.

---

## SEO & Analytics

### jekyll-seo-tag

The `{% seo %}` tag in `default.html` automatically generates:
- `<title>`, `<meta name="description">`, canonical URL
- Open Graph (`og:title`, `og:description`, `og:image`, `og:type`)
- Twitter Card tags

Set per-page SEO in front matter:

```yaml
title:       "Page Title"
description: "Custom meta description for this page."
image:       "/assets/images/og-page.jpg"  # 1200×630px recommended
```

### Google Analytics 4

1. Replace `G-XXXXXXXXXX` in `_config.yml` with your GA4 Measurement ID.
2. The snippet in `default.html` fires automatically on production builds.
3. The cookie banner must be accepted before GA is active (see cookie consent section).

### Sitemap

`jekyll-sitemap` generates `/sitemap.xml` automatically. No configuration required. Verify the sitemap at `https://your-domain.com/sitemap.xml` after deployment, then submit it in Google Search Console.

### Schema.org / JSON-LD

An Organisation schema block is included in `_includes/seo-extras.html`. Update the `email`, `logo`, and `contactPoint` values. The include needs to be added to `default.html` inside `<head>`:

```html
{% include seo-extras.html %}
```

---

## Cookie Consent

`_includes/cookie-banner.html` provides a lightweight GDPR/CCPA-style cookie consent banner:

- Shows on first visit if no consent cookie is set.
- **Accept All** → sets `consent_status=all` cookie, hides banner.
- **Reject Non-Essential** → sets `consent_status=essential` cookie, hides banner.
- Consent persists for 365 days.
- **TODO for production:** wire the GA snippet to fire only after `consent_status=all` is confirmed (replace the `// TODO: initialise analytics here` comment in the banner script with your `gtag('config', ...)` call).

> This banner is a **stub** and does not constitute legal compliance. Consult qualified counsel and consider a full CMP (Consent Management Platform) such as Cookiebot, OneTrust, or Termly for production.

---

## Swapping Images

All images currently use Unsplash placeholder URLs. Before launch:

1. Download or create your own images.
2. Place them in `assets/images/`.
3. Replace Unsplash URLs in `index.html` and `_pages/about.html` with local paths:

```html
<img src="{{ '/assets/images/your-image.jpg' | relative_url }}" alt="Descriptive alt text" loading="lazy">
```

4. Replace `assets/images/favicon.svg` with your brand favicon.
5. Add `assets/images/og-default.jpg` (1200×630px) for Open Graph sharing previews.
