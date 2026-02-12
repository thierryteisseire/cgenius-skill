# Content Genius Skill for Claude Code

Complete AI-powered content creation and proposal generation system with shareable client questionnaires.

## Overview

The Content Genius skill brings professional content creation and proposal automation directly to your Claude Code workflow. Generate emails, blog posts, social media content, proposals, and presentations with simple commands. Create shareable questionnaire links for client intake forms that require no login.

## Features

✅ **Content Generation**
- Professional emails
- SEO-optimized blog posts
- Engaging social media content
- Commercial proposals
- PowerPoint presentations

✅ **Shareable Questionnaires**
- Generate unique shareable links
- No login required for clients
- Custom branding support
- Auto-expiration
- Email notifications
- Optional auto-proposal generation

✅ **Complete Proposal Pipeline**
- 3-stage automated flow
- Market research integration
- Data-backed recommendations
- Async processing support

## Installation

```bash
# Install the skill
claude code skill install thierryteisseire/cgenius-skill

# Or install from source
git clone https://github.com/thierryteisseire/cgenius-skill.git
cd cgenius-skill
claude code skill install .
```

## Quick Start

### 1. Configure Credentials

Set up your Content Genius API credentials:

```bash
# Required: Your Epsimo token
export CGENIUS_EPSIMO_TOKEN="eyJhbGci..."

# Optional: For async operations
export CGENIUS_PROJECT_TOKEN="eyJhbGci..."

# Optional: Custom assistant ID
export CGENIUS_ASSISTANT_ID="a3067fe2-e21b-42f7-bf3c-7d875d6cfdf5"

# Optional: Custom API base URL
export CGENIUS_API_BASE_URL="https://beta.cgenius.app"
```

Or create a `.env` file in your project:

```bash
CGENIUS_EPSIMO_TOKEN=eyJ...
CGENIUS_PROJECT_TOKEN=eyJ...
CGENIUS_ASSISTANT_ID=a3067fe2-e21b-42f7-bf3c-7d875d6cfdf5
CGENIUS_API_BASE_URL=https://beta.cgenius.app
```

### 2. Start Using

```bash
# Generate an email
/cgenius email product launch announcement

# Create a shareable questionnaire
/cgenius questionnaire create "Acme Corp"

# Generate a complete proposal
/cgenius proposal-pipeline "TechStart Inc" --industry "Technology" --budget "$50k"
```

## Usage Examples

### Generate Professional Email

```bash
/cgenius email "New product launch announcement for Q2 2026"
```

**Output:**
```
## Generated Email

Subject: Exciting Product Launch - Q2 2026

Dear [Recipient],

We're thrilled to announce the launch of our latest product...

[Full email content]

---
Generated with Content Genius
```

### Create Shareable Questionnaire

```bash
/cgenius questionnaire create "Acme Corporation" \
  --email "contact@acme.com" \
  --expires 30 \
  --notify "team@agency.com" \
  --auto-proposal
```

**Output:**
```
## Shareable Questionnaire Created ✅

Client: Acme Corporation
Expires: March 14, 2026

Share Link:
🔗 https://beta.cgenius.app/questionnaire/qst_abc123...

Short URL:
🔗 https://beta.cgenius.app/q/qst_abc123

QR Code:
📷 https://api.qrserver.com/v1/create-qr-code/?...

How to Share:
• Email the link to contact@acme.com
• Text the short URL
• Print the QR code

What Happens Next:
1. Client clicks link (no login required)
2. Client fills out 13-question form
3. You get notified at team@agency.com
4. Proposal auto-generates

Expires in 30 days
```

### Generate Blog Post

```bash
/cgenius blog "10 AI trends transforming content marketing in 2026"
```

**Output:**
```
## Generated Blog Post

# 10 AI Trends Transforming Content Marketing in 2026

[SEO-optimized blog content with proper structure]

---
Word count: ~1500 words
Generated with Content Genius
```

### Create Social Media Post

```bash
/cgenius social "Excited to announce our new AI-powered content platform"
```

**Output:**
```
## Social Media Post

{
  "title": "🚀 Big News!",
  "body": "Excited to announce our new AI-powered content platform!

  Generate professional content in seconds:
  ✨ Emails
  ✨ Blogs
  ✨ Social posts
  ✨ Proposals

  #AI #ContentMarketing #Innovation"
}

---
Generated with Content Genius
```

### Generate Complete Proposal

```bash
/cgenius proposal "Acme Corp" "Content marketing, SEO, Social media" "$75,000" "3 months"
```

