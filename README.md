# Joshua-Elms.github.io

Personal website for Joshua Elms — atmospheric scientist and scientific programmer.

Hosted on GitHub Pages at [joshua-elms.github.io](https://joshua-elms.github.io).

---

## Table of Contents

1. [How the Site Works](#how-the-site-works)
2. [File Structure](#file-structure)
3. [Adding Content](#adding-content)
4. [Changing Colors](#changing-colors)
5. [Changing Fonts](#changing-fonts)
6. [Changing Layout & Spacing](#changing-layout--spacing)
7. [Updating the Resume](#updating-the-resume)
8. [Updating Social Links](#updating-social-links)
9. [Local Development](#local-development)
10. [Deployment](#deployment)
11. [Design Decisions](#design-decisions)

---

## How the Site Works

This is a **static HTML/CSS website** with a tiny bit of JavaScript for one purpose: rendering markdown files as styled HTML. There is no build step, no framework, and no server-side code.

### Technology stack

| What            | How                                                                 |
|-----------------|---------------------------------------------------------------------|
| Pages           | Plain HTML files                                                    |
| Styling         | One CSS file (`assets/css/style.css`) with CSS custom properties    |
| Markdown        | [marked.js](https://marked.js.org) loaded from CDN (~7 KB gzipped) |
| Fonts           | Google Fonts (Merriweather + Source Sans 3)                         |
| Favicon         | SVG tree (`favicon.svg`)                                            |
| Hosting         | GitHub Pages (static file serving)                                  |

### How markdown rendering works

The **About Me** page and all **Project/Writing post** pages use the same pattern:

1. The HTML page includes a `<script>` tag that loads `marked.js` from a CDN.
2. A small inline script fetches a `.md` file (via `fetch()`).
3. `marked.parse(md)` converts the markdown string to HTML.
4. The result is inserted into the page.

This means you write content in plain markdown and the browser renders it — no build step needed.

---

## File Structure

```
/
├── index.html                  ← Home page (no-scroll landing page)
├── post.html                   ← Shared markdown post viewer
├── favicon.svg                 ← Tree favicon (SVG)
├── .nojekyll                   ← Tells GitHub Pages to skip Jekyll
├── .gitignore
│
├── assets/
│   ├── css/
│   │   └── style.css           ← ALL styles live here
│   ├── images/
│   │   ├── headshot-placeholder.svg
│   │   ├── tile-placeholder.svg
│   │   ├── projects/           ← Tile images for projects
│   │   └── writings/           ← Tile images for writings
│   └── resume/
│       └── (resume.pdf)        ← Drop your resume PDF here
│
├── projects/
│   ├── index.html              ← Project tile grid page
│   └── posts/
│       └── example-project.md  ← Example project post (markdown)
│
├── writings/
│   ├── index.html              ← Writing tile grid page
│   └── posts/
│       └── example-writing.md  ← Example writing post (markdown)
│
├── about/
│   ├── index.html              ← About Me page shell
│   └── content.md              ← About Me content (markdown)
│
└── resume/
    └── index.html              ← Resume PDF viewer page
```

---

## Adding Content

### Add a new Project or Writing post

1. **Write your markdown file** and save it:
   - Projects → `projects/posts/my-project-name.md`
   - Writings → `writings/posts/my-writing-name.md`

2. **Optionally add a tile image** (recommended ~400×200px):
   - Projects → `assets/images/projects/my-project.jpg`
   - Writings → `assets/images/writings/my-writing.jpg`

3. **Add a tile** to the grid page. Open `projects/index.html` (or `writings/index.html`) and copy this block inside the `<div class="tile-grid">`:

```html
<a class="tile" href="/post.html?src=projects/posts/my-project-name.md&back=projects/">
  <img class="tile-image" src="../assets/images/projects/my-project.jpg" alt="My Project">
  <div class="tile-body">
    <h3>My Project Title</h3>
    <p>A one-line description of what this project is about.</p>
  </div>
</a>
```

   Key parts to change:
   - `?src=` → path to your `.md` file (relative to site root)
   - `&back=` → where the back arrow goes (e.g., `projects/` or `writings/`)
   - `img src` → path to your tile image
   - `<h3>` → tile title
   - `<p>` → tile description

4. **Commit and push** — that's it.

### Edit the About Me page

Edit `about/content.md`. It's plain markdown. Changes appear immediately (after push to GitHub Pages).

### Add your headshot

1. Save your photo as `assets/images/headshot.jpg` (square, ~400×400px recommended).
2. In `index.html`, find the `<img>` inside `.home-photo` and change the `src`:
   ```html
   <img src="assets/images/headshot.jpg" alt="Joshua Elms headshot">
   ```

### Update the mission statement

In `index.html`, find the `<p class="mission">` element and replace the placeholder text.

---

## Changing Colors

All colors are defined as CSS custom properties at the top of `assets/css/style.css`:

```css
:root {
  --bg:           #FAF9F6;   /* page background (warm off-white)     */
  --bg-alt:       #F0EDE6;   /* card/code background                 */
  --bg-nav:       #FFFFFF;   /* navbar background                    */
  --text:         #2C3E2D;   /* primary text (dark forest green)     */
  --text-light:   #5A6B5C;   /* secondary text                      */
  --accent:       #7D9B76;   /* links, buttons (sage green)         */
  --accent-hover: #5E7D57;   /* hover state                         */
  --accent-light: #D5E8D4;   /* subtle highlights (light mint)      */
  --warm:         #E8DCC8;   /* borders, dividers (beige)           */
  --cool:         #D4E4ED;   /* secondary highlights (light blue)   */
}
```

**To change the color scheme:** edit the hex values. Everything updates automatically.

### Alternate palettes to try

**Cool Blue:**
```css
--accent: #6B9BC3; --accent-hover: #4A7DA8; --accent-light: #D4E4ED;
```

**Warm Terracotta:**
```css
--accent: #C4836B; --accent-hover: #A66B53; --accent-light: #F0DDD4;
```

**Muted Lavender:**
```css
--accent: #9B8EC4; --accent-hover: #7D70A8; --accent-light: #E4DFF0;
```

---

## Changing Fonts

Fonts are set in two places:

1. **The `@import` at the top of `style.css`** — this loads the font files from Google Fonts.
2. **The `--font-heading` and `--font-body` variables** — these apply the fonts.

```css
/* Load fonts */
@import url('https://fonts.googleapis.com/css2?family=Merriweather:wght@400;700&family=Source+Sans+3:wght@400;600;700&display=swap');

/* Apply fonts */
--font-heading: 'Merriweather', Georgia, serif;
--font-body:    'Source Sans 3', 'Segoe UI', Helvetica, Arial, sans-serif;
```

**To change fonts:**
1. Browse [Google Fonts](https://fonts.google.com/).
2. Copy the `@import` URL for your chosen fonts.
3. Replace the `@import` line and update the `--font-*` variables.

### Font sizes

All sizes are in `rem` units (relative to browser default, usually 16px):

```css
--text-xs:   0.85rem;    /* ~14px — small labels     */
--text-sm:   1rem;       /* ~16px — nav links        */
--text-base: 1.125rem;   /* ~18px — body text        */
--text-lg:   1.35rem;    /* ~22px — tile headings    */
--text-xl:   1.75rem;    /* ~28px — section headings */
--text-2xl:  2.25rem;    /* ~36px — page titles      */
--text-3xl:  3rem;       /* ~48px — home page name   */
```

Increase `--text-base` to make all body text larger.

---

## Changing Layout & Spacing

### Max page width

```css
--max-width: 1100px;    /* content won't stretch beyond this */
```

### Spacing scale

```css
--space-xs:  0.5rem;    /* 8px  */
--space-sm:  1rem;      /* 16px */
--space-md:  1.5rem;    /* 24px */
--space-lg:  2.5rem;    /* 40px */
--space-xl:  4rem;      /* 64px */
```

### Tile grid

The tile grid uses CSS Grid with `auto-fill` — tiles automatically wrap to new rows. Change the minimum tile width in `style.css`:

```css
.tile-grid {
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
}
```

Change `300px` to a larger value for wider tiles or smaller for narrower ones.

---

## Updating the Resume

1. Save your resume as `assets/resume/resume.pdf`.
2. The resume page (`resume/index.html`) will display it in an embedded PDF viewer.
3. A download fallback link is included for browsers that can't display PDFs inline.

To display a **publication** or other PDF, you can create a new page following the same pattern as `resume/index.html`.

---

## Updating Social Links

Social links (GitHub, LinkedIn, Google Scholar) are in the navbar of every HTML file. To change a URL, find and replace the `href` value. For example:

```html
<a href="https://github.com/Joshua-Elms" ...>
```

The icons are inline SVGs — no external image files needed. If you want to add a new social icon:

1. Find an SVG icon (e.g., from [Simple Icons](https://simpleicons.org/)).
2. Add a new `<a>` element inside the `<div class="nav-social">` block.
3. Paste the SVG path inside.

---

## Local Development

Since markdown rendering uses `fetch()`, you need a local web server (not just opening files directly in a browser).

### Quick options

**Python (built-in):**
```bash
cd /path/to/Joshua-Elms.github.io
python3 -m http.server 8000
# Open http://localhost:8000
```

**Node.js (if installed):**
```bash
npx serve .
```

**VS Code:** Install the "Live Server" extension, right-click `index.html` → "Open with Live Server."

---

## Deployment

This site is designed for **GitHub Pages**.

1. Push your changes to the `main` branch.
2. In your repo settings → Pages → set Source to "Deploy from a branch" → `main` / `/ (root)`.
3. GitHub serves the site at `https://joshua-elms.github.io`.

The `.nojekyll` file tells GitHub Pages to serve files as-is without Jekyll processing.

### Custom domain (future)

If you buy a domain:
1. Add a `CNAME` file to the repo root containing your domain (e.g., `joshuaelms.com`).
2. Configure DNS with your registrar (CNAME pointing to `joshua-elms.github.io`).
3. Enable HTTPS in the GitHub Pages settings.

---

## Design Decisions

| Decision | Reasoning |
|----------|-----------|
| **No framework / build step** | Maximum simplicity. You can understand every file. Nothing hidden. |
| **CSS custom properties for theming** | Change colors/fonts in one place; everything updates. No preprocessor needed. |
| **Client-side markdown rendering** | Write in `.md`, skip manual HTML conversion. One small CDN library (marked.js, ~7 KB). |
| **Inline SVG icons** | No icon library to load. Icons are just `<svg>` tags in the HTML — fast and self-contained. |
| **Google Fonts** | Professional typography with zero local font files. Easy to swap via the `@import` URL. |
| **No-scroll home page** | First impression fits entirely in the viewport. Uses `height: 100vh` and flexbox centering. |
| **`post.html` shared viewer** | One HTML file renders all markdown posts. Add content by writing `.md` files, not HTML. |
| **Mobile-responsive** | CSS media queries at 768px and 480px adjust layout and font sizes for smaller screens. |
| **`.nojekyll`** | Prevents GitHub Pages from running Jekyll, which can break plain HTML sites or add latency. |
| **`rem` font sizes** | Respects user browser settings. Increasing `--text-base` scales all body text proportionally. |
| **18px base font size** | Optimized for readability — comfortable for all ages without squinting. |

---

## Quick Reference

| Task | What to edit |
|------|-------------|
| Change colors | `assets/css/style.css` → `:root` variables |
| Change fonts | `assets/css/style.css` → `@import` + `--font-*` variables |
| Change font sizes | `assets/css/style.css` → `--text-*` variables |
| Add a project/writing | Write `.md` file + add tile in `projects/index.html` or `writings/index.html` |
| Edit About Me | `about/content.md` |
| Update resume | Replace `assets/resume/resume.pdf` |
| Change headshot | Replace image + update `src` in `index.html` |
| Change mission statement | Edit `<p class="mission">` in `index.html` |
| Update social links | Find/replace `href` in navbar across HTML files |
| Change Google Scholar URL | Replace `https://scholar.google.com/` in all HTML files |
