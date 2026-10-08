# Start in any other design tool

How to build an employer's career site in a design tool no other page here covers -- Lovable, v0, or the next one
-- and upload it as a Job Robot site package. The rules the package is
checked against are all in [`BRIEF.md`](../BRIEF.md). Nothing about a tool is special-cased on upload, so what
works in one works in all of them.

## Before you design

1. Put [`BRIEF.md`](../BRIEF.md) where the tool reads instructions before it builds: its project instructions or
   knowledge if it has them, otherwise the first message.
2. Give it the [`starter/`](../starter/) package to restyle, as files if it takes them, or by pasting
   `starter/layout.html` and `starter/static/site.css`.
3. Tell it the site's hostname and give it the employer's brand.
4. Ask for **a static export**: plain HTML pages, one folder per page, no framework build step.

## If the tool can run commands

Copy `site-package-tool.jar` into the project and tell the tool to run, after each change,

```sh
java -jar site-package-tool.jar check package --no-browser --json
```

and fix every line until it exits 0, as the [Replit page](replit.md) does.

## If it cannot

Export, unzip, and run `check` on the export:

```sh
java -jar site-package-tool.jar check my-export --no-browser
```

If the export is a static site that is not a package yet -- a bundler's `assets/`, a CDN font, an inline script in
the layout -- `convert` does the mechanical part and reports every change it made and every line it left to you:

```sh
java -jar site-package-tool.jar convert my-export my-package --hostname https://careers.acme.com
```

Paste every line of either report back into the tool and export again. No tool has needed more than two rounds so
far, and three is the most we allow for; if it takes more,
the brief is what to look at, so tell us what the tool kept getting wrong.

When `check` passes without `--no-browser` as well, `pack` writes the ZIP to upload from the Site package route of
the management plane.
