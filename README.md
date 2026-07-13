# Reneau Law Firm — static website

Plain static HTML site (no build step, no framework). Ready to host on GitHub Pages.

## Structure
```
index.html                 Homepage (video hero)
about.html                 Attorney profile
contact.html               Contact + consultation form
negligent-security.html    ┐
victim-of-crime.html       │
wrongful-death.html        │
premises-liability.html    ├─ Practice-area pages
car-accidents.html         │
slip-and-fall.html         │
medical-malpractice.html   ┘
assets/                    Logo, photos, credential logos, hero video (shared by all pages)
.nojekyll                  Tells GitHub Pages to serve files as-is (no Jekyll build)
CNAME                      (add later — only when pointing the custom domain)
```
All links are relative, so the site works both at a project URL
(`username.github.io/repo/`) and at the root of a custom domain.

---

## Migration steps

### 1. Create the repository
- On GitHub: **New repository** → name it (e.g. `reneau-law`) → **Public** → Create.
- Public is required for GitHub Pages on the free plan.

### 2. Push the files
**Option A — web upload (no tools):** on the empty repo page, "uploading an existing file",
then drag the *contents* of this folder (all `.html` files, the `assets/` folder, `.nojekyll`).
Commit.

**Option B — git command line:**
```bash
cd reneau-github
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/USERNAME/reneau-law.git
git push -u origin main
```

### 3. Turn on GitHub Pages
Repo **Settings → Pages**. Under "Build and deployment":
- Source: **Deploy from a branch**
- Branch: **main** / folder **/ (root)** → Save.

Wait ~1 minute. The page shows a live URL like
`https://USERNAME.github.io/reneau-law/`.

### 4. Test on the github.io URL
Open the live URL and click through every page: nav dropdown, footer links,
practice pages, the contact form, and confirm the homepage video plays and all
images load. **This is as far as you can go without touching DNS** — the site is
fully live and reviewable here.

---

## When the client is ready to switch DNS (final step)

Do the GitHub side first, then hand the DNS records to whoever controls the domain.

### 5a. Add the custom domain on GitHub
Repo **Settings → Pages → Custom domain** → type the domain
(recommended: `www.example.com`) → **Save**.
This commits a `CNAME` file to the repo. (If you push from the command line
afterward, `git pull` first so you don't lose it.)

Recommended: verify the domain first under
**Settings → Pages → "Verify"** (adds a `TXT` record) to prevent takeovers.

### 5b. DNS records for the client to add
GitHub Pages custom-domain values (verified current):

**Apex / root domain (`example.com`) — four A records:**
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```
Optional IPv6 (AAAA records):
```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

**www subdomain — one CNAME record:**
```
www   CNAME   USERNAME.github.io.
```
(Point it at `USERNAME.github.io`, not at the repo path.)

Set the custom domain to `www` and add both the apex A records and the www
CNAME — GitHub then auto-redirects between them. Remove any old A/CNAME records
from the domain's previous host first.

### 5c. Enforce HTTPS
After DNS resolves (can take up to 24h; usually much less), return to
**Settings → Pages** and tick **Enforce HTTPS**. GitHub issues the TLS
certificate automatically once the domain points correctly.

---

## Notes
- **Video (`assets/2024/06/reneau-header-backdrop4.mp4`, ~20 MB)** is a normal file,
  well under GitHub's 100 MB limit. Do **not** put it in Git LFS — GitHub Pages
  does not serve LFS files.
- Keep `.nojekyll` — it avoids any Jekyll processing and speeds up publishing.
- **Contact form** currently uses a placeholder `mailto:` action
  (`info@reneaulawfirm.com`) in `contact.html`. Replace it with the firm's real
  inbox, or wire it to a form service (Formspree, Netlify Forms, etc.) before launch.
- Practice-area copy is professional placeholder text — have the firm review it
  for accuracy and any state-bar advertising requirements.
