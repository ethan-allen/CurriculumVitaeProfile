# Testing Checklist — Corporate Site

Use this checklist before every production release. Mark items ✅ when passing, ❌ when failing (log issue), and ⏭️ when not applicable.

---

## 1. Build & Deployment

- [ ] `bundle exec jekyll build` completes without errors or warnings
- [ ] `bundle exec jekyll serve` starts successfully on `localhost:4000`
- [ ] GitHub Actions workflow (`deploy.yml`) passes on push to `main`
- [ ] Deployed site is reachable at the expected GitHub Pages URL
- [ ] `baseurl` in `_config.yml` matches the repo sub-path (if applicable)

---

## 2. Cross-Browser Testing

Test each browser at Desktop (≥1280px), Tablet (768px), and Mobile (375px viewport).

| Browser | Version | Desktop | Tablet | Mobile |
|---|---|---|---|---|
| Chrome | Latest | [ ] | [ ] | [ ] |
| Firefox | Latest | [ ] | [ ] | [ ] |
| Safari | Latest | [ ] | [ ] | [ ] |
| Edge (Chromium) | Latest | [ ] | [ ] | [ ] |
| Chrome Android | Latest | [ ] | [ ] | — |
| Safari iOS | Latest | — | [ ] | [ ] |

**Per-browser checks:**
- [ ] Hero renders at full viewport height with no clipping
- [ ] GSAP animation plays (or content is visible immediately if GSAP fails to load)
- [ ] Lottie background visible at low opacity behind hero text
- [ ] Nav hamburger opens/closes on mobile
- [ ] Services dropdown opens on click; closes on outside click
- [ ] Contact form submits and success message appears
- [ ] Cookie banner appears on first visit; dismissed correctly
- [ ] Footer links resolve to correct pages
- [ ] No horizontal scroll at any breakpoint
- [ ] Fonts render correctly (Inter, Barlow Condensed)

---

## 3. Responsive / Layout

- [ ] Hero text does not overflow at 375px mobile
- [ ] Hero visual panel hidden below `lg` breakpoint (1024px)
- [ ] Navigation collapses to hamburger below `lg` breakpoint
- [ ] Services cards reflow: 3-col → 2-col → 1-col at correct breakpoints
- [ ] Stats strip hidden below `md` (768px) — no layout break
- [ ] Contact form row collapses to single column on mobile
- [ ] Footer grid collapses correctly on small screens
- [ ] All images have `loading="lazy"` or `loading="eager"` as appropriate
- [ ] No fixed-width elements causing overflow

---

## 4. Animation Quality

- [ ] Hero headline words animate in with stagger (desktop, no reduced-motion)
- [ ] Eyebrow / subtitle / CTA buttons reveal in sequence
- [ ] Parallax on particles is smooth (no jank at 60fps)
- [ ] Scroll-out fade on hero content triggers correctly
- [ ] Below-fold sections (cards, stats) reveal on scroll
- [ ] Counter animation fires once per viewport entry
- [ ] Section rule wipe animation plays on scroll
- [ ] **Reduced-motion test:** with `prefers-reduced-motion: reduce` set in OS settings:
  - [ ] All GSAP animations are skipped
  - [ ] Hero text is immediately visible (opacity 1)
  - [ ] No motion-triggered scroll handlers running

---

## 5. Accessibility (WCAG 2.2 Level AA)

### Keyboard Navigation
- [ ] Skip-to-content link visible on focus (Tab from address bar)
- [ ] All interactive elements reachable by Tab in logical order
- [ ] Dropdown menu opens with Enter/Space; items navigable with arrow keys
- [ ] Dropdown closes with Escape; focus returns to trigger button
- [ ] Mobile hamburger menu operable by keyboard
- [ ] Contact form submittable by keyboard only
- [ ] Cookie banner buttons reachable and operable by keyboard
- [ ] No keyboard trap anywhere on the site

### Screen Reader
- [ ] Page titles announced correctly on navigation
- [ ] Hero headline: `sr-only` original text present for screen readers
- [ ] `aria-hidden="true"` on decorative elements (orbs, particles, Lottie)
- [ ] `aria-label` present on icon-only buttons (hamburger, scroll indicator)
- [ ] Form labels associated with inputs (`for`/`id` pairs correct)
- [ ] Required fields marked with `aria-required="true"`
- [ ] Form success/error messages use `role="alert"` or `aria-live`
- [ ] Landmarks present: `<header role="banner">`, `<main id="main-content">`, `<footer role="contentinfo">`, `<nav aria-label>`

