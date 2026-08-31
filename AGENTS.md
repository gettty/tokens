# Agents

## Cursor Cloud specific instructions

This is a **data/content repository** (community-curated ERC-20 token registry for [Oku](https://oku.trade)), not a running web application. There are no servers to start or databases to connect to.

### Key commands

| Task | Command |
|---|---|
| Install deps | `corepack enable && yarn install` |
| Lint/format check | `yarn biome check` (or `make lint`) |
| Auto-fix formatting | `yarn format` |
| Build TypeScript | `yarn build` |
| Validate all tokens | `yarn validate` (builds first, then validates all `chains/evm/**/info.json` + `logo.png`) |
| Fix address checksums | `yarn checksum` |
| Generate token list | `yarn list` (requires `CF_KEY` and `CF_SECRET` env vars for R2 upload — not needed for local dev) |

### Non-obvious notes

- **Yarn 4 (Berry) via corepack**: The project uses `yarn@4.6.0` specified in `package.json`'s `packageManager` field. Always run `corepack enable` before `yarn install` so the correct Yarn version is activated.
- **No test suite**: There are no unit/integration tests. Validation of token data is done via `yarn validate`. Use this as the primary correctness check.
- **Pre-commit hook**: `.husky/pre-commit` runs `yarn install`, `yarn format`, and auto-commits formatting changes. This runs on every commit.
- **`yarn list` requires cloud credentials**: The `list` script uploads logos to Cloudflare R2 and needs `CF_KEY`/`CF_SECRET` environment variables. This is not needed for typical development or PR validation workflows.
- **Token data structure**: All token data lives under `chains/evm/<chainId>/<checksummedAddress>/` with `info.json` and `logo.png` files. Addresses must be checksummed (EIP-55).
