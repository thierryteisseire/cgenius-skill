# Content Genius Skill v2.0

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

AI-powered content creation and proposal generation system. Generate blogs, social posts, emails, proposals, presentations, videos, and more — all from your AI coding agent.

## Installation

```bash
npx skills add thierryteisseire/cgenius-skill
```

## Configuration

```bash
export CGENIUS_API_BASE_URL="https://beta.cgenius.app"
export CGENIUS_EPSIMO_TOKEN="your_token"
export CGENIUS_ASSISTANT_ID="your_assistant_id"

# Optional (for GraphQL/publish features)
export CGENIUS_APPSYNC_URL="your_appsync_endpoint"
export CGENIUS_APPSYNC_API_KEY="your_api_key"
```

Get your token from: https://beta.cgenius.app/settings/api-tokens

## Available Commands

| Command | Description |
|---------|-------------|
| `/cgenius blog generate <topic>` | Async blog pipeline (research → outline → write) |
| `/cgenius blog list\|status\|resume\|regenerate` | Blog lifecycle management |
| `/cgenius social <message>` | Generate social post |
| `/cgenius email <topic>` | Generate professional email |
| `/cgenius proposal <client>` | Generate commercial proposal |
| `/cgenius proposal-pipeline <client>` | Full 3-stage async pipeline |
| `/cgenius ideate create\|list\|analyze\|generate` | Content ideation workflow |
| `/cgenius brand-voice create\|list` | Manage brand voice profiles |
| `/cgenius offer create\|research <client>` | Automated offer generation |
| `/cgenius questionnaire create\|list\|status` | Shareable client intake forms |
| `/cgenius publish schedule\|list` | Content calendar & publishing |
| `/cgenius pptx <topic>` | PowerPoint generation |
| `/cgenius video render\|list` | Video rendering via Remotion |
| `/cgenius document <topic>` | Reports, briefs, memos |
| `/cgenius analytics seo\|social` | Performance analytics |

## Quick Start

```bash
# Generate a blog post
/cgenius blog generate "How AI is changing sales" --size medium --tone professional

# Create a social post
/cgenius social "Announce our new product launch"

# Generate a proposal
/cgenius proposal "Acme Corp" --services "SEO, Content" --budget "50k"

# Create shareable questionnaire
/cgenius questionnaire create "Acme Corp" --email "client@acme.com" --expires 14

# Schedule content
/cgenius publish schedule content-123 --platform blog --date 2026-06-01
```

## Documentation

- [SKILL.md](./SKILL.md) — Full command reference with options and API endpoints
- [examples/](./examples/) — Usage examples

## Links

- **App:** https://beta.cgenius.app
- **GitHub:** https://github.com/thierryteisseire/cgenius-skill
- **skills.sh:** https://skills.sh/thierryteisseire/cgenius-skill

## License

MIT
