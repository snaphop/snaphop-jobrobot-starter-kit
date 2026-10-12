# Start in Lovable

How to build an employer's career site in Lovable and hand it over as a Job Robot site package. The rules the package is
checked against are all in [`BRIEF.md`](../BRIEF.md); this page says only how to get them into Lovable and what its
export needs afterwards. An agent can follow it through Lovable's MCP server as well as a person in Lovable's editor.

## Before you build

1. Put [`BRIEF.md`](../BRIEF.md), whole and unchanged, in the workspace's or the project's knowledge before the
   project's first message, so Lovable's agent reads it before it builds anything.
2. In the first message, paste [`starter/layout.html`](../starter/layout.html) and
   [`starter/static/site.css`](../starter/static/site.css) to restyle.
3. Give it the site's hostname, for example `https://careers.acme.com/`, and the employer's brand -- colours, logo as
   SVG, fonts as WOFF2.
4. Ask for **a static export in a folder of its own** at the project's root, such as `acme-careers/`: plain HTML
   pages, one folder per page, with `package.json`, `layout.html` and `static/`, following the brief.

Lovable always builds its own preview app, a TypeScript project, and keeps it beside that folder. The package is
the folder alone; the preview app is not part of it.

## Export and check

Lovable cannot run the checker, so you run it on the export. Take the package folder out of the project untouched:
from GitHub if the project is synced there, or with the MCP server's `read_file`. That returns text only, so fonts
and images read through it come back altered; take those from the published site instead, and check each against
the server's read of the same file. The MCP server cannot unpublish a project, so unpublish it in Lovable's editor
afterwards if it should not stay public.

```sh
java -jar site-package-tool.jar check acme-careers --no-browser
```

If the export is a static site that is not a package yet, `convert` does the mechanical part and reports every
change it made and every line it left to the tool:

```sh
java -jar site-package-tool.jar convert acme-careers acme-package --hostname https://careers.acme.com
```

Give Lovable every line of either report, verbatim, and export again. The rules do not bend, and nothing is
corrected for it. Stop after three rounds that do not pass, and keep every export.

When `check` passes without `--no-browser` as well, which runs the accessibility gate in a local Chromium, `pack`
writes the ZIP. Upload it from the Site package route of the management plane, or through the management API with a
key that may manage site packages; the upload is checked again, and the package waits for a person who may change
the site's branding to activate it.

## Rounds recorded

| Date | Brief | What came out | Rounds to pass |
|---|---|---|---|
| 2026-10-11 | yes | A package in `acme-careers/`, beside Lovable's own preview app | 1 |
| 2026-10-12 | yes | A package in `acme-careers/`, beside Lovable's own preview app; one line refused, a checker fault since fixed | 2 |

The 2026-10-11 run was made before this page existed, from [`other-tools.md`](other-tools.md) alone, and driven by an
agent through Lovable's MCP server: the brief as the workspace's knowledge, the starter's layout and stylesheet pasted
into the first message, the hostname and the brand. Lovable's agent wrote itself a checker from the brief's rules,
and nothing had to be pasted back: its first export passed `check` and the accessibility gate.

The 2026-10-12 run was the first made from this page, driven the same way, with the same brand and hostname. Lovable's
agent again checked its own package, at 320 and 1280 pixels, and its first export was refused for one line only:
`[aria-invalid=true]` in the stylesheet. That selector is valid CSS, and the checker refused it by its own fault,
since fixed: its CSS parser read `true` and `false` as keywords. Given the line verbatim, Lovable's agent quoted every
attribute value and changed nothing else, and the second export passed `check` and the accessibility gate. Each run
adds a row here, and its export is kept as a test case.
