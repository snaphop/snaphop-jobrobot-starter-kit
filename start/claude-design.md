# Start in Claude Design

How to design an employer's career site in Claude Design and upload it as a Job Robot site package. The rules the package is
checked against are all in [`BRIEF.md`](../BRIEF.md); this page says only how to get them into Claude Design and
what its export needs afterwards.

## Before you design

1. Start a new project for the employer.
2. Make [`BRIEF.md`](../BRIEF.md) the project's instructions if the project has a place for them; otherwise paste
   it whole as the first message. Tell it the site's hostname, for example `https://careers.acme.com/`.
3. Give it the [`starter/`](../starter/) package to restyle: attach its files if the project takes files, or paste
   `starter/layout.html` and `starter/static/site.css` after the brief.
4. Bring the employer's brand -- colours, logo as SVG, fonts as WOFF2 -- and say the layout must stay HTML and CSS.

## Ask for a site, not a design

Asked without the brief, Claude Design exported a design canvas: one `*.dc.html` document of screens over its own
runtime, which nothing static can serve. Before you export, ask for:

> Static HTML pages, one folder per page, with `package.json`, `layout.html` and `static/` at the root of the
> export, following the brief.

## Export, then convert

Use Export and unzip the result untouched. Run `convert` on it first, because it recognises Claude Design's export
shapes and `check` does not:

```sh
java -jar site-package-tool.jar convert acme-export acme-package --hostname https://careers.acme.com
```

- If it says the export **holds a package** in a folder (round 1 put it in `package/`, beside the tool's own `_ds/`
  files and a `demo/` folder), run `check` on that folder instead:
  `java -jar site-package-tool.jar check acme-export/package --no-browser`.
- If it says the export is **a design canvas**, ask again for static HTML pages as above.
- If it says the export **is already a package**, run `check` on it: `java -jar site-package-tool.jar check acme-export --no-browser`.
- Otherwise it writes `acme-package` and reports every change it made and every line left to you.

Paste every line of the report you got back into Claude Design and export again. The rules do not bend, and nothing
is corrected for you.

When `check` passes without `--no-browser` as well, which runs the accessibility gate in a local Chromium, `pack`
writes the ZIP to upload from the Site package route of the management plane.

## Rounds recorded

| Date | Brief | What came out | Rounds to pass |
|---|---|---|---|
| 2026-09-18 | yes | A package one folder down, every page missing `<title>` | 1 expected; round 2 not run |
| 2026-09-18 | no | A design canvas, no `index.html` | Not run past round 1 |

Neither run used this page, and neither export was kept. The first run with it keeps its exports as test cases and
adds a row here.
