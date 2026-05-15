---
name: cgenius
description: Use this skill when the user wants to create content (blogs, social posts, emails, proposals, presentations, documents), manage content ideation, configure brand voice, generate offers, schedule/publish content, render videos, or manage questionnaires. Trigger on mentions of content creation, blog writing, social media, proposals, brand voice, content calendar, publishing, video, offers, or marketing automation.
license: MIT
---

# Content Genius Skill v2.0

AI-powered content platform: blog generation, social media, proposals, content ideation, brand voice, publishing, video, and offer automation.

## Configuration

```bash
CGENIUS_API_BASE_URL=https://beta.cgenius.app
CGENIUS_EPSIMO_TOKEN=<your_epsimo_token>
CGENIUS_ASSISTANT_ID=<your_assistant_id>
CGENIUS_APPSYNC_URL=<appsync_endpoint>        # For GraphQL features
CGENIUS_APPSYNC_API_KEY=<api_key>              # For GraphQL features
```

## Shared Helpers

### EpsimoAI SSE Stream Consumer
All streaming endpoints use EpsimoAI's cumulative SSE format. Each event contains the full response so far.

```typescript
const API_BASE = process.env.CGENIUS_API_BASE_URL || 'https://beta.cgenius.app';
const EPSIMO_TOKEN = process.env.CGENIUS_EPSIMO_TOKEN;
const ASSISTANT_ID = process.env.CGENIUS_ASSISTANT_ID; // Required
const APPSYNC_URL = process.env.CGENIUS_APPSYNC_URL;
const APPSYNC_API_KEY = process.env.CGENIUS_APPSYNC_API_KEY;

async function consumeStream(response: Response): Promise<string> {
  const reader = response.body.getReader();
  const decoder = new TextDecoder();
  let buffer = '', fullContent = '';
  while (true) {
    const { done, value } = await reader.read();
    if (done) break;
    buffer += decoder.decode(value, { stream: true });
    let end = buffer.indexOf('\n\n');
    while (end !== -1) {
      const block = buffer.slice(0, end);
      buffer = buffer.slice(end + 2);
      let data = '';
      for (const line of block.split('\n'))
        if (line.startsWith('data: ')) data = line.slice(6).trim();
      if (data && data !== '[DONE]') {
        try {
          const msgs = JSON.parse(data);
          for (const m of Array.isArray(msgs) ? msgs : [msgs])
            if (m.type === 'ai' && m.content?.length > fullContent.length)
              fullContent = m.content;
        } catch {}
      }
      end = buffer.indexOf('\n\n');
    }
  }
  return fullContent;
}

async function graphql(query: string, variables?: any) {
  const res = await fetch(APPSYNC_URL, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json', 'x-api-key': APPSYNC_API_KEY },
    body: JSON.stringify({ query, variables }),
  });
  const { data, errors } = await res.json();
  if (errors?.length) throw new Error(errors[0].message);
  return data;
}

async function pollTask(taskId: string): Promise<any> {
  for (let i = 0; i < 100; i++) {
    await new Promise(r => setTimeout(r, 3000));
    const res = await fetch(`${API_BASE}/api/blog-writer-v2/tasks/${taskId}`);
    const task = await res.json();
    if (task.status === 'COMPLETE' || task.status === 'FAILED') return task;
  }
  throw new Error('Task timed out');
}
```

---

## Commands

### 1. Blog Generation

#### `/cgenius blog generate <topic> [options]`
Creates a blog using the V2 async pipeline (RESEARCH → OUTLINE → WRITING → COMPLETE).

Options: `--size single|small|medium|big|huge`, `--tone <tone>`, `--words <number>`, `--keywords <kw>`, `--brand-voice <voice>`, `--title <title>`

```typescript
async function blogGenerate(topic, options) {
  const res = await fetch(`${API_BASE}/api/blog-writer-v2/tasks`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      requestData: {
        topic,
        keywords: options.keywords || '',
        tone: options.tone || 'professional',
        wordCount: parseInt(options.words) || 1500,
        blogSize: options.size || 'medium',
        brandVoice: options.brandVoice || '',
        title: options.title || '',
        assistantId: ASSISTANT_ID,
      },
      epsimoToken: EPSIMO_TOKEN,
    }),
  });
  const { taskId } = await res.json();
  const task = await pollTask(taskId);
  if (task.status === 'FAILED') throw new Error(task.error);
  return task.resultData.finalContent; // HTML
}
```

#### `/cgenius blog list`
```typescript
async function blogList() {
  return await graphql(`query { listBlogs(limit: 20) { items { id title createdAt } } }`);
}
```

#### `/cgenius blog status <taskId>`
```typescript
async function blogStatus(taskId) {
  const res = await fetch(`${API_BASE}/api/blog-writer-v2/tasks/${taskId}`);
  return await res.json(); // { status, currentPhase, progress, resultData }
}
```

#### `/cgenius blog resume <taskId>`
```typescript
async function blogResume(taskId) {
  const res = await fetch(`${API_BASE}/api/blog-writer-v2/tasks/${taskId}/resume`, { method: 'POST' });
  return await res.json();
}
```

