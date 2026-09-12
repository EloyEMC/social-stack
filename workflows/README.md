# Planned workflows

This directory will contain exported n8n workflows after the local integration has been validated. Do not add bot tokens, OAuth tokens, API keys, or other credentials to workflow exports.

## Telegram-to-Postiz workflow

The planned workflow will provide a controlled path from Telegram to Postiz:

1. Receive a Telegram message through an n8n webhook or Telegram trigger.
2. Verify the sender against an allowlist of Telegram user IDs.
3. Parse a command such as `/draft <content>` or `/approve <id>`.
4. Require explicit confirmation before any public, destructive, pause, resume, or retry action.
5. Create or update a draft in Postiz through its supported API or integration.
6. Return a concise status message to Telegram without exposing credentials or private tokens.
7. Record success and failure details for later inspection and bounded retries.

The first local test should use a non-production Telegram bot and a draft-only path. Publishing, OAuth connections, AI provider calls, and remote webhooks are intentionally not configured by this scaffold. The exact Postiz API endpoints and n8n node configuration must be confirmed against the selected, pinned Postiz version before an export is committed.
