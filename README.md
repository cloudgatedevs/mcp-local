# Cloudgate — Local Dev Plugin

Local development marketplace for the **Cloudgate Builder (Local)** plugin. It connects Claude
to a Cloudgate dev server exposed over an ngrok tunnel
(`https://virescent-unmaliciously-ruthie.ngrok-free.dev`) via the `mcp-remote` bridge, for
building and testing workflow-APIs against local code.

```
.claude-plugin/marketplace.json                 # marketplace manifest (root)
plugins/cloudgate-builder-local/
  .claude-plugin/plugin.json                    # plugin manifest
  .mcp.json                                     # mcp-remote -> https://virescent-unmaliciously-ruthie.ngrok-free.dev/mcp/workflow
  README.md                                     # local setup instructions
  skills/cloudgate-build/SKILL.md               # the cloudgate-build skill
```

See the plugin README for the local server/client configuration required for OAuth.

© Cloudgate Dev LLC