#### `/cgenius blog regenerate <taskId> <sectionIndex>`
```typescript
async function blogRegenerate(taskId, sectionIndex) {
  const res = await fetch(`${API_BASE}/api/blog-writer-v2/tasks/${taskId}/regenerate`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ sectionIndex: parseInt(sectionIndex) }),
  });
  return await res.json();
}
```

---

### 2. Social Media

#### `/cgenius social <message>`
Generates a social post (title + body) via EpsimoAI streaming.

```typescript
async function socialGenerate(message) {
  const res = await fetch(`${API_BASE}/api/ai-json`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ assistant_id: ASSISTANT_ID, epsimo_token: EPSIMO_TOKEN, userMessage: message }),
  });
  const content = await consumeStream(res);
  // Extract JSON from markdown code block
  const match = content.match(/```json\n([\s\S]*?)\n```/);
  return match ? JSON.parse(match[1]) : { body: content };
}
```

---

### 3. Email

#### `/cgenius email <topic>`
```typescript
async function emailGenerate(topic) {
  const res = await fetch(`${API_BASE}/api/generate-text`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      assistant_id: ASSISTANT_ID,
      epsimo_token: EPSIMO_TOKEN,
      thread_name: `Email - ${topic}`,
      request_content: `Write a professional email about: ${topic}`,
    }),
  });
  const data = await res.json();
  return data.content;
}
```

---

### 4. Proposal

#### `/cgenius proposal <client_name> [--services <s>] [--budget <b>] [--duration <d>]`
```typescript
async function proposalGenerate(clientName, options) {
  const res = await fetch(`${API_BASE}/api/generate-proposal`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      assistant_id: ASSISTANT_ID,
      epsimo_token: EPSIMO_TOKEN,
      client_name: clientName,
      services: options.services,
      budget: options.budget,
      campaign_duration: options.duration,
      objectives: 'Generate comprehensive proposal',
    }),
  });
  return await consumeStream(res);
}
```

#### `/cgenius proposal-pipeline <client_name> [options]`
Options: `--industry`, `--goals`, `--budget`, `--timeline`, `--services`

```typescript
async function proposalPipeline(clientName, options) {
  const res = await fetch(`${API_BASE}/api/proposal-pipeline`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      client_name: clientName,
      epsimo_token: EPSIMO_TOKEN,
      assistant_id: ASSISTANT_ID,
      industry: options.industry,
      goals: options.goals,
      budget: options.budget,
      timeline: options.timeline,
      services: options.services,
    }),
  });
  return await res.json();
}
```

---

### 5. Content Ideation

#### `/cgenius ideate create <topic> [--priority high|medium|low]`
```typescript
async function ideateCreate(topic, options) {
  const res = await fetch(`${API_BASE}/api/content-ideation`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ topic, priority: options.priority || 'medium', status: 'new' }),
  });
  return await res.json();
}
```

#### `/cgenius ideate list [--status new|analyzing|ready|published]`
```typescript
async function ideateList(options) {
  const params = new URLSearchParams();
  if (options.status) params.set('status', options.status);
  const res = await fetch(`${API_BASE}/api/content-ideation?${params}`);
  return await res.json();
}
```

#### `/cgenius ideate analyze <id>`
```typescript
async function ideateAnalyze(id) {
  const res = await fetch(`${API_BASE}/api/content-ideation/analyze`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ ideationId: id }),
  });
  return await res.json();
}
```

#### `/cgenius ideate generate <id> [--type blog|social|email]`
```typescript
async function ideateGenerate(id, options) {
  const res = await fetch(`${API_BASE}/api/content-ideation/${id}/generate`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ type: options.type || 'blog' }),
  });
  return await res.json();
}
```

---

### 6. Brand Voice

#### `/cgenius brand-voice create <name> [--tone <tone>] [--description <desc>]`
```typescript
async function brandVoiceCreate(name, options) {
  return await graphql(`mutation CreateBrandVoice($input: CreateBrandVoiceInput!) {
    createBrandVoice(input: $input) { id name tone description createdAt }
  }`, { input: { name, tone: options.tone || '', description: options.description || '' } });
}
```

#### `/cgenius brand-voice list`
```typescript
async function brandVoiceList() {
  return await graphql(`query { listBrandVoices(limit: 50) { items { id name tone description } } }`);
}
```

---

### 7. Offer Agent

#### `/cgenius offer create <client_name> [--subjects <comma-separated>]`
Triggers the batch offer agent (async via Trigger.dev).

```typescript
async function offerCreate(clientName, options) {
  const subjects = (options.subjects || 'Market Analysis,Client Context,Competitor Analysis,Offer Components').split(',');
  const res = await fetch(`${API_BASE}/api/offer-agent`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      client_name: clientName,
      subjects,
      assistant_id: ASSISTANT_ID,
      epsimo_token: EPSIMO_TOKEN,
    }),
  });
  return await res.json(); // { id, publicAccessToken }
}
```

#### `/cgenius offer research <client_name> [--industry <industry>]`
```typescript
async function offerResearch(clientName, options) {
  const res = await fetch(`${API_BASE}/api/new-offer-agent`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      client_name: clientName,
      industry: options.industry,
      assistant_id: ASSISTANT_ID,
      epsimo_token: EPSIMO_TOKEN,
    }),
  });
  return await res.json();
}
```

