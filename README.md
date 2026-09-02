# turnbullbiomedical.com — source files

A plain HTML/CSS static site, hosted free on GitHub Pages, using your existing
`turnbullbiomedical.com` domain (bought at GoDaddy).

Full step-by-step setup (GitHub account, GoDaddy DNS, etc.) is in
`SETUP-GUIDE.md`. This file is the quick reference for day-to-day editing
once it's live.

## File layout

```
index.html            company home page
trips/index.html      "Trips" landing page (your personal projects)
trips/france.html     France 2026 trip page
trips/oregon.html     Oregon coast 2026 trip page
assets/style.css       shared styling for every page
CNAME                  tells GitHub Pages your custom domain (do not delete)
```

## Making a change and publishing it

1. Edit any `.html` file in a text editor (or ask Claude to edit it for you).
2. From this folder, run:
   ```
   git add -A
   git commit -m "describe your change"
   git push
   ```
3. GitHub Pages rebuilds automatically, usually within a minute or two.

## Adding a new page

Copy `trips/france.html` as a template, rename it (e.g. `trips/colorado.html`),
edit the content, and add a link to it from `trips/index.html`. Keep the
`<link rel="stylesheet" href="../assets/style.css">` line so it picks up the
shared look.
