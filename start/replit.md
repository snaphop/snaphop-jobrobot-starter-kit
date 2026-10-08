# Start in Replit

How to build an employer's career site in Replit and upload it as a Job Robot site package. The rules the package is
checked against are all in [`BRIEF.md`](../BRIEF.md); this page says only how to get them into Replit and how its
agent checks its own work.

## Before you build

1. Create a static HTML project for the employer -- not a framework template; the package is the files themselves.
2. Copy the [`starter/`](../starter/) package into a folder named `package/` and `site-package-tool.jar` into the
   project's root.
3. Save [`BRIEF.md`](../BRIEF.md), followed by the block below, as the project's agent instructions file (`replit.md`
   at the project's root), so the agent reads both before it changes anything.
4. Tell the agent the site's hostname, for example `https://careers.acme.com/`, and give it the employer's brand.

## The agent checks its own work

Replit runs code, so its agent runs the same checker the upload runs and fixes what it reports, rather than you
pasting the report back. Add this to the instructions file after the brief:

````markdown
## Checking the package

The site package is the `package/` folder. After every change to it, run:

```sh
java -jar site-package-tool.jar check package --no-browser --json
```

Fix every problem it reports, by file and line, and run it again. Do not stop until it exits 0. Do not change the
rules to fit the design, and do not move the package out of `package/`.
````

The project needs a Java runtime for this; install one with Replit's package tools if `java` is missing. The
accessibility half of `check` needs Chromium and is left to the machine you pack on.

## Upload

On your own machine, with the folder downloaded:

```sh
java -jar site-package-tool.jar check package     # validation, then the accessibility gate
java -jar site-package-tool.jar pack package      # package.zip and the digest the plane will show
```

Upload the ZIP from the Site package route of the management plane.

## Rounds recorded

| Date | Brief | What came out | Rounds to pass |
|---|---|---|---|

Replit has not been run yet. The first run keeps its export as a test case, adds a row here, and says whether the
agent ran the checker on its own.
