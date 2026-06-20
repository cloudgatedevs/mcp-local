# Cloudgate Builder (Local)

A **local development** build of the Cloudgate Builder plugin. It connects Claude to a
Cloudgate server running on your own machine instead of production, so you can build and test
workflow-APIs against local code.

- **API / MCP server:** `http://localhost:44301`  (MCP endpoint: `/mcp/workflow`)
- **Client (React) app:** `http://localhost:5173`  (where OAuth sign-in happens)
- **Transport:** `mcp-remote` (local bridge) — completes OAuth via a loopback listener.

## Prerequisites

- **Node.js** installed (the bridge runs via `npx mcp-remote`).
- Your Cloudgate **dev server running** on `http://localhost:44301`.
- Your Cloudgate **React client running** on `http://localhost:5173`.

## Local server configuration (one-time)

For the local OAuth flow to complete, your dev server (`appsettings.json` /
`appsettings.Development.json`) needs:

1. **OpenIddict enabled**, and the `cloudgate-mcp` client seeded with redirect URIs that include
   the mcp-remote loopback callback:
   - `http://127.0.0.1:33418/callback`
   - `http://localhost:33418/callback`
2. **`App:ClientRootAddress`** pointing at the local client so the login redirect works:
   - `http://localhost:5173/`
3. Transport security relaxed for local HTTP (already handled in non-Production via
   `DisableTransportSecurityRequirement`).

> If your dev server runs over **HTTPS** instead of HTTP, change the URL in `.mcp.json` to
> `https://localhost:44301/mcp/workflow`. With a self-signed dev cert you may also need to start
> Claude with `NODE_TLS_REJECT_UNAUTHORIZED=0` so `mcp-remote` accepts the certificate.

## Install

1. Add this marketplace in Claude (Customize → Plugins → + → Add marketplace → from a
   repository), then install **cloudgate-builder-local**.
2. Make sure the local server (`:44301`) and client (`:5173`) are running.
3. Ask Claude to list your Cloudgate projects → a browser opens to
   `http://localhost:5173` to sign in → approve → the local tools become available.

## Notes

- This points at `localhost`, so it only works on the machine running your dev server — it is
  **not** for distribution. Use the production plugin / store build for real users.
- The connection is named `cloudgate-local` to avoid clashing with a production Cloudgate
  connector if both are installed. (Both default to loopback port `33418`, so run only one at a
  time, or change the port here and in the seeded redirect URIs.)

© Cloudgate Dev LLC
