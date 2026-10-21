# Security policy

Please do not report security problems in a public issue. Send a
[private vulnerability report](https://github.com/mrdarkside/directus-live-preview-bridge/security/advisories/new)
through GitHub.

Include the affected version, a minimal reproduction, and the impact you
observed. Remove tokens, project URLs, and content from the report unless they
are required to reproduce the problem.

Only the latest released version receives security fixes.

## Development dependency audit

`npm audit` reports Vite vulnerabilities through this development-only
dependency path:

```text
@directus/extensions-sdk
└─ vite
```

The SDK currently pins Vite to `8.1.2`. The advisories concern the development
server's editor-launch functionality, including Windows command injection
([GHSA-587h-7wmj-h5pm](https://github.com/advisories/GHSA-587h-7wmj-h5pm),
[GHSA-698v-4r3x-3vcv](https://github.com/advisories/GHSA-698v-4r3x-3vcv), and
[GHSA-r825-w8mm-5pxw](https://github.com/advisories/GHSA-r825-w8mm-5pxw)). This
project's scripts do not start a Vite development server or use its
editor-launch functionality. The Directus SDK is a development dependency used
to type-check and build the extension. npm does not install a package's
development dependencies for its consumers, and the compiled `dist/` files do
not contain Vite.

Use these commands to check the distinction:

```sh
npm audit
npm audit --omit=dev
npm pack --dry-run --ignore-scripts
```

The complete development tree currently reports these upstream advisories, while
the production audit reports no vulnerabilities. npm's automated fix proposes
downgrading `@directus/extensions-sdk` from version 18 to version 17. We do not
apply a major SDK downgrade as an automated audit fix. Dependabot tracks the
SDK, so the dependency path can be updated when Directus publishes a compatible
fix.

Reassess this conclusion if Vite ever moves into `dependencies`, appears in the
built package, or is used to serve a development environment.
