---
name: cgenius
description: Use this skill whenever the user wants to create AI-powered content including emails, blog posts, social media content, commercial proposals, or PowerPoint presentations. Also use when they want to generate shareable client questionnaires (intake forms requiring no login), or run a complete 3-stage proposal pipeline with market research. If the user mentions content creation, proposals, questionnaires, client intake forms, or marketing automation, use this skill.
license: MIT
---

# Content Genius Skill

AI-powered content creation and proposal generation system.

## Available Commands

### 1. Generate Content

**Email Generation**
```
/cgenius email <topic>
```
Generates professional email content.

**Blog Post**
```
/cgenius blog <topic>
```
Generates comprehensive blog content with SEO optimization.

**Social Media Post**
```
/cgenius social <message>
```
Creates engaging social media content in JSON format (title + body).

**Proposal**
```
/cgenius proposal <client_name> <services> <budget> <duration>
```
Generates a complete commercial proposal with multiple sections.

**Presentation (PPTX)**
```
/cgenius pptx <title> <slides...>
```
Creates a PowerPoint presentation file.

### 2. Shareable Questionnaires

**Create Questionnaire Link**
```
/cgenius questionnaire create <client_name> [options]
```
Generates a unique shareable link for client intake forms.

Options:
- `--email <email>` - Pre-fill client email
- `--expires <days>` - Expiration in days (default: 30)
- `--notify <email>` - Email for notifications
- `--auto-proposal` - Auto-generate proposal on submission

**List Questionnaires**
```
/cgenius questionnaire list [--status <pending|completed|expired>]
```
Lists all your questionnaire tokens.

**Check Status**
```
/cgenius questionnaire status <token>
```
Checks if a questionnaire has been submitted.

### 3. Complete Proposal Pipeline

**Generate Full Proposal**
```
/cgenius proposal-pipeline <client_name> [options]
```
Runs the complete 3-stage pipeline:
1. Process client information
2. Conduct market research
3. Generate custom proposal

Options:
- `--industry <industry>` - Client industry
- `--goals <goals>` - Business goals
- `--budget <budget>` - Budget range
- `--timeline <timeline>` - Project timeline
- `--services <services>` - Comma-separated services
- `--research-depth <quick|standard|deep>` - Research depth
- `--async` - Run asynchronously with Trigger.dev

## Configuration

Set up your credentials:

```bash
# Set API URL
export NEXT_PUBLIC_EPSIMOAI_API_URL="backend.epsimoai.io"

# Set Epsimo Token (required)
export CGENIUS_EPSIMO_TOKEN="eyJhbGci..."

# Set Project Token (for async operations)
export CGENIUS_PROJECT_TOKEN="eyJhbGci..."

# Set Assistant ID (optional, uses default)
export CGENIUS_ASSISTANT_ID="a3067fe2-e21b-42f7-bf3c-7d875d6cfdf5"

# Set API Base URL (optional)
export CGENIUS_API_BASE_URL="https://beta.cgenius.app"
```

Or create a `.env` file:
```bash
NEXT_PUBLIC_EPSIMOAI_API_URL=backend.epsimoai.io
CGENIUS_EPSIMO_TOKEN=eyJ...
CGENIUS_PROJECT_TOKEN=eyJ...
CGENIUS_ASSISTANT_ID=a3067fe2-e21b-42f7-bf3c-7d875d6cfdf5
CGENIUS_API_BASE_URL=https://beta.cgenius.app
```

## API Integration

This skill integrates with Content Genius APIs:

- `/api/generate-text` - Email generation
- `/api/blog-generate` - Blog content
- `/api/ai-json` - Social media posts
- `/api/generate-proposal` - Proposal generation
- `/api/pptx-generate` - PowerPoint files
- `/api/questionnaire/create-token` - Questionnaire tokens
- `/api/questionnaire/[token]` - Questionnaire access
- `/api/proposal-pipeline` - Complete proposal flow

## Usage Instructions

When the user invokes `/cgenius`, analyze their request and:

1. **Parse the command** - Identify the operation (email, blog, social, proposal, questionnaire, etc.)

2. **Gather required information** - If the user hasn't provided all required fields, ask them:
   - For email: topic/purpose
   - For blog: topic, optional title
   - For social: message/content
   - For proposal: client name, services, budget, duration
   - For questionnaire: client name, optional settings
   - For proposal pipeline: client name, industry, goals, budget, etc.

3. **Make API call** - Use the appropriate endpoint with proper authentication

4. **Handle response** - Present the results to the user:
   - For content: Show generated text
   - For proposals: Show sections as they stream
   - For questionnaires: Show shareable URL and instructions
   - For PPTX: Provide download link

