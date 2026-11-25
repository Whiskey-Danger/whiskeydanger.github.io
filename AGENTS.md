# AGENTS.md

This is the Whiskey Danger studio site at whiskeydanger.com. It's a plain static site served by GitHub Pages from `main`. There's no build step, no framework and no package manager, so every page is a self-contained `.html` file.

## Layout

```
index.html                 Homepage: consulting pitch, then the "Other musings" project list
whiskey-danger-logo.png
favicon.png, apple-touch-icon.png, og.png   Homepage icons and link-preview image
CNAME                      whiskeydanger.com. Don't change it.
<project>/
  index.html               Project landing page, served at /<project>/
  privacy.html             Privacy policy, if the project is an app
  assets/                  Images, icons, sounds for that project only
```

`pacachat/` is the reference example. Copy its structure.

## Adding a new project

1. **Pick a slug.** Use lowercase with no spaces, such as `pacachat`. It becomes the URL `whiskeydanger.com/<slug>/`, so don't rename it once it's live.

2. **Create `<slug>/index.html`.** Keep it to one file with inline `<style>` and `<script>`. Fonts can come from Google Fonts. Each project can have its own look and doesn't need to match the homepage.
   - Inside the project, use relative paths: `assets/icon.png`, `privacy.html`.
   - Link back to the studio with `<a href="/">Whiskey Danger</a>` in the footer.
   - Include in `<head>`: `title`, `meta description`, `og:title`, `og:description`, `og:image`, `og:url`, `theme-color`, favicon and apple-touch-icon. Use absolute `https://whiskeydanger.com/<slug>/...` URLs for `og:image` and `og:url`.
   - For an iOS app, add `<meta name="apple-itunes-app" content="app-id=...">`.
   - Respect `prefers-reduced-motion` for any animation.

3. **Add assets to `<slug>/assets/`.** The usual set:
   - `favicon.png`: small square icon
   - `apple-touch-icon.png`: 180×180
   - `og.jpg`: 1200×630 social share image
   - `<slug>-mark.png` or similar: a square icon for the homepage card. Keep it small, around 15 KB.

4. **Add a privacy policy for apps** at `<slug>/privacy.html`. App Store listings link to this URL, so don't move it after the app ships. Update its "Last Updated" date whenever the content changes.

5. **List it on the homepage.** In `index.html`, add an `<li>` to `.work-list` under "Other musings":

   ```html
   <li>
       <a class="work-item" href="/<slug>/">
           <img src="/<slug>/assets/<slug>-mark.png" alt="">
           <span><b>Project Name</b><small>One short line about what it is</small></span>
       </a>
   </li>
   ```

   For a project that isn't live yet, use a `<div class="work-item soon">` instead of the link and put `<span class="soon-tag">Coming soon</span>` after the name and description (it sits in the card's top-right corner), like the Venn card. Swap it for a link when the page ships.

   The card styles already exist, so don't add new CSS for a single project. The `img` has a blue (`#007AFF`) background by default. If the new icon needs a different background, add an inline `style="background: ..."` on that `img`.

## Checking your work

Serve the site from the repo root so absolute paths like `/pacachat/` resolve correctly:

```bash
python3 -m http.server 8000
# open http://localhost:8000/ and http://localhost:8000/<slug>/
```

Check:
- The homepage card links to the project, and its icon shows.
- The project page's assets, privacy link and the "Whiskey Danger" link back to `/` all work.
- The page works at phone width, around 375px.

## Shipping

Pushing to `main` deploys the site, usually within a minute. Keep commits small and descriptive, for example `Add <Project> landing page` and then `Homepage: add <Project> to Other musings`.
