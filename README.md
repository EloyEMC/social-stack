# social-stack

Private, self-hosted social-media automation for two household users.

## Planned stack

- **Postiz** — social account connections, content calendar, scheduling, publishing, and analytics.
- **n8n Community Edition** — self-hosted workflows and integrations.
- **Telegram Bot** — notifications, approvals, and controlled remote commands.
- **AI provider API** — drafting, platform-specific adaptation, and analysis.
- **Herdr** — development and VPS operations; not required at runtime.

## Supported initial account types

- X profiles, subject to X API access.
- LinkedIn personal profiles and company Pages.
- Facebook Pages.
- Instagram professional Business or Creator accounts.

Personal Instagram accounts and ordinary Facebook profiles are outside the initial official-API scope.

## Documentation

See [`docs/project-definition.md`](docs/project-definition.md) for the goals, architecture, MVP workflow, security requirements, costs, licensing notes, and acceptance criteria.

## Status

Planning and infrastructure validation. No production credentials belong in this repository.

## Security

Never commit `.env` files, OAuth tokens, API keys, Telegram bot tokens, VPS configuration, or private media. Use official OAuth flows and restrict Telegram commands to an allowlist of user IDs.