5. **Error handling** - If API calls fail:
   - Check if credentials are configured
   - Verify the API is accessible
   - Provide helpful error messages

## Examples

### Generate Email
```
User: /cgenius email product launch announcement
Claude: I'll generate a professional email for a product launch announcement...
```

### Create Questionnaire
```
User: /cgenius questionnaire create "Acme Corp" --email contact@acme.com --notify team@agency.com
Claude: Created shareable questionnaire for Acme Corp!

URL: https://beta.cgenius.app/questionnaire/qst_abc123...
Short URL: https://beta.cgenius.app/q/qst_abc123
QR Code: https://api.qrserver.com/v1/create-qr-code/?...

Share this link with your client - they can fill it out without logging in!
Expires in 30 days.
You'll be notified at team@agency.com when they submit.
```

### Generate Full Proposal
```
User: /cgenius proposal-pipeline "TechStart Inc" --industry "Technology" --budget "$50k" --timeline "3 months" --services "Content marketing, SEO"
Claude: Starting complete proposal pipeline for TechStart Inc...

Stage 1: ✅ Processing client information
- Industry: Technology
- Budget: $50,000
- Timeline: 3 months
- Services: Content marketing, SEO

Stage 2: 🔍 Conducting market research...
- Researching content marketing trends in Technology
- Researching SEO best practices
- Analyzing target audience behavior

Stage 3: 📝 Generating proposal...
[Proposal content streams here...]

✅ Complete proposal ready!
```

## Implementation Logic

When invoked, follow this logic:

```typescript
async function handleCGeniusCommand(args: string[]) {
  const command = args[0];
  const API_BASE = process.env.CGENIUS_API_BASE_URL || 'https://beta.cgenius.app';
  const EPSIMO_TOKEN = process.env.CGENIUS_EPSIMO_TOKEN;
  const PROJECT_TOKEN = process.env.CGENIUS_PROJECT_TOKEN;
  const ASSISTANT_ID = process.env.CGENIUS_ASSISTANT_ID || 'a3067fe2-e21b-42f7-bf3c-7d875d6cfdf5';

  // Check credentials
  if (!EPSIMO_TOKEN && ['email', 'blog', 'social', 'proposal', 'proposal-pipeline'].includes(command)) {
    throw new Error('CGENIUS_EPSIMO_TOKEN not set. Please configure your credentials.');
  }

  switch (command) {
    case 'email':
      return await generateEmail(args.slice(1).join(' '));

    case 'blog':
      return await generateBlog(args.slice(1).join(' '));

    case 'social':
      return await generateSocial(args.slice(1).join(' '));

    case 'proposal':
      return await generateProposal(args.slice(1));

    case 'pptx':
      return await generatePPTX(args.slice(1));

    case 'questionnaire':
      return await handleQuestionnaire(args.slice(1));

    case 'proposal-pipeline':
      return await runProposalPipeline(args.slice(1));

    default:
      return showHelp();
  }
}

async function generateEmail(topic: string) {
  const response = await fetch(`${API_BASE}/api/generate-text`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      user_id: 'contact@epsimoai.com',
      password: 'EpsimoAI184',
      projet: '2e8693e4-9b7b-4fe4-a289-0822b2db3665',
      assistant_id: ASSISTANT_ID,
      thread_name: `Email - ${topic}`,
      request_content: `Write a professional email about: ${topic}`
    })
  });

  const data = await response.json();
  return data.content;
}

async function generateBlog(topic: string) {
  const response = await fetch(`${API_BASE}/api/blog-generate`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      blogPrompt: topic,
      epsimo_token: EPSIMO_TOKEN,
      assistant_id: ASSISTANT_ID
    })
  });

  // Handle streaming response
  const reader = response.body.getReader();
  let content = '';

  while (true) {
    const { done, value } = await reader.read();
    if (done) break;
    content += new TextDecoder().decode(value);
  }

  return content;
}

async function generateSocial(message: string) {
  const response = await fetch(`${API_BASE}/api/ai-json`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      assistant_id: ASSISTANT_ID,
      epsimo_token: EPSIMO_TOKEN,
      userMessage: message
    })
  });

  // Handle streaming response
  const reader = response.body.getReader();
  let content = '';

  while (true) {
    const { done, value } = await reader.read();
    if (done) break;
    content += new TextDecoder().decode(value);
  }

  return content;
}

async function generateProposal(args: string[]) {
  // Parse args: client_name services budget duration
  const [client_name, services, budget, duration] = args;

  const response = await fetch(`${API_BASE}/api/generate-proposal`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      assistant_id: ASSISTANT_ID,
      epsimo_token: EPSIMO_TOKEN,
      client_name,
      services,
      budget,
      campaign_duration: duration,
      objectives: 'Generate comprehensive proposal'
    })
  });

  // Handle streaming response
  const reader = response.body.getReader();
  let content = '';

  while (true) {
    const { done, value } = await reader.read();
    if (done) break;
    content += new TextDecoder().decode(value);
  }

  return content;
}

async function handleQuestionnaire(args: string[]) {
  const subcommand = args[0];

  if (subcommand === 'create') {
    const client_name = args[1];
    // Parse options like --email, --expires, --notify

    const response = await fetch(`${API_BASE}/api/questionnaire/create-token`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        client_name,
        // Add parsed options
      })
    });

    const data = await response.json();
    return formatQuestionnaireResponse(data);
  }

  if (subcommand === 'list') {
    const response = await fetch(`${API_BASE}/api/questionnaire/create-token`);
    const data = await response.json();
    return formatQuestionnaireList(data);
  }

  if (subcommand === 'status') {
    const token = args[1];
    const response = await fetch(`${API_BASE}/api/questionnaire/${token}`);
    const data = await response.json();
    return formatQuestionnaireStatus(data);
  }
}

async function runProposalPipeline(args: string[]) {
  const client_name = args[0];
  // Parse options

  const response = await fetch(`${API_BASE}/api/proposal-pipeline`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      client_name,
      epsimo_token: EPSIMO_TOKEN,
      project_token: PROJECT_TOKEN,
      assistant_id: ASSISTANT_ID,
      // Add parsed options
    })
  });

  const data = await response.json();
  return formatPipelineResponse(data);
}
```

