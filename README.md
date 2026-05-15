# Content Genius Skill v2.0

[![npm version](https://img.shields.io/npm/v/@thierryteisseire/cgenius-skill.svg)](https://www.npmjs.com/package/@thierryteisseire/cgenius-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Complete AI-powered content platform skill for Claude Code / Kiro. Generate blogs, social posts, emails, proposals, presentations, videos, and more — all from your terminal.

## Features

| Feature | Command | Description |
|---------|---------|-------------|
| **Blog** | `/cgenius blog generate` | V2 async pipeline: research → outline → write |
| **Blog Management** | `/cgenius blog list\|status\|resume\|regenerate` | Full lifecycle management |
| **Social** | `/cgenius social` | AI social post generation |
| **Email** | `/cgenius email` | Professional email generation |
| **Proposal** | `/cgenius proposal` | Commercial proposal with streaming |
| **Proposal Pipeline** | `/cgenius proposal-pipeline` | 3-stage async pipeline |
| **Content Ideation** | `/cgenius ideate` | Create, analyze, generate from ideas |
| **Brand Voice** | `/cgenius brand-voice` | Create and manage brand voices |
| **Offer Agent** | `/cgenius offer` | Automated offer generation with research |
| **Questionnaire** | `/cgenius questionnaire` | Shareable client intake forms |
| **Publish** | `/cgenius publish` | Schedule and publish content |
| **PPTX** | `/cgenius pptx` | PowerPoint generation |
| **Video** | `/cgenius video` | Video rendering via Remotion |
| **Document** | `/cgenius document` | Reports, briefs, memos |
| **Analytics** | `/cgenius analytics` | SEO and social analytics |

## Installation

```bash
# Via npx (recommended)
npx skills add thierryteisseire/cgenius-skill

# Via npm
npm install -g @thierryteisseire/cgenius-skill
```

## Configuration

Set these environment variables:

```bash
# Required
export CGENIUS_EPSIMO_TOKEN="your_token"
export CGENIUS_API_BASE_URL="https://beta.cgenius.app"
export CGENIUS_ASSISTANT_ID="your_assistant_id"

# Optional (for GraphQL/publish features)
export CGENIUS_APPSYNC_URL="your_appsync_endpoint"
export CGENIUS_APPSYNC_API_KEY="your_api_key"
```

## Quick Start

```bash
# Generate a blog post
/cgenius blog generate "How AI is changing sales" --size medium --tone professional

# Create a social post
/cgenius social "Announce our new product launch"

# Generate a proposal
/cgenius proposal "Acme Corp" --services "SEO, Content" --budget "50k"

# Create content ideation
/cgenius ideate create "SaaS marketing trends 2026" --priority high

# Schedule content
/cgenius publish schedule content-123 --platform blog --date 2026-06-01
```

## What's New in v2.0

- **Blog V2 Pipeline** — async task-based generation with research, outline, and writing phases
- **Content Ideation** — create, analyze, and generate content from ideas
- **Brand Voice** — define and reuse brand voice profiles
- **Offer Agent** — automated offer generation with market research
- **Publish/Schedule** — content calendar and multi-platform publishing
- **Video** — Remotion-based video rendering
- **Fixed Streaming** — proper EpsimoAI SSE cumulative content handling
- **GraphQL Support** — direct DynamoDB operations for CRUD

## Documentation

- [SKILL.md](./SKILL.md) — Full command reference and implementation logic
- [examples/](./examples/) — Usage examples for each feature

## License

MIT
