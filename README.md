ontained `index.html` with embedded CSS and vanilla JavaScript —
no build step, no framework, no dependencies beyond two Google Fonts.

## Why this approach

- **Plain HTML/CSS/JS.** For a five-section marketing site, a framework
  (React, Vue, etc.) adds a build step and dependency graph without adding
  capability. A single static file is easiest to host, review, and hand off.
- **One real subject, not generic placeholders.** Rather than "Feature A /
  Feature B" boilerplate, the whole page is written around one product and
  audience, so the layout choices (a job-ticket graphic, a job list instead
  of feature cards, testimonials styled as invoice stubs) come from the
  subject matter instead of a generic template.
- **Design tokens as CSS custom properties.** Colors, fonts, and the max
  content width are defined once at the top of `<style>` (`:root`) so the
  palette and type scale stay consistent and are easy to re-theme.
- **Accessibility and resilience built in, not bolted on:** visible focus
  states, `aria-live` on the form status message, a `prefers-reduced-motion`
  override, and semantic landmarks (`header`, `main`, `section`, `footer`).

## Structure

```
index.html   — everything: markup, styles, and JavaScript
README.md    — this file
```

Sections, in order:

1. **Navbar** — sticky, collapses to a hamburger menu under 720px.
2. **Hero** — headline, subhead, two CTAs, and a hand-built "job ticket" SVG/CSS graphic standing in for a product screenshot.
3. **Features** — five items presented as a job list (numbered line items with rules), not cards, to fit the paperwork/ledger metaphor.
4. **Testimonials** — three quotes styled as invoice stubs.
5. **Contact form** — name, trade, email, optional phone, and a message field, with full client-side validation (see below).
6. **Footer.**

## Form validation

Validation is plain JavaScript, no library:

- Required fields (`name`, `trade`, `email`, `message`) are checked for
  minimum length; `email` is checked against a standard email pattern;
  `phone` is optional but validated if filled in.
- Fields validate on blur, and re-validate on every keystroke *after* a field
  has already been marked invalid, so errors clear as soon as they're fixed
  rather than staying stuck.
- On submit, all fields are re-checked; if anything fails, focus moves to the
  first invalid field and an inline message explains what's wrong.
- On success, the user sees an inline confirmation and the form resets.

**Note:** the form does not currently send data anywhere — there's no backend
in this deliverable. The `form.addEventListener('submit', ...)` handler in
`index.html` has a comment marking exactly where to add a real submission
(e.g. a `fetch()` call to Formspree, Netlify Forms, or your own API).

## Running it locally

No build step. Either:

- Double-click `index.html`, or
- Serve it locally: `python3 -m http.server 8000` from this folder, then
  visit `http://localhost:8000`.

## Deploying

### Option A — GitHub Pages (free, static)

```bash
git init
git add .
git commit -m "Anchor landing page"
git branch -M main
git remote add origin https://github.com/<your-username>/anchor-landing.git
git push -u origin main
```

Then in the repo on GitHub: **Settings → Pages → Source: Deploy from a
branch → Branch: main / (root)**. GitHub will publish it at
`https://<your-username>.github.io/anchor-landing/` within a minute or two.

### Option B — Netlify (drag-and-drop)

Go to [app.netlify.com/drop](https://app.netlify.com/drop) and drag the
project folder in. It deploys instantly and gives you a live URL with no
git or CLI required.

### Option C — Vercel

```bash
npm i -g vercel
vercel
```
Follow the prompts; Vercel detects it as a static site automatically.

## Browser support / responsiveness

Tested breakpoints: desktop (1120px+ content width, capped by `--maxw`),
tablet (~880px, two-column sections collapse to one), and mobile (~720px,
navbar collapses to a hamburger, form fields stack). Uses CSS Grid and
custom properties, both supported in all evergreen browsers.
