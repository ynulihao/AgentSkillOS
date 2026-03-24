---
name: openai-api
description: "Build with OpenAI's stateless Chat Completions, Embeddings, Images, Audio, and Moderation APIs. Use when implementing GPT-5/GPT-4o chat completions, streaming SSE responses, adding function calling or structured JSON outputs, generating embeddings for RAG, creating images with DALL-E 3, transcribing audio with Whisper, synthesizing speech with TTS, or troubleshooting 401/429 errors. Covers Node.js SDK and fetch-based approaches for Cloudflare Workers."
---

# OpenAI API

**Package**: `openai@6.9.1` | **API**: Stateless

## Workflow

1. **Install and configure** -- `npm install openai@6.9.1`, set `OPENAI_API_KEY` in env
2. **Select model** -- GPT-5.1 (reasoning), GPT-5 (balanced), GPT-4o (vision/multimodal), GPT-5-mini (cost-effective)
3. **Choose API** -- Chat Completions, Embeddings, Images, Audio, or Moderation
4. **Implement** -- Use Node.js SDK or fetch (for Cloudflare Workers)
5. **Add streaming** -- Set `stream: true` for responses >100 tokens
6. **Handle errors** -- Implement exponential backoff for 429s, validate API keys
7. **Validate** -- Check token usage, test tool calls loop to completion, verify structured output parsing

---

## Quick Start

### Installation

```bash
npm install openai@6.9.1
```

### Environment Setup

```bash
export OPENAI_API_KEY="sk-..."
```

Or create `.env` file:
```
OPENAI_API_KEY=sk-...
```

### First Chat Completion (Node.js SDK)

```typescript
import OpenAI from 'openai';

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});

const completion = await openai.chat.completions.create({
  model: 'gpt-5',
  messages: [
    { role: 'user', content: 'What are the three laws of robotics?' }
  ],
});

console.log(completion.choices[0].message.content);
```

### First Chat Completion (Fetch - Cloudflare Workers)

```typescript
const response = await fetch('https://api.openai.com/v1/chat/completions', {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${env.OPENAI_API_KEY}`,
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    model: 'gpt-5',
    messages: [
      { role: 'user', content: 'What are the three laws of robotics?' }
    ],
  }),
});

const data = await response.json();
console.log(data.choices[0].message.content);
```

---

## Chat Completions API

**Endpoint**: `POST /v1/chat/completions`

The Chat Completions API is the core interface for interacting with OpenAI's language models. It supports conversational AI, text generation, function calling, structured outputs, and vision capabilities.

### Supported Models

#### GPT-5 Series (Released August 2025)
- **gpt-5**: Full-featured reasoning model with advanced capabilities
- **gpt-5-mini**: Cost-effective alternative with good performance
- **gpt-5-nano**: Smallest/fastest variant for simple tasks

#### GPT-4o Series
- **gpt-4o**: Multimodal model with vision capabilities
- **gpt-4-turbo**: Fast GPT-4 variant

#### GPT-4 Series
- **gpt-4**: Original GPT-4 model

### Basic Request Structure

```typescript
{
  model: string,              // Model to use (e.g., "gpt-5")
  messages: Message[],        // Conversation history
  reasoning_effort?: string,  // GPT-5 only: "minimal" | "low" | "medium" | "high"
  verbosity?: string,         // GPT-5 only: "low" | "medium" | "high"
  temperature?: number,       // NOT supported by GPT-5
  max_tokens?: number,        // Max tokens to generate
  stream?: boolean,           // Enable streaming
  tools?: Tool[],             // Function calling tools
}
```

**Response**: Access generated text via `completion.choices[0].message.content`. Check `finish_reason` for "stop" (complete), "length" (truncated), or "tool_calls" (function calling).

**Message roles**: `system` (behavior), `user` (input), `assistant` (model responses). API is stateless -- send full conversation history each request.

---

## GPT-5 Series Models

GPT-5 models (released August 2025) introduce reasoning and verbosity controls:

### GPT-5.1 (Released November 13, 2025)

**Latest model with major improvements**:
- **gpt-5.1**: Adaptive reasoning that varies thinking time dynamically
- **24-hour extended prompt caching**: Faster follow-up queries at lower cost
- **New developer tools**: apply_patch (code editing), shell (command execution)

**BREAKING CHANGE**: GPT-5.1 defaults to `reasoning_effort: 'none'` (vs GPT-5 defaulting to `'medium'`). Update your code when migrating!

### reasoning_effort Parameter

Controls thinking depth (available on GPT-5 and GPT-5.1):
- **"none"**: No reasoning (fastest, lowest latency) - GPT-5.1 default
- **"minimal"**: Quick responses, minimal thinking
- **"low"**: Basic reasoning
- **"medium"**: Balanced reasoning - GPT-5 default
- **"high"**: Deep reasoning for complex problems

```typescript
// GPT-5.1 with no reasoning (fast)
const completion = await openai.chat.completions.create({
  model: 'gpt-5.1',
  messages: [{ role: 'user', content: 'Simple query' }],
  // reasoning_effort: 'none' is implicit default for GPT-5.1
});

