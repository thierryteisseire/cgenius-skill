# Security Policy

## Trusted Domains

This skill only communicates with the following trusted endpoints:

- `https://beta.cgenius.app` — Content Genius production API
- `https://app.cgenius.app` — Content Genius production app

No other domains are contacted. The `CGENIUS_API_BASE_URL` environment variable is restricted to these trusted hosts.

## Authentication

- All API tokens are read from environment variables, never hardcoded
- Tokens are sent via `Authorization: Bearer` headers where supported
- No credentials are logged, cached, or stored on disk

## Data Handling

- User content is sent only to the configured Content Genius API
- No data is sent to third-party analytics, telemetry, or tracking services
- No data is written to the filesystem
- No subprocess execution or shell commands

## Secrets Management

- `CGENIUS_EPSIMO_TOKEN` — EpsimoAI authentication token
- `CGENIUS_ASSISTANT_ID` — AI assistant identifier
- `CGENIUS_APPSYNC_URL` — GraphQL endpoint (optional)
- `CGENIUS_APPSYNC_API_KEY` — GraphQL API key (optional)

All secrets must be provided via environment variables. The skill never prompts for or stores secrets.

## Reporting Vulnerabilities

Report security issues to: thierry@epsimoai.com

## Version

Security policy version: 2.0.1
