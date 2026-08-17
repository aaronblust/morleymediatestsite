# Morely Media — website

A Jekyll site for Morely Media (videography/media, Juneau, AK), built
to be edited by hand in Markdown — no build tools required for
day-to-day changes, GitHub does the building for you.

## How the site is organized

```
index.md              → Home page: hero photo, bio, services
projects.md            → The "Work" grid page (intro text only)
_projects/              → One file per project. Add a new .md file here
                            to add a new project to the grid automatically.
assets/images/          → All photos. Placeholder .svg files live here now.
assets/css/style.css    → All styling — colors, fonts, spacing.
_layouts/, _includes/   → Page templates. You shouldn't need to touch
                            these for normal content edits.
_config.yml             → Site title, nav links, your domain.
CNAME                   → Your custom domain goes here.
```

## Everyday edits

### Change the home page bio / photo
Open `index.md`.
- `hero_image:` — path to Finn's photo (see "Adding real photos" below).
- `headline:` — the big line of text next to the photo.
- The bio paragraph(s) below the `---` line are what shows under the headline.
- `services:` — the three cards under the hero. Edit `title` and
  `description` for each, or add a fourth block the same shape.

### Add a new project
1. Duplicate any file in `_projects/`, e.g. copy `project-one.md` to
   `project-four.md`.
2. Edit the front matter at the top (between the `---` lines):
   - `title` — project name
   - `order` — controls its position in the grid (lower = earlier)
   - `summary` — one line shown on the grid card
   - `cover` — path to the cover image
   - `client`, `role`, `year` — shown on the project page (optional,
     delete a line to hide that field)
   - `gallery` — list of extra image paths shown below the description
3. Write the project description as normal text below the `---`.

It will appear on `/projects/` automatically — no other file needs editing.

### Remove a project
Delete its file from `_projects/`.

### Adding real photos
1. Drop your image files into `assets/images/` (JPG or PNG, reasonably
   compressed — aim under ~500KB each for fast loading).
2. Update the relevant path in the front matter (`hero_image:`,
   `cover:`, or an entry in `gallery:`) to point at the new filename,
   e.g. `/assets/images/finn-portrait.jpg`.
3. Delete the placeholder `.svg` once replaced, if you like — it's not required.

Recommended aspect ratios (the placeholders are already this shape):
- Home hero photo: portrait, roughly 4:5
- Project cover: landscape, roughly 3:2
- Gallery photos: roughly 4:3

### Colors and fonts
All design tokens are at the top of `assets/css/style.css` under `:root`:
```css
--color-stone:    #E7E2D6;  /* page background */
--color-offwhite: #F6F4EE;  /* light surfaces */
--color-ink:      #2F3A2F;  /* text, dark surfaces */
--color-copper:   #9C6B45;  /* accent — links, labels, buttons on hover */
--color-moss:     #6B7353;  /* secondary accent */
```
Change a hex value and it updates everywhere that color is used.

## Previewing changes before they go live

**Easiest — GitHub's web editor:** edit any `.md` file directly on
github.com, commit to a branch, and open a pull request. GitHub shows
a diff; merge when you're happy. The live site only updates once
changes land on your default branch (usually `main`).

**Full local preview (optional, more control):**
```bash
bundle install
bundle exec jekyll serve
```
Then open `http://localhost:4000`. Requires Ruby installed
(see https://www.ruby-lang.org for install instructions).

## Deploying — one-time setup

1. Create a new GitHub repository and push this folder to it:
   ```bash
   git init
   git add .
   git commit -m "Initial site"
   git branch -M main
   git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO.git
   git push -u origin main
   ```
2. In the repo on GitHub: **Settings → Pages** → under "Build and
   deployment," set Source to **Deploy from a branch**, branch
   `main`, folder `/ (root)`. Save.
3. **Connect your domain:**
   - Edit `CNAME` in this repo — replace `www.yourdomain.com` with
     your actual domain, commit and push.
   - Also update `url:` in `_config.yml` to match.
   - At your domain registrar, add these DNS records (adjust if you
     want the apex/root domain instead of `www` — GitHub's docs below
     cover both):
     - `CNAME` record: `www` → `YOUR-USERNAME.github.io`
     - If you also want the bare domain (`yourdomain.com` without
       `www`) to work, add `A` records pointing at GitHub's IPs —
       see https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site
   - Back in **Settings → Pages**, enter your custom domain in the
     "Custom domain" field and enable "Enforce HTTPS" once it's
     verified (can take a few minutes to a few hours).

That's it — from then on, any commit to `main` redeploys the live site
within a minute or two.
