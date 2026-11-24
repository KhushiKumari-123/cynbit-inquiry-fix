# Cynbit Inquiry Form — Fix & Improvement

A working prototype that fixes a sticky-header overlap bug on Cynbit's "AI Suite Inquiry" form and turns the long single-page form into a validated, accessible multi-step flow.

## Problem

On mobile, the sticky top header overlaps form content while scrolling — the "Select Goal" dropdown and nearby labels get cut off behind the header. The form is also a single long page with no inline validation, so users can submit incomplete or malformed data (e.g. invalid email).

## What this fixes

- **Header overlap**: content area uses `scroll-margin-top` and explicit scroll targeting so no field is ever hidden behind the sticky header.
- **Validation**: each field is validated on "Continue" with inline, accessible error messages (`aria-live` region announces errors to screen readers).
- **Conversion**: the form is split into 3 short steps (contact → project details → review & consent) with a progress indicator, which typically reduces abandonment versus one long form.

## Tech stack

Plain HTML/CSS/JS — no build step, no dependencies. Chosen deliberately so the prototype:
- Deploys anywhere in seconds (static file)
- Is trivial for a reviewer to read top-to-bottom in one file
- Demonstrates the fix without framework overhead masking the actual change

## Run locally

Just open `index.html` in a browser, or serve it:

```bash
npx serve .
```

## Deploy (free, ~2 minutes)

**Vercel**
```bash
npm i -g vercel
vercel --prod
```

**Netlify** — drag the folder into https://app.netlify.com/drop

**GitHub Pages**
```bash
git init
git add .
git commit -m "Initial commit: fixed multi-step inquiry form"
git branch -M main
git remote add origin <your-repo-url>
git push -u origin main
```
Then enable Pages in repo Settings → Pages → Deploy from branch `main`, folder `/root`.

## Suggested commit history (clean, incremental)

```
1. chore: scaffold static project structure
2. fix: resolve sticky-header overlap with scroll-margin-top
3. feat: add client-side validation with accessible error states
4. feat: convert single-page form into 3-step flow with progress bar
5. docs: add README and technical report
```
