# n8n-nodes-haystack-enterprise-mcp

n8n community node package (`@deepset-ai/n8n-nodes-haystack-enterprise-mcp`) that exposes a Haystack
Enterprise Platform workspace's MCP server as tools for an n8n AI Agent. Self-hosted n8n only, so it
is not n8n Cloud eligible and not n8n-verified. The package lives at the repo root; the sibling
verified package is https://github.com/deepset-ai/n8n-nodes-haystack-enterprise.

## Conventions

- Nodes and credentials are registered in `package.json` under `n8n.nodes` and `n8n.credentials`
  (paths to compiled files in `dist/`). Update them when adding, removing or renaming either.
- Run `npm run build`, `npm run lint`, `npm test` and `npm run format:check` before pushing. Fix
  lint and type errors rather than disabling them.
- Mark sensitive credential fields as `password`; never log secrets.
- The credentials are kept in sync by hand with the ones in `n8n-nodes-haystack-enterprise`.
- Update `CHANGELOG.md` when bumping the version.
- Releases are published by pushing a `v<version>` tag (see `.github/workflows/publish.yml`).

## n8n docs

- https://docs.n8n.io/integrations/creating-nodes/overview/
- https://docs.n8n.io/integrations/creating-nodes/build/reference/
- https://docs.n8n.io/integrations/creating-nodes/build/reference/ux-guidelines/
