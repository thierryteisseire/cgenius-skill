---
name: cgenius
description: Use this skill when the user wants to create content (blogs, social posts, emails, proposals, presentations, documents), manage content ideation, configure brand voice, generate offers, schedule/publish content, render videos, or manage questionnaires. Trigger on mentions of content creation, blog writing, social media, proposals, brand voice, content calendar, publishing, video, offers, or marketing automation.
license: MIT
---

# Content Genius Skill v2.0

AI-powered content creation and proposal generation system.

## Installation

```bash
npx skills add thierryteisseire/cgenius-skill
```

## Configuration

Set these environment variables before using:

```bash
export CGENIUS_API_BASE_URL="https://beta.cgenius.app"
export CGENIUS_EPSIMO_TOKEN="your_token"
export CGENIUS_ASSISTANT_ID="your_assistant_id"

# Optional (for GraphQL/publish features)
export CGENIUS_APPSYNC_URL="your_appsync_endpoint"
export CGENIUS_APPSYNC_API_KEY="your_api_key"
```

Get your token from: https://beta.cgenius.app/settings/api-tokens

---

## Available Commands

### 1. Blog Generation

| Command | Description |
|---------|-------------|
| `/cgenius blog generate <topic>` | Create blog via async pipeline (research → outline → write) |
| `/cgenius blog list` | List all blogs |
| `/cgenius blog status <taskId>` | Check task progress |
| `/cgenius blog resume <taskId>` | Resume a paused task |
| `/cgenius blog regenerate <taskId> <sectionIndex>` | Regenerate a specific section |

**Options for `blog generate`:**
- `--size single|small|medium|big|huge`
- `--tone <tone>` (default: professional)
- `--words <number>` (default: 1500, max: 10000)
- `--keywords <comma-separated>`
- `--brand-voice <voice>`
- `--title <title>`

**Example:**
```
/cgenius blog generate "How AI is changing B2B sales" --size medium --tone professional --keywords "AI, sales, automation"
```

---

### 2. Social Media

| Command | Description |
|---------|-------------|
| `/cgenius social <message>` | Generate social post (title + body) |

**Example:**
```
/cgenius social "Announce our new product launch for Q3"
```

---

### 3. Email

| Command | Description |
|---------|-------------|
| `/cgenius email <topic>` | Generate professional email content |

**Example:**
```
/cgenius email "Follow up after demo meeting with Acme Corp"
```

---

### 4. Proposals

| Command | Description |
|---------|-------------|
| `/cgenius proposal <client_name>` | Generate commercial proposal (streaming) |
| `/cgenius proposal-pipeline <client_name>` | Full 3-stage async pipeline |

**Options for `proposal`:**
- `--services <services>`
- `--budget <budget>`
- `--duration <duration>`

**Options for `proposal-pipeline`:**
- `--industry <industry>`
- `--goals <goals>`
- `--budget <budget>`
- `--timeline <timeline>`
- `--services <services>`

**Example:**
```
/cgenius proposal "Acme Corp" --services "SEO, Content Marketing" --budget "50k"
/cgenius proposal-pipeline "Acme Corp" --industry "SaaS" --budget "100k" --timeline "6 months"
```

---

### 5. Content Ideation

| Command | Description |
|---------|-------------|
| `/cgenius ideate create <topic>` | Create a new content idea |
| `/cgenius ideate list` | List ideas |
| `/cgenius ideate analyze <id>` | Analyze an idea |
| `/cgenius ideate generate <id>` | Generate content from idea |

**Options:**
- `--priority high|medium|low`
- `--status new|analyzing|ready|published` (for list)
- `--type blog|social|email` (for generate)

**Example:**
```
/cgenius ideate create "SaaS marketing trends 2026" --priority high
/cgenius ideate generate abc123 --type blog
```

---

### 6. Brand Voice

| Command | Description |
|---------|-------------|
| `/cgenius brand-voice create <name>` | Create brand voice profile |
| `/cgenius brand-voice list` | List all brand voices |

**Options for `create`:**
- `--tone <tone>`
- `--description <description>`

**Example:**
```
/cgenius brand-voice create "Corporate Friendly" --tone "warm, professional" --description "Used for client communications"
```

---

### 7. Offer Agent

| Command | Description |
|---------|-------------|
| `/cgenius offer create <client_name>` | Trigger batch offer agent (async) |
| `/cgenius offer research <client_name>` | Research-based offer generation |

