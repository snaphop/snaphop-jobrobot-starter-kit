# SnapHop Job Robot starter kit

Everything an agency needs to design an employer's career site in an AI design tool -- Claude Design, Replit,
Lovable, v0 or any other -- and hand it over as a **site package** for SnapHop Job Robot.

A site package is a folder of plain HTML, CSS, images and fonts: the frame Job Robot renders its job listing, job
pages and apply form into, and any pages of your own. You design it; Job Robot generates the job pages and runs the
application form.

## What is here

| | |
|---|---|
| [`BRIEF.md`](BRIEF.md) | The package rules, written as instructions for a design tool. Put it into the tool before you design. |
| [`starter/`](starter/) | A package that already follows every rule. The fastest route is to restyle a copy of it. |
| [`start/`](start/) | How to start in [Claude Design](start/claude-design.md), [Replit](start/replit.md) or [any other tool](start/other-tools.md). |
| [`examples/`](examples/) | Two finished packages by fictional agencies, `northbeam` and `fieldnote`, to see what a designed site looks like. |
| [Releases](https://github.com/snaphop/snaphop-jobrobot-starter-kit/releases) | `site-package-tool.jar`, which checks a package on your machine exactly as the upload does. |

## The steps

1. **Start** in your tool with the brief and the starter, as the page for that tool in `start/` describes.
2. **Check** what the tool exports with the site package tool, and paste every line of its report back into the
   tool until it passes. A tool that can run commands, such as Replit, can run the check itself.
3. **Pack** the folder into the ZIP to upload, and upload it from the Site package page of the employer's Job Robot
   workspace. The upload runs the same checks; the employer activates the site once its accessibility check passes.

## The site package tool

Download `site-package-tool.jar` from the [latest release](https://github.com/snaphop/snaphop-jobrobot-starter-kit/releases/latest)
and check it against the `.sha256` file beside it. It needs Java 25 or later. The accessibility check and the preview
also need Node.js 26 and Chromium; the tool says how to install them the first time it needs them.

```sh
java -jar site-package-tool.jar check my-package            # the upload's checks, then the accessibility check
java -jar site-package-tool.jar check my-package --no-browser --json   # the same without Chromium, for a tool to read
java -jar site-package-tool.jar convert my-export my-package --hostname https://careers.acme.com
java -jar site-package-tool.jar preview my-package          # the site with sample jobs at http://127.0.0.1:8765/
java -jar site-package-tool.jar pack my-package             # my-package.zip and the digest the upload will show
```

The tool works offline and sends nothing anywhere. Exit status 0 means the package is fine, 1 that it has problems,
2 a usage error or an export `convert` will not guess at, and 3 that the tool could not run.

A new release is cut only when the tool changes. Each release says which checks changed, so a package that passed
an older release may need another look.

## Licence

The brief, the starter, the guides and the examples are under the [MIT licence](LICENSE), so copy and adapt them
freely; the example fonts are under the SIL Open Font License, in [`font-licenses/`](font-licenses/). The jar is free
to run and to pass on unmodified, under [its own terms](CHECKER-LICENSE.md).

## Questions and problems

If the brief keeps leading a tool to the same mistake, or the tool reports something you think is wrong, open an
issue here with the report and the tool you used. Do not attach a site with a real employer's unpublished content,
or anything about candidates.

This repository is published from SnapHop's own source, so pull requests are read but not merged here: what they
change is made at the source and appears in the next update.
