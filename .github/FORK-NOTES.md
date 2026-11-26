# Fork notes (awoaixxx/nodewarden)

Upstream: shuaiplus/nodewarden. This fork is a deployment snapshot, not a code fork.

## Automation

- `.github/workflows/sync-upstream.yml` — daily 02:00 +08, merges upstream/main into fork main.
  On merge conflict the run fails and pushes nothing; resolve manually.
- Cloudflare Workers Builds is connected to this repo (branch `main`):
  push to main -> `npm run build` -> `npx wrangler deploy -c wrangler.kv.toml`.
  Previews are off.
- `wrangler.kv.toml` pins the real D1 database id and KV namespace id for the
  deployed Worker. Do NOT deploy with plain `wrangler.toml` (it has no D1 id
  and binds attachments to R2 instead of KV).
- `wrangler deploy` never touches the `JWT_SECRET` secret or existing bindings;
  secrets persist across deploys.

## How to stop

- Pause sync: disable the "Sync upstream" workflow (Actions tab).
- Pause auto-deploy: disconnect Git in the Worker's Settings > Builds, or
  delete the build configuration via the Cloudflare API.
