# AGENTS.md — exfield.app

The public website for **Exfield**, the "Don't break the chain" habit
calendar app. Two hand-written pages: the landing page and the privacy
policy that App Store Connect links to. The app itself lives in the
sibling repo `../habit-tracker` (Flutter). Its `AGENTS.md` and `PLAN.md`
hold the product context and launch status.

## Deploy: merging to `main` publishes

- GitHub Pages (legacy build) serves `main` at `/` on the custom domain
  `exfield.app` (set by `CNAME`, HTTPS enforced). There is no build step
  or CI. A merge to `main` goes live within minutes.
- So: work on a feature branch and push it. Radu reviews and merges.
  Never commit to or merge into `main`.
- Never delete or edit `CNAME`: that takes the domain offline.

## Layout

- `index.html`: the landing page (hero X, App Store badge, screenshots).
- `privacy/index.html`: the privacy policy, served at `/privacy/`.
- `img/`: screenshots (light + dark pairs) and Apple's App Store badges.
- `favicon-32.png`, `icon-512.png`, `apple-touch-icon.png`: made from the
  app icon.

## Rules

- **Plain static HTML.** CSS and JS stay inline in each page. No
  frameworks, no build tooling, no `package.json`, no third-party scripts,
  fonts or analytics. The site should load as fast and as privately as
  the app promises to behave.
- **The privacy policy describes real data flows.** It has to match the
  app's actual behaviour (see `../habit-tracker`) and the App Privacy
  answers in App Store Connect / Play Console (RevenueCat: purchase info
  for app functionality and analytics). Only change a claim when the app
  or those answers change, and bump the "Effective date". Flag every
  wording change to Radu. Don't make these edits on your own.
- **The site mirrors the app's look.** The colour tokens in each page's
  `:root` come from `../habit-tracker/app/lib/ui/style.dart`, and both
  pages copy them. Change them in both pages and keep them matched to the
  app. The hero X copies `app/lib/ui/marker_x.dart` (seed 6, 450 ms
  draw). Update it only from the app's source, never by hand.
- **Light and dark everywhere.** Theming follows
  `prefers-color-scheme`. Every screenshot ships as a `-light`/`-dark`
  pair behind a `<picture>`, with `width`/`height` attributes and
  descriptive `alt` text.
- **App Store badge:** use Apple's official artwork unmodified (black in
  light mode, white in dark), at least 40 px tall. Google Play is
  "coming soon" until the Android release. When it ships, use Google's
  official badge the same way.
- Motion honours `prefers-reduced-motion`.
- Language: English for code, comments, commits and all site copy.

## Verifying a change

There are no tests or build. Before calling a change done:

- Open the changed pages in a browser in light **and** dark mode, at
  phone width (~375 px) and desktop width. Check that nothing scrolls
  horizontally.
- Check that every link and asset path resolves. Pages use root-absolute
  paths (`/privacy`, `/favicon-32.png`). Serve the repo root over HTTP
  instead of opening files directly (`file://`).
- After Radu merges, confirm the change is live at https://exfield.app.
