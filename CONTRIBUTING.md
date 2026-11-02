# Contributing

Thanks for your interest in contributing! Please follow our [Code of Conduct](./CODE_OF_CONDUCT.md).
Report security issues as described in [SECURITY.md](./SECURITY.md), not in public issues.

For bugs and feature requests, open a GitHub issue. For other questions, contact us at
[info@deepset.ai](mailto:info@deepset.ai).

## Setup

Use Node.js 24, as CI does (`isolated-vm`, which n8n's packages depend on, doesn't build on Node 26
yet).

```bash
npm install
npm run build
npm run lint
npm run format:check
npm run test
```

Use `npm run format` to apply Prettier.

Optionally enable the pre-commit hook (`.githooks/pre-commit`), which checks the formatting of the
files you commit and runs `npm run lint`:

```bash
git config core.hooksPath .githooks
```

If it fails, run `npm run format` or `npm run lint:fix` and stage the changes. You can skip it with
`git commit --no-verify`; CI runs the same checks. (It isn't set up by an npm `prepare` script,
because n8n forbids lifecycle scripts in community node packages.)

## Local development

Run the node in a local n8n instance with hot reload:

```bash
npm run dev    # builds the node, loads it into n8n and starts n8n at http://localhost:5678
```

If another n8n instance is already running on your machine, give the dev instance its own ports:

```bash
N8N_PORT=5679 N8N_RUNNERS_BROKER_PORT=5681 npm run dev
```

This package isn't n8n Cloud eligible (self-hosted only), so one non-cloud lint pass covers the
source and the tests.

## Pull requests

1. Branch from `main`. External contributors: fork the repo first. deepset maintainers: clone the
   repo directly.
2. Keep changes focused, add or update tests, and update `CHANGELOG.md` for user-visible changes.
3. Make sure lint, format check, build and tests pass. CI runs them on every PR.

By contributing, you agree that your contributions are licensed under the [MIT License](./LICENSE).

## CI

`.github/workflows/ci.yml` runs on every pull request and on every push to `main`:

- **lint-and-build**: lint, Prettier format check and build.
- **test**: unit tests (vitest).
- **latest-n8n**: builds and tests against the latest `n8n-workflow` and `n8n-core`.

## Releasing

Maintainers only. Pushing a `v<version>` tag triggers
[`.github/workflows/publish.yml`](./.github/workflows/publish.yml), which publishes the package to
npm with a provenance statement.

1. **Prepare the release in a pull request.** Set the new `version` in `package.json` and move the
   `CHANGELOG.md` entries from `Unreleased` to a heading with that version. Merge it into `main`
   after CI passes.
2. **Tag and push.** The tag version must match `package.json`: the workflow publishes the version
   in `package.json`, not the one in the tag name.

   ```bash
   git checkout main && git pull
   git tag -a v0.2.1 -m "@deepset-ai/n8n-nodes-haystack-enterprise-mcp 0.2.1"
   git push origin v0.2.1
   ```

3. **Verify.** Follow the **Publish** run in the Actions tab, then check on npmjs.com that the new
   version shows the **Provenance** badge.

### If a release fails

- **Failed before publishing:** fix it in a pull request, delete the tag
  (`git push --delete origin <tag> && git tag -d <tag>`) and tag the fixed commit.
- **Already published to npm:** a version can never be published twice. Bump the version and release
  again, and deprecate the bad one with `npm deprecate <package>@<version> "<reason>"`.