**Output:** (Streams proposal sections)
```
## Generating Proposal for Acme Corp

=== Introduction ===
[Proposal introduction content...]

=== Objectives ===
[Objectives content...]

=== Budget Allocation ===
[Budget breakdown...]

[... more sections ...]

✅ Proposal complete!
```

### Run Complete Proposal Pipeline

```bash
/cgenius proposal-pipeline "TechStart Inc" \
  --industry "Technology / SaaS" \
  --goals "Scale to $1M ARR" \
  --budget "$50,000 - $100,000" \
  --timeline "3 months" \
  --services "Content marketing, SEO, Social media" \
  --research-depth "standard"
```

**Output:**
```
## Proposal Pipeline Started 🚀

Pipeline ID: prop_1234567890
Client: TechStart Inc

Stage 1: ✅ Processing questionnaire
- Industry: Technology / SaaS
- Goal: Scale to $1M ARR
- Budget: $50,000 - $100,000

Stage 2: 🔍 Conducting market research
- Researching SaaS content marketing trends
- Analyzing competitor strategies
- Gathering industry statistics

Stage 3: 📝 Generating proposal
[Proposal content streams here...]

✅ Complete!

Generated proposal with:
• Executive summary
• Market analysis
• Proposed solution
• Budget breakdown
• Timeline
• Success metrics

Ready to send to client!
```

### Generate PowerPoint Presentation

```bash
/cgenius pptx "Product Roadmap 2026" \
  "Q1: Feature A" \
  "Q2: Feature B" \
  "Q3: Feature C"
```

**Output:**
```
## PowerPoint Generated ✅

File: Product_Roadmap_2026.pptx
Size: 87 KB
Slides: 5 (Title + 3 content + Thank you)

Download: [download link]

---
Generated with Content Genius
```

## All Commands

### Content Generation

| Command | Description | Example |
|---------|-------------|---------|
| `/cgenius email <topic>` | Generate professional email | `/cgenius email product launch` |
| `/cgenius blog <topic>` | Generate blog post | `/cgenius blog "AI trends 2026"` |
| `/cgenius social <message>` | Create social media post | `/cgenius social "new product"` |
| `/cgenius proposal <client> <services> <budget> <duration>` | Generate proposal | `/cgenius proposal "Acme" "SEO" "$50k" "3mo"` |
| `/cgenius pptx <title> <slides...>` | Create presentation | `/cgenius pptx "Title" "Slide 1" "Slide 2"` |

### Questionnaire Management

| Command | Description | Example |
|---------|-------------|---------|
| `/cgenius questionnaire create <client>` | Generate shareable link | `/cgenius questionnaire create "Acme"` |
| `/cgenius questionnaire list` | List all questionnaires | `/cgenius questionnaire list --status pending` |
| `/cgenius questionnaire status <token>` | Check submission status | `/cgenius questionnaire status qst_abc123` |

### Proposal Pipeline

| Command | Description | Example |
|---------|-------------|---------|
| `/cgenius proposal-pipeline <client>` | Run complete pipeline | `/cgenius proposal-pipeline "TechStart" --industry "Tech"` |

## Command Options

### Questionnaire Create Options

- `--email <email>` - Pre-fill client email
- `--expires <days>` - Expiration in days (default: 30)
- `--notify <email>` - Notification email address
- `--auto-proposal` - Auto-generate proposal on submission
- `--brand-name <name>` - Company name for branding
- `--brand-logo <url>` - Logo URL
- `--brand-color <hex>` - Primary color (hex code)

**Example:**
```bash
/cgenius questionnaire create "Acme Corp" \
  --email "contact@acme.com" \
  --expires 14 \
  --notify "team@agency.com" \
  --auto-proposal \
  --brand-name "Digital Agency" \
  --brand-logo "https://agency.com/logo.png" \
  --brand-color "#6366F1"
```

### Proposal Pipeline Options

- `--industry <industry>` - Client industry
- `--goals <goals>` - Business goals
- `--budget <budget>` - Budget range
- `--timeline <timeline>` - Project timeline
- `--services <services>` - Comma-separated services
- `--audience <audience>` - Target audience
- `--research-depth <quick|standard|deep>` - Research depth
- `--async` - Run asynchronously

**Example:**
```bash
/cgenius proposal-pipeline "FastGrow Co" \
  --industry "E-commerce" \
  --goals "Increase sales by 50%" \
  --budget "$25,000 - $50,000" \
  --timeline "2 months" \
  --services "Content, SEO, Social" \
  --audience "Fashion millennials" \
  --research-depth "deep" \
  --async
```

## Configuration

### Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `CGENIUS_EPSIMO_TOKEN` | Yes | - | Your Epsimo authentication token |
| `CGENIUS_PROJECT_TOKEN` | No | - | Token for async operations |
| `CGENIUS_ASSISTANT_ID` | No | `a3067fe2...` | AI assistant ID |
| `CGENIUS_API_BASE_URL` | No | `https://beta.cgenius.app` | API base URL |
| `NEXT_PUBLIC_EPSIMOAI_API_URL` | No | `backend.epsimoai.io` | EpsimoAI API URL |

### Getting Your Tokens

1. **Epsimo Token**:
   - Log in to https://beta.cgenius.app
   - Go to Settings → API Tokens
   - Create a new token
   - Copy the token (starts with `eyJ...`)

2. **Project Token**:
   - Same as above
   - Used for Trigger.dev async operations

3. **Assistant ID**:
   - Default provided
   - Or create your own in EpsimoAI platform

## Use Cases

### For Marketing Agencies

**Client Onboarding:**
```bash
# Generate questionnaire for new prospect
/cgenius questionnaire create "New Client" --auto-proposal

# Share link via email
# Client fills form
# Proposal auto-generates
# Close deal faster!
```

**Content Creation:**
```bash
# Bulk email campaign
/cgenius email "Q2 newsletter content"

# Social media schedule
/cgenius social "Monday motivation post"
/cgenius social "Product highlight post"

# Blog content calendar
/cgenius blog "SEO tips for 2026"
```

### For Consultants

**Proposal Generation:**
```bash
# Complete proposal with research
/cgenius proposal-pipeline "Enterprise Client" \
  --industry "Healthcare" \
  --budget "$100k+" \
  --research-depth "deep"
```

### For SaaS Companies

**Lead Qualification:**
```bash
# Embed questionnaire in "Get Demo" flow
/cgenius questionnaire create "Demo Request" \
  --auto-proposal \
  --notify "sales@company.com"
```

## API Integration

This skill connects to the following Content Genius APIs:

- `POST /api/generate-text` - Email generation
- `POST /api/blog-generate` - Blog content
- `POST /api/ai-json` - Social media posts
- `POST /api/generate-proposal` - Proposal generation
- `POST /api/pptx-generate` - PowerPoint files
- `POST /api/questionnaire/create-token` - Create questionnaire tokens
- `GET /api/questionnaire/[token]` - View questionnaire
- `POST /api/questionnaire/[token]` - Submit questionnaire
- `POST /api/proposal-pipeline` - Complete proposal pipeline

## Troubleshooting

### Error: Missing CGENIUS_EPSIMO_TOKEN

**Problem:** Token not configured

**Solution:**
```bash
export CGENIUS_EPSIMO_TOKEN="your_token_here"
```

Or add to `.env` file.

### Error: API request failed

**Problem:** Network or API issue

**Solutions:**
1. Check API is accessible: `curl https://beta.cgenius.app/api/questionnaire/create-token`
2. Verify token is valid
3. Check API_BASE_URL is correct

### Error: Invalid token format

**Problem:** Token expired or incorrect

**Solutions:**
1. Generate new token from https://beta.cgenius.app/settings/api-tokens
2. Copy complete token (starts with `eyJ`)
3. Set environment variable again

### Slow response times

**Problem:** API processing

**Solutions:**
1. Use `--async` flag for long operations
2. Use `--research-depth quick` for faster results
3. Check your internet connection

## Development

### Project Structure

```
cgenius-skill/
├── skill.json          # Skill manifest
├── instructions.md     # Skill implementation
├── README.md          # Documentation (this file)
├── LICENSE            # MIT License
└── examples/          # Usage examples
    ├── email.md
    ├── questionnaire.md
    └── proposal.md
```

### Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

### Running Tests

```bash
# Test email generation
/cgenius email "test email topic"

# Test questionnaire creation
/cgenius questionnaire create "Test Client"

# Test proposal pipeline
/cgenius proposal-pipeline "Test Co" --industry "Tech" --budget "$10k"
```

## Support

- **GitHub Issues**: https://github.com/thierryteisseire/cgenius-skill/issues
- **Documentation**: https://github.com/thierryteisseire/cgenius-skill
- **API Docs**: https://beta.cgenius.app/docs

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Credits

Created by Thierry Teisseire

Powered by:
- Content Genius Platform
- EpsimoAI
- Perplexity AI (market research)
- Claude Code

## Changelog

### v1.0.0 (2026-02-12)
- ✅ Initial release
- ✅ Email, blog, social, proposal, PPTX generation
- ✅ Shareable questionnaire system
- ✅ Complete proposal pipeline
- ✅ Comprehensive documentation

---

**Made with ❤️ for Claude Code**