// GPT-5.1 with high reasoning (complex tasks)
const completion = await openai.chat.completions.create({
  model: 'gpt-5.1',
  messages: [{ role: 'user', content: 'Solve this complex math problem...' }],
  reasoning_effort: 'high',
});
```

### verbosity Parameter

Controls output detail (GPT-5/GPT-5.1):
- **"low"**: Concise
- **"medium"**: Balanced (default)
- **"high"**: Verbose

### GPT-5 Limitations

**NOT Supported**:
- ❌ `temperature`, `top_p`, `logprobs` parameters
- ❌ Stateful Chain of Thought between turns

**Alternatives**: Use GPT-4o for temperature/top_p, or `openai-responses` skill for stateful reasoning

---

## Streaming Patterns

Enable with `stream: true` for token-by-token delivery.

### Node.js SDK
```typescript
const stream = await openai.chat.completions.create({
  model: 'gpt-5.1',
  messages: [{ role: 'user', content: 'Write a poem' }],
  stream: true,
});

for await (const chunk of stream) {
  const content = chunk.choices[0]?.delta?.content || '';
  process.stdout.write(content);
}
```

For **fetch-based streaming** (Cloudflare Workers): Read the response body with `getReader()`, parse SSE lines starting with `data: `, handle `[DONE]` signal, and skip invalid JSON chunks gracefully.

---

## Function Calling

Define tools with JSON schema, model invokes them based on context.

### Tool Definition & Request
```typescript
const tools = [{
  type: 'function',
  function: {
    name: 'get_weather',
    description: 'Get current weather for a location',
    parameters: {
      type: 'object',
      properties: {
        location: { type: 'string', description: 'City name' },
        unit: { type: 'string', enum: ['celsius', 'fahrenheit'] }
      },
      required: ['location']
    }
  }
}];

const completion = await openai.chat.completions.create({
  model: 'gpt-5.1',
  messages: [{ role: 'user', content: 'What is the weather in SF?' }],
  tools: tools,
});
```

### Handle Tool Calls
```typescript
const message = completion.choices[0].message;

if (message.tool_calls) {
  for (const toolCall of message.tool_calls) {
    const args = JSON.parse(toolCall.function.arguments);
    const result = await executeFunction(toolCall.function.name, args);

    // Send result back to model
    await openai.chat.completions.create({
      model: 'gpt-5.1',
      messages: [
        ...messages,
        message,
        {
          role: 'tool',
          tool_call_id: toolCall.id,
          content: JSON.stringify(result)
        }
      ],
      tools: tools,
    });
  }
}
```

**Loop pattern**: Continue calling API until no tool_calls in response.

---

## Structured Outputs

Structured outputs allow you to enforce JSON schema validation on model responses.

### Using JSON Schema

```typescript
const completion = await openai.chat.completions.create({
  model: 'gpt-4o', // Note: Structured outputs best supported on GPT-4o
  messages: [
    { role: 'user', content: 'Generate a person profile' }
  ],
  response_format: {
    type: 'json_schema',
    json_schema: {
      name: 'person_profile',
      strict: true,
      schema: {
        type: 'object',
        properties: {
          name: { type: 'string' },
          age: { type: 'number' },
          skills: {
            type: 'array',
            items: { type: 'string' }
          }
        },
        required: ['name', 'age', 'skills'],
        additionalProperties: false
      }
    }
  }
});

