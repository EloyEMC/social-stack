# Private Social Media Automation Platform

A self-hosted system for two household users to manage and automate social-media content from a private VPS. The platform will centralize publishing, scheduling, AI-assisted content creation, notifications, and operational control through Telegram.

## Decision summary

- **Postiz**: social-media dashboard, account connections, content calendar, scheduling, publishing, and analytics.
- **n8n Community Edition**: self-hosted workflow automation on the same VPS; free for internal use.
- **Telegram Bot**: notifications and a controlled command interface.
- **AI provider API**: content generation, platform-specific adaptation, and analysis.
- **Herdr**: development, deployment, maintenance, and agent coordination; not part of the production runtime.

## Goals

1. Connect the users' supported social-media accounts through official OAuth flows.
2. Compose one campaign and adapt it for each network.
3. Schedule and publish content reliably.
4. Receive success, failure, and approval notifications in Telegram.
5. Approve, reject, pause, resume, and inspect workflows from Telegram.
6. Keep credentials, content, and automation data on the private VPS.
7. Require confirmation before potentially destructive or public actions.

## Initial supported accounts

| Network | Supported account type | Initial scope |
|---|---|---|
| X | Personal or business profile, subject to API access | Posts and scheduling |
| LinkedIn | Personal profile and company Page | Posts and scheduling |
| Facebook | Facebook Page | Posts and scheduling |
| Instagram | Professional Business or Creator account | Posts, images, videos, Reels where supported |

Personal Instagram accounts and ordinary Facebook profiles are not part of the initial publishing scope because the official APIs do not generally support them for this use case.

## MVP workflow

1. User creates a draft in Postiz or through an approved Telegram command.
2. The AI produces platform-specific variants.
3. The user reviews the variants in Postiz or Telegram.
4. The user explicitly approves publication.
5. Postiz schedules or publishes through official platform APIs.
6. n8n sends a Telegram notification with the result.
7. Failures are stored, reported, and optionally retried.

## Telegram commands (proposal)

| Command | Behavior | Confirmation |
|---|---|---|
| `/status` | Show services and next scheduled posts | No |
| `/queue` | Show pending publications | No |
| `/draft` | Create a draft from supplied text | No |
| `/approve <id>` | Approve a draft or scheduled publication | Yes |
| `/pause <network>` | Pause a network workflow | Yes |
| `/resume <network>` | Resume a network workflow | Yes |
| `/retry <id>` | Retry a failed publication | Yes |
| `/metrics` | Return a basic metrics summary | No |

Only Telegram user IDs belonging to the two household users may issue commands.

## Responsibilities by component

### Postiz

- OAuth account connections.
- Social-media provider integrations.
- Media uploads and platform-specific constraints.
- Calendar, drafts, scheduling, and publishing.
- Publication history and supported analytics.

### n8n

- Telegram webhooks and notifications.
- Scheduled or event-driven workflows.
- AI provider calls.
- Conditional logic, retries, and external integrations.
- Calling the Postiz API.

### Telegram Bot

- Remote notifications.
- Approval and operational commands.
- No storage of social-media access tokens or AI secrets.

### AI provider

- Draft generation.
- Rewriting for character limits and tone.
- Hashtag and format suggestions.
- Post-publication summaries.

### Herdr

- Workspaces and terminal panes for development.
- SSH operations against the VPS.
- Running deployment, update, backup, and diagnostic commands.
- Coordinating coding agents.

Herdr is not required for Postiz, n8n, or the Telegram bot to keep running.

## Deployment model

```text
Internet
   |
HTTPS reverse proxy
   |
VPS
   ├── Postiz
   ├── n8n Community Edition
   ├── Telegram bot workflows
   ├── PostgreSQL/Redis or the versions required by the chosen deployment
   ├── encrypted secrets and backups
   └── monitoring and logs
```

The VPS must use HTTPS for OAuth callbacks and webhooks. Services should be isolated with Docker, protected by strong credentials, and exposed only through the required reverse-proxy routes.

## Costs and licensing

- VPS: existing infrastructure cost.
- Telegram Bot API: free for normal bot usage.
- n8n Community Edition self-hosted: free for internal use, subject to its Sustainable Use License.
- Postiz: AGPL-3.0; review obligations before redistributing or offering it as a service.
- AI: usage-based API costs unless using a local model.
- X: API availability and pricing depend on the current X developer plan.
- Meta and LinkedIn: app registration, permissions, and possible review/business-verification requirements.

A ChatGPT Plus or Claude Pro subscription should not be assumed to include API usage. Automated workflows normally require a separate provider API key and billing arrangement.

## Security requirements

- Use official OAuth flows; never store social-media passwords.
- Store secrets in environment variables or a dedicated secret store.
- Use HTTPS and rotate secrets when needed.
- Restrict Telegram commands by user ID.
- Require confirmation for publishing, deleting, pausing, or retrying.
- Keep an audit trail of commands and publication results.
- Back up Postiz, n8n, and workflow configuration data.
- Do not send OAuth tokens, API keys, or private content into Telegram messages.

## Out of scope for the MVP

- Scraping or browser automation.
- Automatically following, unfollowing, liking, or mass-messaging users.
- Publishing to personal Facebook profiles.
- Publishing to personal Instagram accounts.
- Fully autonomous public posting without human approval.
- Selling access to the installation as a hosted automation service.

## Acceptance criteria

- Both users can log in to the private Postiz instance.
- Each user can connect only the accounts they are authorized to manage.
- A single campaign can be adapted and scheduled for all four networks where account permissions allow it.
- Telegram reports success and failure for every publication.
- Authorized Telegram commands can inspect and control the queue.
- An unauthorized Telegram user cannot execute commands.
- The system continues scheduling if Herdr is offline.
- A backup and restore procedure is documented and tested.

## Recommended implementation order

1. Deploy Postiz on the VPS.
2. Connect one test account per network.
3. Publish a manual test post on each network.
4. Deploy n8n Community Edition.
5. Create Telegram notifications.
6. Add approval commands.
7. Add AI drafting and platform adaptation.
8. Add retries, backups, monitoring, and operational documentation.