## Response Formatting

Format responses clearly:

### Email Response
```markdown
## Generated Email

**Subject:** [Generated subject line]

**Body:**
[Generated email content]

---
*Generated with Content Genius*
```

### Questionnaire Response
```markdown
## Shareable Questionnaire Created ✅

**Client:** Acme Corp
**Expires:** March 14, 2026

**Share Link:**
🔗 https://beta.cgenius.app/questionnaire/qst_abc123...

**Short URL:**
🔗 https://beta.cgenius.app/q/qst_abc123

**QR Code:**
📷 https://api.qrserver.com/v1/create-qr-code/?...

### How to Share:
- **Email:** Copy the link and send to client@acme.com
- **SMS:** Text the short URL
- **Print:** Download and print the QR code

### What Happens Next:
1. Client clicks link (no login required)
2. Client fills out questionnaire
3. You get notified when submitted
4. [Optional] Proposal auto-generates

*Expires in 30 days*
```

### Proposal Pipeline Response
```markdown
## Proposal Pipeline Started 🚀

**Pipeline ID:** prop_abc123
**Client:** TechStart Inc

### Progress:
✅ Stage 1: Questionnaire - Complete
🔍 Stage 2: Market Research - In Progress
⏳ Stage 3: Proposal Generation - Pending

### Research Topics:
1. Content marketing trends in Technology
2. SEO best practices for SaaS
3. Target audience analysis

Check status: [status URL]

*Estimated completion: 3-5 minutes*
```

## Error Handling

Provide helpful error messages:

```markdown
## Error: Missing Credentials ❌

The CGENIUS_EPSIMO_TOKEN environment variable is not set.

### To fix this:

1. Get your token from: https://beta.cgenius.app/settings/api-tokens
2. Set the environment variable:
   ```bash
   export CGENIUS_EPSIMO_TOKEN="your_token_here"
   ```
3. Or add to your .env file

Need help? Check the documentation: https://github.com/thierryteisseire/cgenius-skill
```

## Advanced Features

### Batch Operations
```
/cgenius batch emails topics.txt
```
Generate multiple emails from a list of topics.

### Custom Templates
```
/cgenius proposal --template healthcare
```
Use industry-specific proposal templates.

### Analytics
```
/cgenius stats
```
Show usage statistics and success rates.

## Best Practices

1. **Always check credentials** before making API calls
2. **Handle streaming responses** properly for real-time feedback
3. **Provide progress updates** for long-running operations
4. **Format output clearly** with markdown
5. **Include next steps** in responses
6. **Handle errors gracefully** with helpful messages

## Support

- GitHub: https://github.com/thierryteisseire/cgenius-skill
- Issues: https://github.com/thierryteisseire/cgenius-skill/issues
- Documentation: https://github.com/thierryteisseire/cgenius-skill/blob/main/README.md