### Colour Contrast
- [ ] Body text on white: ≥ 4.5:1 (target: `#1a2332` on `#fff` ≈ 14.7:1 ✓)
- [ ] Hero white text on navy: ≥ 4.5:1
- [ ] Muted text (`#5a6e82`) on white: ≥ 4.5:1 (verify — borderline)
- [ ] Button labels on blue background: ≥ 4.5:1
- [ ] Teal eyebrow on dark background: ≥ 3:1

### Images
- [ ] Decorative images have `alt=""` or `aria-hidden="true"`
- [ ] Informative images have descriptive `alt` text
- [ ] SVG icons included via `<img>` have `alt`; inline SVGs have `aria-hidden`

### Forms
- [ ] Error messages are associated with the relevant field
- [ ] Privacy checkbox is labelled clearly
- [ ] Focus ring visible on all interactive elements

---

## 6. SEO & Meta

- [ ] `<title>` tag present on all pages; format: `Page Title | Site Name`
- [ ] `<meta name="description">` present and ≤160 characters on all pages
- [ ] Open Graph tags present: `og:title`, `og:description`, `og:image`, `og:url`, `og:type`
- [ ] Twitter Card tags present
- [ ] Canonical link present
- [ ] `robots.txt` accessible at `/robots.txt`
- [ ] `sitemap.xml` accessible at `/sitemap.xml` and contains all pages
- [ ] Schema.org `Organization` JSON-LD present on home page
- [ ] No duplicate `<h1>` on any page
- [ ] Heading hierarchy is logical (h1 → h2 → h3, no skips)
- [ ] All links have descriptive text (no "click here")

---

## 7. Performance

Run Lighthouse (Chrome DevTools → Lighthouse) on the deployed URL.

| Metric | Target | Result |
|---|---|---|
| Performance | ≥ 90 | |
| Accessibility | ≥ 95 | |
| Best Practices | ≥ 90 | |
| SEO | ≥ 95 | |
| LCP (Largest Contentful Paint) | < 2.5s | |
| CLS (Cumulative Layout Shift) | < 0.1 | |
| FID / INP | < 200ms | |

**Performance quick-wins to check:**
- [ ] Hero image has `loading="eager"` (above fold) and is an appropriate size
- [ ] Below-fold images have `loading="lazy"`
- [ ] GSAP and Lottie scripts are loaded only on the home layout
- [ ] Sass output is compressed (`style: compressed` in `_config.yml`)
- [ ] No render-blocking resources beyond Google Fonts
- [ ] Google Fonts loaded with `display=swap` (check URL param)

---

## 8. Security

- [ ] No API keys, tokens, or secrets committed to the repo
- [ ] Formspree honeypot field present (`name="_honey"`, `style="display:none"`)
- [ ] Contact form uses HTTPS endpoint (Formspree default)
- [ ] External links in footer use `rel="noopener noreferrer"` where `target="_blank"` is used
- [ ] CSP (Content Security Policy) header configured if using a CDN or Netlify (not required for basic GitHub Pages)

---

## 9. Content & Copy

- [ ] All placeholder text (`[Replace with…]`, `Placeholder — …`) replaced with real content
- [ ] Unsplash placeholder images replaced with licensed / owned images
- [ ] Real contact email/phone/address in `_pages/contact.html`
- [ ] Real company name replacing "Acme Corp" in `_config.yml`
- [ ] Privacy Policy, Terms of Use, and Accessibility Statement reviewed by counsel
- [ ] Formspree endpoint updated with live form ID
- [ ] Google Analytics Measurement ID updated
- [ ] Footer copyright year auto-updates (JS snippet present ✓)
- [ ] Favicon updated with brand asset

---

## 10. Pre-Launch Sign-Off

- [ ] Stakeholder review of all pages complete
- [ ] Legal review of Privacy Policy and Terms
- [ ] All checklist items above marked ✅ or ⏭️ with no blocking ❌
- [ ] DNS / custom domain configured (if applicable)
- [ ] Google Search Console property set up; sitemap submitted

---

*Checklist version 1.0 — update this file after each major release.*
