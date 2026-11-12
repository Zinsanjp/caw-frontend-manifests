# CAW frontend manifests (unofficial)

SHA-256 manifests of CAW frontend builds, for checking which files a CAW node serves.

This is **not an official reference**. It is not published by the CAW maintainers.

## What is here

- `manifests/<version>.json`: one manifest per build.
- `manifests/latest.json`: a copy of the newest one.

Each file has the same format as the `build-manifest.json` that the CAW frontend build writes:

```json
{
  "version": 1,
  "clientVersion": "<version>",
  "builtAt": "<time of the build>",
  "files": { "<served path>": "sha256-<hex of the file's bytes>" }
}
```

`builtAt` is not part of the check. Compare `files`.

## How they are made

On a GitHub-hosted Ubuntu runner: Node 22.23.4, `yarn install --frozen-lockfile`, `CAW_RUNTIME_CONFIG=1`, `CAW_CLIENT_VERSION=<version>`, then `vite build`.

The source is the upstream `GilgameshCaw/Caw` master at `97cd13f3`, plus a small set of local changes that move the frontend settings from build time to runtime. Those changes are not published here.

The same inputs gave the same manifest hash on a VPS and on the runner, with Node 22.12.0, 22.23.4 and 24.22.2, with and without dependency install scripts.

## Limits

- A node installed the standard way builds with its own `VITE_*` settings, so its files differ from these manifests. These manifests describe a runtime-config build only.
- `sw.js` and `workbox-*.js` are not listed (the build leaves them out).
- Nobody can rebuild these files exactly from this repository, because the local changes are not public.
- A check that runs inside a page served by the node being checked can be fooled by that node. Check from somewhere else.