const person = JSON.parse(completion.choices[0].message.content);
// { name: "Alice", age: 28, skills: ["TypeScript", "React"] }
```

For simple JSON without strict schemas, use `response_format: { type: 'json_object' }`. Always include "JSON" in the prompt.

---

## Vision (GPT-4o)

GPT-4o supports image understanding alongside text.

### Image via URL

```typescript
const completion = await openai.chat.completions.create({
  model: 'gpt-4o',
  messages: [
    {
      role: 'user',
      content: [
        { type: 'text', text: 'What is in this image?' },
        {
          type: 'image_url',
          image_url: {
            url: 'https://example.com/image.jpg'
          }
        }
      ]
    }
  ]
});
```

For base64 images, use `data:image/jpeg;base64,${base64String}` as the URL. Multiple images can be passed in the same `content` array.

---

## Embeddings API

**Endpoint**: `POST /v1/embeddings`

Convert text to vectors for semantic search and RAG.

### Models
- **text-embedding-3-large**: 3072 dims (custom: 256-3072), highest quality
- **text-embedding-3-small**: 1536 dims (custom: 256-1536), cost-effective, recommended

### Basic Request
```typescript
const embedding = await openai.embeddings.create({
  model: 'text-embedding-3-small',
  input: 'The food was delicious.',
});
// Returns: { data: [{ embedding: [0.002, -0.009, ...] }] }
```

Use `dimensions` parameter to reduce vector size (e.g., 256 instead of 1536) for 4x-12x storage savings. Batch up to 2048 inputs per request (8192 tokens/input max). Embeddings are deterministic -- cache them.

---

## Images API

### Image Generation (DALL-E 3)

**Endpoint**: `POST /v1/images/generations`

```typescript
const image = await openai.images.generate({
  model: 'dall-e-3',
  prompt: 'A white siamese cat with striking blue eyes',
  size: '1024x1024', // Also: 1024x1536, 1536x1024, 1024x1792, 1792x1024
  quality: 'standard', // or 'hd'
  style: 'vivid', // or 'natural'
});

console.log(image.data[0].url);
console.log(image.data[0].revised_prompt); // DALL-E 3 may revise for safety
```

**DALL-E 3 Specifics**:
- Only supports `n: 1` (one image per request)
- May revise prompts for safety/quality (check `revised_prompt`)
- URLs expire in 1 hour (use `response_format: 'b64_json'` for persistence)

### Image Editing (GPT-Image-1)

**Endpoint**: `POST /v1/images/edits` (uses `multipart/form-data`, not JSON)

Supports transparency (PNG/WebP), compositing with `image_2`, and output compression control. Send image files via FormData.

---

## Audio API

### Whisper Transcription

**Endpoint**: `POST /v1/audio/transcriptions`

```typescript
const transcription = await openai.audio.transcriptions.create({
  file: fs.createReadStream('./audio.mp3'),
  model: 'whisper-1',
});
// Returns: { text: "Transcribed text..." }
```

**Formats**: mp3, mp4, mpeg, mpga, m4a, wav, webm

### Text-to-Speech (TTS)

**Endpoint**: `POST /v1/audio/speech`

**Models**:
- **tts-1**: Standard quality, lowest latency
- **tts-1-hd**: High definition audio
- **gpt-4o-mini-tts**: Supports voice instructions (November 2024), streaming

**11 Voices**: alloy, ash, ballad, coral, echo, fable, onyx, nova, sage, shimmer, verse

```typescript
const mp3 = await openai.audio.speech.create({
  model: 'tts-1',
  voice: 'alloy',
  input: 'Text to speak (max 4096 chars)',
  speed: 1.0, // 0.25-4.0
  response_format: 'mp3', // mp3|opus|aac|flac|wav|pcm
});
```

**gpt-4o-mini-tts** additionally supports `instructions` (custom voice control) and `stream_format: "sse"` for streaming audio.

---

## Moderation API

**Endpoint**: `POST /v1/moderations`

Check content across 11 safety categories.

```typescript
const moderation = await openai.moderations.create({
  model: 'omni-moderation-latest',
  input: 'Text to moderate',
});

console.log(moderation.results[0].flagged);
console.log(moderation.results[0].categories);
console.log(moderation.results[0].category_scores); // 0.0-1.0
```

**11 categories**: sexual, hate, harassment, self-harm, violence (plus sub-categories like sexual/minors, hate/threatening, etc.). Scores range 0.0-1.0. Supports batch input as arrays. Use lower thresholds for severe categories.

---

## Error Handling & Rate Limits

### Common Errors

- **401**: Invalid API key
- **429**: Rate limit exceeded (implement exponential backoff)
- **500/503**: Server errors (retry with backoff)

```typescript
async function completionWithRetry(params, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await openai.chat.completions.create(params);
    } catch (error) {
      if (error.status === 429 && i < maxRetries - 1) {
        await new Promise(resolve => setTimeout(resolve, Math.pow(2, i) * 1000));
        continue;
      }
      throw error;
    }
  }
}
```

Check `x-ratelimit-remaining-requests` header. Limits vary by tier/model (RPM, TPM, IPM).

**Production tips**: Never expose API keys client-side. Stream responses >100 tokens. Use `reasoning_effort: 'none'` for simple tasks to reduce cost.

---

## When to Use openai-api vs openai-responses

| Need | openai-api (this skill) | openai-responses |
|---|---|---|
| Simple chat, one-off generation | Yes | No |
| Embeddings, images, audio, moderation | Yes | No |
| Agentic multi-turn with built-in tools | No | Yes |
| Stateful conversation management | No | Yes |
| Cloudflare Workers / edge deployment | Yes | No |

Many apps use both: openai-api for embeddings/images/audio and openai-responses for conversational agents.
