# Cloudgate Builder (Local)

Connects directly to `http://localhost:44301/mcp/workflow` using native HTTP and OAuth.
The connection name is `cloudgate-local`, separate from production's `cloudgate`.

## Prerequisites

- A desktop MCP client with native HTTP and OAuth support.
- Cloudgate API running at `http://localhost:44301` and React client at `http://localhost:5173`.
- OpenIddict enabled with MCP scope support and dynamic client registration at
  `/connect/register`, so the client can register its own OAuth callback URI.
- `App:ClientRootAddress` set to `http://localhost:5173/`.
- Local HTTP allowed in the development environment, as configured by
  `DisableTransportSecurityRequirement`. Keep production on HTTPS.

The native client manages its callback port and credentials. The old `cloudgate-mcp`
client and port 33418 are not requirements for this plugin.

## Install and authenticate

Install/update `cloudgate-builder-local` from this marketplace. Start a new conversation
and ask to list Cloudgate projects. Complete the OAuth sign-in in the browser.

If using a direct Codex configuration instead of the plugin:

```toml
[mcp_servers.cloudgate-local]
url = "http://localhost:44301/mcp/workflow"
```

Then run `codex mcp login cloudgate-local`.

## Troubleshooting

Check both `/.well-known/oauth-protected-resource` and
`/.well-known/oauth-authorization-server`. The advertised authorization server and issuer
must both be exactly `http://localhost:44301/`. Rebuild and restart the API after changes.
If registration or sign-in fails, inspect the native client's error and the API logs.
Use the client's connection settings to sign in again; do not clear unrelated credentials.
For HTTPS development, trust the development certificate rather than disabling TLS checks.

After updating from the bridge version, verify the installed `.mcp.json` contains
`type: http` and no `npx` command, then start a new conversation.
