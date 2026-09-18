# Development and releases

[← README](../README.md)

## Local installation

npm packages live under `~/.pi/agent/npm/`; `~/.pi/agent/extensions/` is for directly discovered extensions. Restart Pi or run `/reload` after installation.

```bash
git clone git@github.com:zorbeytorunoglu/pi-ask-plan-build.git ~/src/pi-ask-plan-build
ln -s ~/src/pi-ask-plan-build ~/.pi/agent/extensions/pi-ask-plan-build
```

Do not load multiple npm/Git/local copies simultaneously.

## Testing and release process

The test command requires **Node.js 22.6+**. Run these commands from the repository root:

```bash
npm test
npm pack --dry-run
```

The Node suites cover accumulated context, canonical persistence and inert legacy records, branch allocation, tool-event ordering and guards, awaiting validation, paused execution, Markdown parsing/revision, UI ownership/width, shortcuts/autocomplete/history, transcript rails, questions, handoff ordering/recovery, and bounded reconciliation.

Runtime responsibilities are split between `plan-state.ts`, `plan-context.ts`, `plan-markdown.ts`, `plan-execution.ts`, `composer.ts`, `handoff.ts`, `mode-selection.ts`, and `tool-presentation.ts`; `index.ts` wires commands, tools, and events. Smaller question/panel/shortcut/rail modules remain independent.

Releases use [`.github/workflows/publish.yml`](https://github.com/zorbeytorunoglu/pi-ask-plan-build/blob/main/.github/workflows/publish.yml): a `vX.Y.Z` tag must match `package.json` before publishing. `prepublishOnly` runs tests. After publication, the workflow downloads the exact version’s registry tarball and verifies its internal name/version, with six attempts, ten-second intervals, and request timeouts. If availability cannot be confirmed, it reports that publication may already have succeeded; it never automatically unpublishes, bumps again, or republishes. This post-publication path requires a release to exercise end to end.

The publish step authenticates with npm Trusted Publishing (`id-token: write`), so the package’s trusted publisher must be configured on npmjs.com for this repository and `publish.yml`. npm only accepts that configuration for a package that already exists, so the very first version of a new package name is published manually with `npm login` followed by `npm publish --access public`; later releases publish from a tag push.
