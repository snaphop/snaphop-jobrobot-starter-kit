# Start in Claude Design

How an agent designs an employer's career site in Claude Design and hands it over as a Job Robot site package. No person runs Claude Design
for a site: an agent starts the design, checks every download, gives the tool every line back, and uploads the
package when it passes. The rules the package is checked against are all in [`BRIEF.md`](../BRIEF.md); this page
says only how the agent gets them into Claude Design and what its downloads need afterwards.

## Which Claude Design

Claude Design has moved into Claude, where a design is an artifact of Claude's Design type. The standalone version
at claude.ai/design closes on 14 December 2026
([Anthropic's migration guide](https://support.claude.com/en/articles/17440474-migrate-from-standalone-claude-design-to-claude)).
Start every site in Claude.

## Start the design

1. Start a new design for the employer in Claude's Design type.
2. Give it [`BRIEF.md`](../BRIEF.md) whole as the first message, before anything is designed, with the site's
   hostname, for example `https://careers.acme.com/`.
3. Give it the [`starter/`](../starter/) package to restyle, as files, or as `starter/layout.html` and
   `starter/static/site.css` after the brief.
4. Give it the employer's brand -- colours, logo as SVG, fonts as WOFF2 -- and say the layout must stay HTML and CSS.
5. Ask for the site, not a design of it:

   > Static HTML pages, one folder per page, with `package.json`, `layout.html` and `static/` in a folder of their
   > own, following the brief.

   The Design type keeps the design's own boards (`*.dc.html`) and `canvas.json` in its `project/` folder, and a
   package cannot hold them, so the package needs a folder of its own beside them, such as `project/site/`.

## Download, convert, check

Download every file of the design, untouched, into an empty folder: the design's files are under `project/`. Run
`convert` on the whole download first, because it reads `project/` and recognises what the Design type keeps
there, and `check` does not:

```sh
java -jar site-package-tool.jar convert acme-download acme-package --hostname https://careers.acme.com
```

What it answers, and what to do next:

- **holds a package at `project/site/`** (or any folder): run `check` on that folder,
  `java -jar site-package-tool.jar check acme-download/project/site --no-browser --json`.
- **holds a package at `project/`, beside the design's boards**: give the tool that line. The package is right but
  sits among the boards; it moves into a folder of its own.
- **is a design canvas, and `project/site/` beside it holds static pages**: run `convert` on that folder.
- **is a design canvas, not a site**: give the tool that line, and ask again for the site as in step 5.
- **is already a package**: run `check` on the folder named.
- Otherwise it writes `acme-package` and reports every change it made and every line it leaves to the tool.

Give the tool every line of the report, verbatim, and download again. The rules do not bend, and nothing is
corrected for it. Stop after three rounds that do not pass, and keep every download: each one is what teaches
`convert` a shape it does not know yet.

When `check` passes without `--no-browser` as well, which runs the accessibility gate in a local Chromium, `pack`
writes the ZIP. Upload it from the Site package route of the management plane, or through the management API with a
key that may manage site packages; the upload is checked again. The package then waits for a person who may change
the site's branding to activate it: what goes live on an employer's hostname is still a person's decision.

## Rounds recorded

| Date | Where | Brief | What came out | Rounds to pass |
|---|---|---|---|---|
| 2026-09-18 | Standalone | yes | A package one folder down, every page missing `<title>` | 1 expected; round 2 not run |
| 2026-09-18 | Standalone | no | A design canvas, no `index.html` | Not run past round 1 |
| 2026-10-11 | Claude | yes | A package beside the design's boards in `project/`, refused for the boards | 3: the package moved to `project/site/` |
| 2026-10-11 | Claude | no | A design canvas; then a static site written by hand beside it | 3: links made root-relative, web fonts dropped |

The 2026-09-18 runs were a person's, in the standalone version; they did not use this page, and their exports were
not kept. The 2026-10-11 runs were driven by agents: one built each design in Claude's Design type, from the brief
and the starter or from the request alone, and another ran the tool on each download and gave every report back
verbatim. Both passed `check` and the accessibility gate in their third round, and every round's export is kept as
a test case. `convert` now answers each of those downloads from its root with the step that run needed, so a run
with this page is expected to take fewer rounds; each run adds a row here.
