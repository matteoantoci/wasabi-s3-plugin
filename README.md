# Wasabi plugin for Claude Code

This plugin connects Claude Code to the hosted [Wasabi MCP server](https://docs.wasabi.com/docs/hosted-wasabi-mcp-with-ai-systems). The server is in beta.

| Server | URL | Tools |
| --- | --- | --- |
| `wasabi-s3` | `https://mcp.wasabisys.dev/s3` | `wasabi_s3_*` |
| `wasabi-iam` | `https://mcp.wasabisys.dev/iam` | `wasabi_iam_*`, `wasabi_sts_*` |

The plugin also includes the `/wasabi:setup` skill for OAuth and credential problems.

## Install

```
/plugin marketplace add <path-or-git-url-of-this-repo>
/plugin install wasabi@wasabi-plugins
```

For a local test: `claude --plugin-dir .`

## Connect

1. Run `/mcp`, select `wasabi-s3`, and authenticate. Your browser opens the Wasabi login.
2. On the first login, save your Wasabi access key and secret key in the dashboard at <https://mcp.wasabisys.dev>.
3. Do step 1 again for `wasabi-iam`. It uses the same saved key.

Claude Code gets only a short-lived OAuth token. Your Wasabi keys stay encrypted on the server.

## WACM

WACM is not included, because it needs a separate WACM username and API key. To add it:

```
claude mcp add --transport http wasabi-wacm https://mcp.wasabisys.dev/wacm
```
