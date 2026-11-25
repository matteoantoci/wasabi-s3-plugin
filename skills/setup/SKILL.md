---
name: setup
description: Wasabi MCP connection and auth. Use when a wasabi_s3_* or wasabi_iam_*/wasabi_sts_* tool is missing, returns an auth or credential error, or the user asks how to connect Wasabi.
---

# Wasabi MCP setup

The plugin connects two hosted MCP servers. Each server has its own OAuth login:

| Server | URL | Tools |
| --- | --- | --- |
| `wasabi-s3` | `https://mcp.wasabisys.dev/s3` | `wasabi_s3_*` (buckets, objects) |
| `wasabi-iam` | `https://mcp.wasabisys.dev/iam` | `wasabi_iam_*`, `wasabi_sts_*` (users, policies, keys, temporary credentials) |

Two auth layers exist. Find the failed layer, then give the user its fix.

## Layer 1: OAuth between Claude Code and the server

Symptom: the tools are missing, or a tool returns 401, an expired token, or an OAuth error.

1. Tell the user to run `/mcp`, select the server, and authenticate. A browser window opens for the login.
2. If the server still fails, tell the user to clear its authentication in `/mcp` and authenticate again.

The OAuth callback is a loopback address. A firewall that blocks `localhost` callbacks stops the login.

## Layer 2: Wasabi credentials on the server

Symptom: OAuth works, but a tool returns a credential, access-denied, or `reauth_required` error.

The user saves the Wasabi access key and secret key in the dashboard at `https://mcp.wasabisys.dev`. Region, endpoint, and an STS session token are optional there. The S3 and IAM servers use the same key. Claude Code never receives the key, so the user never pastes it into chat or into `.mcp.json`.

An access-denied error with a saved key comes from the IAM policy of that key, not from the MCP server.

## Limits

- The service is in beta. Report server faults to support@wasabi.com.
- Only `/s3`, `/iam`, and `/wacm` exist. A `/mcp` path fails.
- WACM (sub-account management) is not in this plugin. It needs a separate WACM username and API key. To add it: `claude mcp add --transport http wasabi-wacm https://mcp.wasabisys.dev/wacm`.