**Options:**
- `--subjects <comma-separated>` (default: Market Analysis, Client Context, Competitor Analysis, Offer Components)
- `--industry <industry>` (for research)

**Example:**
```
/cgenius offer create "TechStartup Inc" --subjects "Market Analysis,Competitor Analysis"
/cgenius offer research "TechStartup Inc" --industry "fintech"
```

---

### 8. Questionnaire

| Command | Description |
|---------|-------------|
| `/cgenius questionnaire create <client_name>` | Create shareable client intake form |
| `/cgenius questionnaire list` | List all questionnaires |
| `/cgenius questionnaire status <token>` | Check completion status |

**Options for `create`:**
- `--email <client_email>`
- `--expires <days>` (default: 30)
- `--notify <email>` (notification on completion)

**Returns:** Shareable URL, short URL, QR code, expiration date.

**Example:**
```
/cgenius questionnaire create "Acme Corp" --email "contact@acme.com" --expires 14 --notify "me@agency.com"
```

---

### 9. Publish / Schedule

| Command | Description |
|---------|-------------|
| `/cgenius publish schedule <contentId>` | Schedule content for publication |
| `/cgenius publish list` | List scheduled publications |

**Options for `schedule`:**
- `--platform blog|social|email|newsletter` (required)
- `--date <YYYY-MM-DD>` (required)

**Options for `list`:**
- `--status draft|scheduled|published|failed`

**Example:**
```
/cgenius publish schedule content-123 --platform blog --date 2026-06-01
/cgenius publish list --status scheduled
```

---

### 10. Presentation (PPTX)

| Command | Description |
|---------|-------------|
| `/cgenius pptx <topic>` | Generate PowerPoint presentation |

**Options:**
- `--slides <number>` (default: 10)

**Example:**
```
/cgenius pptx "Q3 Marketing Strategy" --slides 12
```

---

### 11. Video

| Command | Description |
|---------|-------------|
| `/cgenius video render <timeline_json>` | Render video via Remotion |
| `/cgenius video list` | List rendered videos |

**Example:**
```
/cgenius video render '{"scenes": [...]}'
```

---

### 12. Document

| Command | Description |
|---------|-------------|
| `/cgenius document <topic>` | Generate professional document |

**Options:**
- `--type report|brief|memo` (default: report)

**Example:**
```
/cgenius document "Market expansion analysis for EMEA" --type report
```

---

### 13. Analytics

| Command | Description |
|---------|-------------|
| `/cgenius analytics seo` | SEO performance data |
| `/cgenius analytics social` | Social media analytics |

---

## API Endpoints Reference

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/api/blog-writer-v2/tasks` | POST | Create blog task |
| `/api/blog-writer-v2/tasks/:id` | GET | Get task status |
| `/api/blog-writer-v2/tasks/:id/resume` | POST | Resume task |
| `/api/blog-writer-v2/tasks/:id/regenerate` | POST | Regenerate section |
| `/api/ai-json` | POST | Social post generation (SSE stream) |
| `/api/generate-text` | POST | Email/document generation |
| `/api/generate-proposal` | POST | Proposal generation (SSE stream) |
| `/api/proposal-pipeline` | POST | Proposal pipeline |
| `/api/content-ideation` | GET/POST | Content ideation CRUD |
| `/api/content-ideation/analyze` | POST | Analyze idea |
| `/api/content-ideation/:id/generate` | POST | Generate from idea |
| `/api/offer-agent` | POST | Offer agent |
| `/api/new-offer-agent` | POST | Research offer agent |
| `/api/questionnaire/create-token` | GET/POST | Questionnaire management |
| `/api/questionnaire/:token` | GET | Questionnaire status |
| `/api/pptx-generate` | POST | PPTX generation |
| `/api/remotion/render` | POST | Video render |
| `/api/remotion/files` | GET | List videos |

**Authentication:** All API calls require `Authorization: Bearer <CGENIUS_EPSIMO_TOKEN>` header.

**Streaming:** Proposal and social endpoints use SSE (Server-Sent Events) with EpsimoAI's cumulative format — each event contains the full response so far.

---

## Error Handling

If credentials are missing:
```
❌ CGENIUS_EPSIMO_TOKEN is not set.
Fix: Get your token from https://beta.cgenius.app/settings/api-tokens
     Then: export CGENIUS_EPSIMO_TOKEN="your_token"
```

---

## Links

- **App:** https://beta.cgenius.app
- **GitHub:** https://github.com/thierryteisseire/cgenius-skill
- **API Tokens:** https://beta.cgenius.app/settings/api-tokens
