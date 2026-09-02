# Hosting turnbullbiomedical.com on GitHub Pages — setup guide

**The plan:** GitHub Pages hosts the site for free (no hosting bill, ever, for
a static site like this). GoDaddy stays your domain registrar — you're just
pointing its DNS at GitHub instead of at GoDaddy's own website builder. Total
new cost: $0/month. You keep paying GoDaddy only for the domain renewal you
already pay for.

**Site structure:** one site, one repo. The company home page lives at
`turnbullbiomedical.com`, and personal project pages (trips, etc.) live under
`turnbullbiomedical.com/trips/`. This keeps everything on one domain with a
single DNS setup. If you'd rather keep personal stuff off the company domain
entirely, or on its own `trips.turnbullbiomedical.com` subdomain, see the
"Alternatives" section at the end — it's an easy change later either way.

---

## 1. Create a GitHub account (if you don't have one)

Go to [github.com/signup](https://github.com/signup) and create a free
account. The free tier is all you need for this.

## 2. Create a new repository

1. Click the **+** in the top right of GitHub → **New repository**.
2. Name it `turnbullbiomedical.com` (the name doesn't actually matter once a
   custom domain is set up, but this is a clear one).
3. Set it to **Public** (GitHub Pages requires a public repo on the free
   tier — this only exposes your website's files, which are public on the
   web anyway once the site is live).
4. Don't add a README/gitignore/license — leave it empty. Click **Create
   repository**.

## 3. Push the site files to GitHub

You should have received a folder (or zip) called `turnbullbiomedical-site`
containing `index.html`, the `trips/` folder, `assets/style.css`, `CNAME`,
and this guide. Unzip it if needed, then open a terminal in that folder and
run:

```bash
git init
git add -A
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/turnbullbiomedical.com.git
git push -u origin main
```

Replace `YOUR-USERNAME` with your actual GitHub username. GitHub will prompt
you to sign in the first time you push.

*(If you don't have `git` installed: on a Mac, open Terminal and type `git`
— macOS will offer to install it. On Windows, install
[Git for Windows](https://git-scm.com/download/win).)*

## 4. Turn on GitHub Pages

1. In your repository on GitHub, go to **Settings** → **Pages** (left
   sidebar).
2. Under **Build and deployment → Source**, choose **Deploy from a branch**.
3. Under **Branch**, choose `main` and `/ (root)`, then **Save**.
4. GitHub will build the site — this takes a minute or two. It will show you
   a temporary URL like `https://your-username.github.io/turnbullbiomedical.com/`.
   Confirm that loads before moving on.

## 5. Point your custom domain at GitHub Pages

Still in **Settings → Pages**:

1. Under **Custom domain**, type `turnbullbiomedical.com` and click **Save**.
   (This is already in the `CNAME` file you pushed, but entering it here too
   makes GitHub double-check the DNS for you.)
2. Leave this tab open — you'll come back to check the "DNS check
   successful" message after the next step.

## 6. Update DNS at GoDaddy

1. Log in at [godaddy.com](https://godaddy.com) → **My Products** → find
   `turnbullbiomedical.com` → **DNS** (or **Manage DNS**).
2. You'll see a list of DNS records. Delete or edit any existing **A**
   records for the root/`@` entry, and add four **A** records, all with
   host `@`, pointing to GitHub Pages' IP addresses:

   | Type | Name | Value |
   |------|------|---------------------|
   | A | @ | 185.199.108.153 |
   | A | @ | 185.199.109.153 |
   | A | @ | 185.199.110.153 |
   | A | @ | 185.199.111.153 |

3. Add one **CNAME** record so `www.turnbullbiomedical.com` also works:

   | Type | Name | Value |
   |------|------|-------|
   | CNAME | www | YOUR-USERNAME.github.io |

4. Save. DNS changes can take anywhere from a few minutes up to 24 hours to
   fully propagate (GoDaddy is usually on the faster end).

## 7. Confirm and enable HTTPS

1. Back in GitHub's **Settings → Pages**, wait for **"DNS check
   successful"** to appear next to your custom domain (refresh the page
   after 15–30 minutes if it's not there yet).
2. Once that appears, check the **Enforce HTTPS** box. This gives you a free
   SSL certificate (the padlock in the browser) — GitHub handles the
   renewal automatically forever.
3. Visit `https://turnbullbiomedical.com` — your site should load.

---

## Making changes later

See `README.md` in the site folder — it's just edit the HTML, then
`git add -A && git commit -m "..." && git push`. Changes go live within a
minute or two of pushing.

## Alternatives considered

- **Subdomain for trips** (`trips.turnbullbiomedical.com`) instead of
  `/trips/`: add a fifth DNS record — a CNAME for host `trips` pointing to
  `YOUR-USERNAME.github.io` — and create a second GitHub Pages site (a
  second repo) for that subdomain. More separation, a bit more setup.
- **Fully separate free hosting for trips** (e.g. a free
  `yourname.github.io` site with no custom domain, unrelated to the company
  brand): same GitHub Pages process, just skip the custom-domain and CNAME
  steps for that repo.
- **No-code builder** (Carrd, Squarespace, Wix): easier day-to-day editing
  with no git involved, but typically $10–20/month and less control over
  structure. Worth it mainly if you expect to update the site often and
  don't want to touch code at all.
- **Cloudflare Pages / Netlify** instead of GitHub Pages: functionally very
  similar (also free for static sites), slightly more setup since you'd
  route DNS through a different provider's nameservers or CNAME target.
  GitHub Pages was chosen here as the simplest path since it needs the
  fewest moving parts.
