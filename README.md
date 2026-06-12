# WebIQ Animated Contact Page

This is a static GitHub Pages-ready landing page.

## Contents

- `index.html` — the contact page
- `assets/css/styles.css` — all visual styling and animations
- `assets/js/main.js` — particle generation and subtle pointer motion
- `.nojekyll` — keeps GitHub Pages from processing the site with Jekyll

## Contact link

The only clickable link on the page is:

`mailto:iam.administrator@webiq.cloud`

## Deploy to GitHub Pages

1. Create a GitHub repository.
2. Upload the contents of this folder to the repository root.
3. Go to **Settings > Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**.
5. Select your branch, usually `main`, and folder `/root`.
6. Save.
7. Open the GitHub Pages URL after GitHub finishes publishing.

## Local preview

Open `index.html` in a browser, or run a simple local server:

```bash
python -m http.server 8000
```

Then open:

```text
localhost:8000
```

## Validation

The bundle is intentionally self-contained:

- No company logos
- No external website links
- No navigation/header/footer menu links
- No externally hosted company assets
- No third-party scripts
- One email contact link only
