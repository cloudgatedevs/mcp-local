# Cloudgate Local Development Plugin

Connects a desktop MCP client directly to `http://localhost:44301/mcp/workflow` using
native streamable HTTP and OAuth. Includes the `cloudgate-build` skill.

Install `cloudgate-builder-local` from this marketplace and start a new conversation.
See `plugins/cloudgate-builder-local/README.md` for the local OAuth prerequisites.

The plugin does not use Node.js, `mcp-remote`, or a shared fixed callback port.
Localhost must be reachable from the machine running the MCP client; a cloud-hosted
client cannot reach your computer's loopback address.
