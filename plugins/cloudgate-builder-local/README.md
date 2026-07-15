# Cloudgate Builder (Local)

A **local development** build of the Cloudgate Builder plugin. It connects Claude to a
Cloudgate server running on your own machine (exposed via an ngrok tunnel) instead of
production, so you can build and test workflow-APIs against local code.

- **API / MCP server:** `https://virescent-unmaliciously-ruthie.ngrok-free.dev`  (MCP endpoint: `/mcp/workflow`)
- **Local origin behind the tunnel:** `http://localhost:44301`
- **Client (React) app:** `http://localhost:5173`  (where OAuth sign-in happens)
- **Transport:** `mcp-remote` (local bridge) — completes OAuth via a loopback listener.

## Prerequisites

- **Node.js** installed (the bridge runs via `npx mcp-remote`).
- Your Cloudgate **dev server running** on `http://localhost:44301`.
- Your Cloudgate **React client running** on `http://localhost:5173`.
- An **ngrok tunnel** forwarding `https://virescent-unmaliciously-ruthie.ngrok-free.dev`
  to the local dev server (`http://localhost:44301`).

## Local server configuration (one-time)

For the local OAuth flow to complete, your dev server (`appsettings.json` /
`appsettings.Development.json`) needs:

1. **OpenIddict enabled**, and the `cloudgate-mcp` client seeded with redirect URIs that include
   the mcp-remote loopback callback (**path must be `/oauth/callback`**, not `/callback`):
   - `http://127.0.0.1:33418/oauth/callback`
   - `http://localhost:33418/oauth/callback`
2. **`App:ClientRootAddress`** pointing at the local client so the login redirect works:
   - `http://localhost:5173/`
3. Transport security relaxed for local HTTP (already handled in non-Production via
   `DisableTransportSecurityRequirement`).
4. Scope **`mcp`** allowed for that client (Protected Resource Metadata advertises
   `scopes_supported: ["mcp"]`, and `mcp-remote` requests it).

> The ngrok tunnel terminates TLS with a valid certificate, so `mcp-remote` connects over
> plain HTTPS with no extra flags. If you point `.mcp.json` at a direct `https://localhost`
> dev server with a self-signed cert instead, start Claude with `NODE_TLS_REJECT_UNAUTHORIZED=0`
> so `mcp-remote` accepts the certificate.

## Install

1. Add this marketplace in Claude (Customize → Plugins → + → Add marketplace → from a
   repository), then install **cloudgate-builder-local**.
2. Make sure the local server (`:44301`) and client (`:5173`) are running.
3. Ask Claude to list your Cloudgate projects → a browser opens to
   `http://localhost:5173` to sign in → approve → the local tools become available.

## Re-auth / troubleshooting

`mcp-remote` stores tokens under `%USERPROFILE%\.mcp-auth`. To force a fresh OAuth login:

```powershell
# 1) Clear cached tokens / lockfiles
Remove-Item -Recurse -Force "$env:USERPROFILE\.mcp-auth" -ErrorAction SilentlyContinue

# 2) Manual reauth probe (PowerShell-safe: put client_id JSON in a file)
Set-Content -Path .\client-info.json -Value '{"client_id":"cloudgate-mcp"}'
npx -y -p mcp-remote@latest mcp-remote-client `
  http://localhost:44301/mcp/workflow `
  33418 `
  --static-oauth-client-info "@$PWD\client-info.json" `
  --debug
```

Do **not** paste `--static-oauth-client-info {"client_id":"..."}` raw in PowerShell — `{...}` is
parsed as a script block and breaks JSON. Use the `@file` form above, or run from cmd.exe.

Expected authorize `redirect_uri`:
`http://localhost:33418/oauth/callback`

If the browser errors with an invalid redirect URI, update the seeded OpenIddict client URIs
to that exact path and restart the API.

## Notes

- This points at `localhost`, so it only works on the machine running your dev server — it is
  **not** for distribution. Use the production plugin / store build for real users.
- The connection is named `cloudgate-local` to avoid clashing with a production Cloudgate
  connector if both are installed. (Both default to loopback port `33418`, so run only one at a
  time, or change the port here and in the seeded redirect URIs.)

© Cloudgate Dev LLC