---

### 8. Questionnaire

#### `/cgenius questionnaire create <client_name> [--email <e>] [--expires <days>] [--notify <email>]`
```typescript
async function questionnaireCreate(clientName, options) {
  const res = await fetch(`${API_BASE}/api/questionnaire/create-token`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      client_name: clientName,
      client_email: options.email,
      expires_days: parseInt(options.expires) || 30,
      notify_email: options.notify,
    }),
  });
  return await res.json(); // { token, url, shortUrl, qrCode, expiresAt }
}
```

#### `/cgenius questionnaire list`
```typescript
async function questionnaireList() {
  const res = await fetch(`${API_BASE}/api/questionnaire/create-token`);
  return await res.json();
}
```

#### `/cgenius questionnaire status <token>`
```typescript
async function questionnaireStatus(token) {
  const res = await fetch(`${API_BASE}/api/questionnaire/${token}`);
  return await res.json();
}
```

---

### 9. Publish / Schedule

#### `/cgenius publish schedule <contentId> --platform <platform> --date <date>`
Platforms: `blog`, `social`, `email`, `newsletter`

```typescript
async function publishSchedule(contentId, options) {
  return await graphql(`mutation CreatePublicationSchedule($input: CreatePublicationScheduleInput!) {
    createPublicationSchedule(input: $input) { id contentId platform publishDate status }
  }`, { input: { contentId, platform: options.platform, publishDate: options.date, status: 'scheduled' } });
}
```

#### `/cgenius publish list [--status draft|scheduled|published|failed]`
```typescript
async function publishList(options) {
  const filter = options.status ? `filter: { status: { eq: "${options.status}" } }` : '';
  return await graphql(`query { listPublicationSchedules(limit: 50 ${filter ? ',' + filter : ''}) {
    items { id contentId platform publishDate status publishedAt } } }`);
}
```

---

### 10. Presentation (PPTX)

#### `/cgenius pptx <topic> [--slides <number>]`
```typescript
async function pptxGenerate(topic, options) {
  const res = await fetch(`${API_BASE}/api/pptx-generate`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      assistant_id: ASSISTANT_ID,
      epsimo_token: EPSIMO_TOKEN,
      topic,
      slides: parseInt(options.slides) || 10,
    }),
  });
  return await res.json(); // { url }
}
```

---

### 11. Video

#### `/cgenius video render <timeline_json>`
```typescript
async function videoRender(timelineJson) {
  const timeline = JSON.parse(timelineJson);
  const res = await fetch(`${API_BASE}/api/remotion/render`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(timeline),
  });
  return await res.json(); // { url, renderId }
}
```

#### `/cgenius video list`
```typescript
async function videoList() {
  const res = await fetch(`${API_BASE}/api/remotion/files`);
  return await res.json();
}
```

---

### 12. Document

#### `/cgenius document <topic> [--type report|brief|memo]`
```typescript
async function documentGenerate(topic, options) {
  const type = options.type || 'report';
  const res = await fetch(`${API_BASE}/api/generate-text`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      assistant_id: ASSISTANT_ID,
      epsimo_token: EPSIMO_TOKEN,
      thread_name: `Document - ${topic}`,
      request_content: `Create a professional ${type} about: ${topic}`,
    }),
  });
  const data = await res.json();
  return data.content;
}
```

---

### 13. Analytics

#### `/cgenius analytics seo`
```typescript
async function analyticsSeo() {
  return await graphql(`query { listBlogs(limit: 50) { items { id title createdAt updatedAt } } }`);
}
```

#### `/cgenius analytics social`
```typescript
async function analyticsSocial() {
  return await graphql(`query { listPublicationSchedules(filter: { platform: { eq: "social" } }, limit: 50) {
    items { id contentId publishDate status publishedAt } } }`);
}
```

---

## Usage Instructions

When the user invokes `/cgenius`:

1. **Parse command** — identify the operation from the first argument
2. **Check credentials** — verify `CGENIUS_EPSIMO_TOKEN` is set for API calls, `CGENIUS_APPSYNC_URL` + `CGENIUS_APPSYNC_API_KEY` for GraphQL
3. **Gather missing info** — ask user for required fields not provided
4. **Execute** — call the appropriate function
5. **Format response** — present results clearly in markdown
6. **Handle errors** — show helpful messages with fix instructions

## Error Handling

```markdown
## ❌ Error: Missing Credentials

CGENIUS_EPSIMO_TOKEN is not set.

### Fix:
1. Get your token from: https://beta.cgenius.app/settings/api-tokens
2. Set: `export CGENIUS_EPSIMO_TOKEN="your_token"`
```

## Response Formatting

- **Blog:** Show HTML content or link to view
- **Social:** Show `{ title, body }` formatted
- **Email:** Show subject + body
- **Proposal:** Show sections as they complete
- **Questionnaire:** Show shareable URL + QR code link
- **Task status:** Show phase + progress bar
- **Lists:** Show as tables with key fields
