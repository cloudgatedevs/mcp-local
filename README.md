# Cloudgate — Local Dev Plugin

Local development marketplace for the **Cloudgate Builder (Local)** plugin. It connects Claude
to a Cloudgate server running on `http://localhost:44301` (client on `http://localhost:5173`)
via the `mcp-remote` bridge, for building and testing workflow-APIs against local code.

```
.claude-plugin/marketplace.json                 # marketplace manifest (root)
plugins/cloudgate-builder-local/
  .claude-plugin/plugin.json                    # plugin manifest
  .mcp.json                                     # mcp-remote -> http://localhost:44301/mcp/workflow
  README.md                                     # local setup instructions
  skills/cloudgate-build/SKILL.md               # the cloudgate-build skill
```

See the plugin README for the local server/client configuration required for OAuth.

© Cloudgate Dev LLC
