# Brief: build a careers site as a Job Robot site package

Paste this into the first prompt of your design tool, or save it as the project's instructions file, before you
start. It describes what you are building and the rules the package is checked against on upload. The
`starter/` folder next to this file already follows every rule; the fastest route is to start from a copy of it
and restyle it.

## What you are building

A **site package**: a folder of plain HTML, CSS, images and fonts that a careers site is built from. It is zipped
and uploaded as it is. Nothing in it is compiled or executed on the server.

The product generates the pages that show jobs: the listing at `/jobs/`, the pages for each job, the directories of
jobs by location and category, the search, the apply form and its confirmation. You do **not** build those. You
build the frame they are shown in, and any pages of your own.

The package has three parts.

1. **`package.json`**, the manifest. At minimum:

   ```json
   { "schemaVersion": 1, "name": "Acme careers site by Your Agency, v1.0" }
   ```

2. **`layout.html`**, the frame every generated page is rendered into: the header, navigation and footer, with four
   slots where the product puts its content. It is HTML and CSS only.

3. **Content pages**, optional: your own pages, each a folder with an `index.html`. `index.html` at the root is the
   home page at `/`; `about/index.html` is `/about/`. A content page is a complete HTML document with its own header
   and footer, and it may use JavaScript within the limits below.

Everything else is assets: stylesheets, images and fonts, referenced by relative or root-relative path.

## The layout

- Begin with `<!doctype html>` and `<html lang="...">`.
- Use exactly these four slots, and nothing else that is dynamic. They are replaced by text; there is no logic,
  no loops and no conditionals.

  | Slot | Where | Meaning |
  |---|---|---|
  | `{{head}}` | once, on its own line inside `<head>`, after your own links | the page's description and structured data (not its `<title>`: that is yours, below) |
  | `{{content}}` | once, on its own line inside `<body>`, where the page goes | the listing, the job, the form |
  | `{{title}}` | in `<title>`, which the layout must have, and anywhere else | the page's title, written as below |
  | `{{company}}` | anywhere, as often as wanted | the employer's name |

  The `<title>` the layout must have is written exactly this way:

  ```html
  <title>{{title}} | {{company}}</title>
  ```

- Put `{{content}}` inside a container with an `id` that the skip link targets. Do not wrap it in `<main>`: the
  generated content brings its own `<main>`.
- **No JavaScript at all.** No `<script>`, no `onclick` or any other event handler, no `javascript:` links, no
  `<noscript>`. Also none of `<form>`, `<iframe>`, `<object>`, `<embed>`, `<video>`, `<audio>`, `<canvas>`, `<dialog>`,
  `<template>`, `<input>`, `<button>`, `<select>` or `<textarea>`, and no `<meta http-equiv>`. Navigation is links.
- `<link>` may only be `stylesheet`, `icon`, `apple-touch-icon` or `preload` (with `as` of `font`, `style` or `image`),
  and must name a file in the package. No CDN stylesheets and no Google Fonts links: bring fonts into the package.
- Inline `<style>` and `style=""` are allowed, and are parsed, so they must be valid CSS.
- **Navigation links in the layout are absolute**, `https://careers.acme.com/about/`, not `/about/`. The same
  layout frames the apply form, which is served from a second hostname, and a root-relative link there would point
  to the wrong place. Ask for the site's hostname; the starter uses `https://careers.example.com/`. This is for
  `<a href>` only: the stylesheet, icon and images the layout loads stay root-relative, `/static/site.css`, because
  they must name files in the package, and the product serves them on both hostnames.
- Link to `/jobs/` for the listing, `/jobs/locations/` and `/jobs/categories/` for the directories. Do not draw
  a jobs list, a search box or an apply form of your own in the layout.
- If the employer offers the site's assistant, link to it at `/assist/` on the **apply hostname**, which is the
  career hostname's first label with `-apply` added: `https://careers-apply.acme.com/assist/` beside
  `https://careers.acme.com/`. The starter's footer does. Leave the link out if they do not offer it, because the
  page is not there until they do. Do not build a chat box of your own: the assistant is a page the product serves.
- Style the generated markup. It uses plain elements: the listing is `<main>` with an `<h1>`, then one `<article>`
  per job holding an `<h2>` with the link, a paragraph and a `<time>`, then a `<nav>` for pages. A job page is
  `<main>` with a back link, an `<h1>`, the description, and `<a class="button">` to apply. The apply form is a
  `<form>` with `<label>`s, `<fieldset>`s, inputs and a `<button type="submit">`, with `#form-errors` and
  `[role="alert"]` for errors. The starter's stylesheet covers all of it under the comment *Generated pages*.

## Content pages

