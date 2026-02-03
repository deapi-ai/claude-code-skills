# Skills Project

## Purpose
Building skills and commands for Claude Code using deAPI.

## deAPI Overview

Unified REST API for running open-source AI models on a decentralized GPU network.

### Key Features
- **10-20x cheaper** inference vs proprietary providers
- Free tier + $5 initial credits
- Base endpoint: `https://api.deapi.ai`
- Auth: Bearer token (`DEAPI_API_KEY` in `.env`)

### Supported Tasks
- Text-to-image
- Image-to-image
- Text-to-speech (TTS)
- OCR
- Video transcription
- Embeddings
- Background removal

### Result Delivery
1. **Webhooks** (recommended) - auto HTTP POST with retries
2. **WebSockets** - real-time via Pusher
3. **Polling** - simple GET status checks

### Pricing
- Free tier: 1-10 RPM, daily limits
- Premium: 300 RPM unlimited
- Text-to-image: from $0.00136/image (512×512, 4 steps)

### Integrations
- Native MCP server for Claude/Cursor
- n8n automation
- REST API (15+ endpoints)

## Environment
```
DEAPI_API_KEY=<key in .env>
```

## Project Structure

```
claude-code-skills/
├── CLAUDE.md                 # This file
├── README.md
├── deapi/                    # Skill folder (copy to ~/.claude/skills/)
│   ├── SKILL.md              # Main skill with commands table
│   └── commands/             # Individual commands
│       ├── transcribe.md
│       ├── generate-image.md
│       ├── generate-audio.md
│       ├── generate-video.md
│       ├── ocr.md
│       ├── remove-bg.md
│       ├── upscale.md
│       ├── transform-image.md
│       ├── embed.md
│       └── deapi-setup.md
└── docs/
    └── api-reference.md      # Full deAPI documentation
```

## Skill Conventions

### Frontmatter
```yaml
---
name: command-name
description: One-line description
argument-hint: <required> [optional]  # only if command takes arguments
---
```

### Command Structure
1. **Step 1-N** - sequential execution steps
2. **Error handling** - table with errors and actions
3. Use `$ARGUMENTS` for user arguments
4. Use `$DEAPI_API_KEY` for API key

### API Pattern (all endpoints)
1. POST request → returns `request_id`
2. Poll `/request-status/{id}` every 10s
3. When `status = "done"` → fetch from `result_url`

## API Reference (Source of Truth)

**OpenAPI spec:** `docs/client-api-docs.json` - pełna specyfikacja API w formacie OpenAPI 3.0

Użyj tego pliku do weryfikacji parametrów endpointów.

## Key Links
- Webhooks: https://docs.deapi.ai/execution-modes-and-integrations/webhooks
- WebSockets: https://docs.deapi.ai/execution-modes-and-integrations/websockets
- API Base: `https://api.deapi.ai/api/v1/client/`