- A complete document: `<!doctype html>`, `<html lang>`, `<head>` before anything else, `<title>`, a viewport meta.
- Repeat the header and footer from the layout, with root-relative links this time: `/about/`, `/jobs/`.
- JavaScript is allowed, with limits: no event-handler attributes, no `javascript:` links, and every `<script src>`,
  `<iframe src>` and `<form action>` must name a file in the package or a host the manifest declares. An inline
  `<script>` runs only if the manifest says `"allowInlineScripts": true`, and the employer sees that at activation.
  None of `<object>`, `<embed>`, `<base>`, `<meta http-equiv>`, `srcdoc`.
- Declared hosts, by kind, each an `https://` origin in lower case with no path, at most 20 per kind:

  ```json
  {
    "schemaVersion": 1,
    "name": "Acme careers site by Your Agency, v1.0",
    "allowInlineScripts": true,
    "hosts": {
      "scripts": ["https://www.googletagmanager.com"],
      "connections": ["https://www.google-analytics.com", "https://*.analytics.google.com"],
      "frames": ["https://www.youtube-nocookie.com"],
      "images": ["https://www.google-analytics.com"]
    }
  }
  ```

  The kinds are `scripts`, `styles`, `images`, `fonts`, `connections`, `frames` and `forms`. Prefer no scripts at
  all: a site with none needs no declarations and nothing for the employer to approve.
- No product data is available in a content page. There is no slot, no jobs feed and no company name; write the
  name out.

## Privacy wording for the assistant

If the employer offers the assistant and lets it compare a candidate's resume with the open roles, the privacy notice
the site links to has to say so. The page is usually the employer's own, outside the package; give them this
paragraph to adapt:

> **Careers assistant.** Our careers site offers an assistant that answers questions about our open roles from the
> roles and pages the site publishes. If you choose to upload your resume to it, its text is read by an automated
> assistant to compare it with our open roles, with email addresses, telephone numbers and links removed first. The
> assistant does not keep it: the document is deleted once the comparison is made, the comparison is shown to you
> alone, and it is not part of any application. Nothing reaches us unless you apply. The assistant runs on a model
> service our platform operator has approved for this site's processing jurisdiction. What you ask it is not
> stored.

## Files and paths

- Accepted: HTML, CSS, JavaScript, Markdown, PNG, JPEG, GIF, WebP, ICO, WOFF2 and SVG. Types are checked by
  content, so a PNG named `.jpg` is refused. Nothing else: no `.txt`, `.json` other than the manifest, `.map`,
  `.ttf`, `.otf`, `.woff`, `.mp4`, `.pdf`, hidden files or `.gitignore`. Font licences stay outside the package.
- Fonts: WOFF2 only, in the package, loaded by `@font-face` with `url()` relative to the stylesheet. Fontsource
  publishes WOFF2 subsets of the Google Fonts catalogue.
- SVG is sanitized to a static image: shapes, text, gradients, masks and filters stay; scripts, links, foreign
  objects and event handlers do not.
- Paths are ASCII letters, digits, `.`, `-`, `_` and `@`, no leading dot, at most 200 characters, and no two
  paths may be the same apart from case.
- **Reserved**: these routes are generated for every site, and a package cannot place a file at them:
  `/jobs/`, `/assets/`, `/page/`, `/apply/` and `/api/` (anything under them), and `/sitemap.xml`, `/robots.txt`,
  `/llms.txt`, `/llms-full.txt`, `/manifest.json`, `/index.md`. Bundlers default to `assets/`; use `static/` instead.
- CSS is parsed. `url()` and `@import` must name files in the package, relative to the stylesheet.
- Limits: 20 MiB archive, 500 files, 5 MiB per file, 50 MiB unpacked. No symbolic links, no `..`.

## Accessibility

Activation is blocked until the listing, a job, the apply form and its confirmation, rendered in your layout,
pass WCAG 2.2 AA checks in a browser at 320 pixels and at 1280. Content pages are checked and reported but do not
block. What fails most often:

- text contrast under 4.5:1, especially muted grey on white and white on a light accent;
- links and buttons with no visible focus style;
- the page wider than 320 pixels, from a fixed width, a wide image or a long unbroken word;
- missing `lang` on `<html>`, a missing `<title>`, headings out of order, an image without `alt`;
- a `<nav>` without an `aria-label` when there is more than one, and no skip link.

## Before upload

Zip the folder's contents, not the folder:

```sh
cd my-package && zip -qrD -X ../my-package.zip .
```

The upload answers with every problem it found, by file and line, or with nothing to fix. Paste the report back
into your tool and fix each line; the rules do not bend, and nothing is corrected silently.
