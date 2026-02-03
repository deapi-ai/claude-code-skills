# deAPI Skills - AI Media Generation for Claude Code

> AI media generation skill for Claude Code - transcribe YouTube, generate images,
> text-to-speech, OCR and more. Claude automatically suggests the right command.
> Also works with Cursor, Windsurf & Continue.dev. **10-20x cheaper** inference.
> Open source Claude Code plugin by Anthropic community.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Table of Contents

- [Why deAPI Skills?](#why-deapi-skills)
- [Quick Start](#quick-start)
- [Installation](#installation)
- [Available Commands](#available-commands)
- [Use Cases](#use-cases)
- [Compatibility](#compatibility)
- [Pricing](#pricing)
- [Documentation](#documentation)
- [License](#license)

## Why deAPI Skills?

**Problem:** AI media generation APIs are expensive and complex to integrate into your CLI workflow.

**Solution:** Simple slash commands that work directly in Claude Code, Cursor IDE, and other AI coding assistants.

| Feature | deAPI Skills | Traditional APIs |
|---------|--------------|------------------|
| Setup time | 30 seconds | Hours |
| Cost | $0.027/image | $0.20-0.50/image |
| YouTube transcription | Built-in | Requires multiple APIs |
| Integration | Slash commands | Custom code needed |

## Quick Start

**1. Get your API key** from [deapi.ai](https://deapi.ai) (free $5 credit)

**2. Set environment variable:**
```bash
export DEAPI_API_KEY=your_key_here
```

**3. Install skill:**
```bash
git clone https://github.com/deapi/deapi-skills.git
cp -r deapi-skills/skills/deapi ~/.claude/skills/
```

**4. Try it:**
```
/transcribe https://youtube.com/watch?v=dQw4w9WgXcQ
```

**Done!** Claude now automatically knows about deAPI commands.
Try: "transcribe this YouTube video" - Claude will suggest `/transcribe`.

## Installation

### Skill vs Commands - What's the difference?

| Approach | Location | What happens |
|----------|----------|--------------|
| **Skill** (recommended) | `~/.claude/skills/` | Claude knows about all deAPI commands and suggests them automatically when relevant |
| **Commands only** | `~/.claude/commands/` | Only specific commands you install, no auto-suggestions |

**Example:** With skill installed, you say "transcribe this YouTube video" and Claude automatically knows to use `/transcribe`. With commands only, you must explicitly type `/transcribe`.

### How it works

Claude Code automatically discovers skills in `~/.claude/skills/`. No configuration needed - just copy the folder and start chatting.

```
~/.claude/skills/
└── deapi/
    ├── SKILL.md              ← Skill metadata + command routing
    └── commands/             ← Full command documentation (loaded on-demand)
        ├── transcribe.md
        ├── generate-image.md
        └── ...
```

### Prerequisites

1. Get your API key from [deapi.ai](https://deapi.ai)
2. Set environment variable:
   ```bash
   # Add to ~/.bashrc or ~/.zshrc
   export DEAPI_API_KEY=your_key_here
   ```

### Full Skill Installation (Recommended)

```bash
git clone https://github.com/deapi/deapi-skills.git
cp -r deapi-skills/skills/deapi ~/.claude/skills/
```

### Individual Command Installation

Install only specific commands if you prefer:

```bash
# Create commands directory if needed
mkdir -p ~/.claude/commands

# Just transcription
cp deapi-skills/skills/deapi/commands/transcribe.md ~/.claude/commands/

# Just image generation
cp deapi-skills/skills/deapi/commands/generate-image.md ~/.claude/commands/
```

## Available Commands

| Command | Description | Example |
|---------|-------------|---------|
| `/transcribe` | Transcribe YouTube, audio, video | `/transcribe https://youtube.com/watch?v=...` |
| `/generate-image` | AI image generation from text | `/generate-image a sunset over mountains` |
| `/generate-audio` | Text-to-speech conversion | `/generate-audio "Hello world" --voice am_adam` |
| `/generate-video` | Text/image to video | `/generate-video a cat walking through garden` |
| `/ocr` | Extract text from images | `/ocr https://example.com/document.png` |
| `/remove-bg` | Remove image background | `/remove-bg https://example.com/photo.jpg` |
| `/upscale` | Upscale image resolution | `/upscale https://example.com/small.jpg --scale 4` |
| `/transform-image` | Style transfer | `/transform-image https://... watercolor style` |
| `/embed` | Generate text embeddings | `/embed "text to embed"` |
| `/setup-delivery` | Configure webhooks/websockets | `/setup-delivery` |

> **Note:** When you install the full skill, Claude automatically suggests these
> commands based on context. You don't need to memorize them.

## Use Cases

### Transcribe YouTube videos in Claude Code
Use `/transcribe` to convert any YouTube video to text with timestamps.
Perfect for creating notes, summaries, or searchable content from video.

### Generate AI images in Claude Code
Use `/generate-image` with Flux model for high-quality image generation.
10x cheaper than DALL-E or Midjourney API.

### Text-to-speech in Claude Code
Use `/generate-audio` for natural voice synthesis with 50+ voices.
Great for creating podcasts, audiobooks, or voice-overs.

### Extract text from images (OCR)
Use `/ocr` to extract text from screenshots, documents, or photos.

### Configure webhooks for production apps
Use `/setup-delivery` to set up webhooks or websockets for server-side integration.

## Compatibility

deAPI Skills work with multiple AI coding assistants:

### Claude Code (Primary)

```bash
# Full skill installation (recommended)
cp -r deapi-skills/skills/deapi ~/.claude/skills/

# Or just commands
cp deapi-skills/skills/deapi/commands/*.md ~/.claude/commands/
```

### Cursor IDE

```bash
mkdir -p .cursor/commands
cp deapi-skills/skills/deapi/commands/*.md .cursor/commands/
```

### Windsurf

```bash
mkdir -p .windsurf/workflows
cp deapi-skills/skills/deapi/commands/*.md .windsurf/workflows/
```

### Continue.dev

Continue.dev uses YAML configuration. Add to your `config.yaml`:

```yaml
customCommands:
  - name: transcribe
    description: Transcribe video/audio
    prompt: |
      # Include content from commands/transcribe.md
```

See [Continue.dev docs](https://continue.dev/docs) for full setup.

### Platform Support Matrix

| Platform | Status | Format | Location |
|----------|--------|--------|----------|
| Claude Code | Fully supported | Markdown | `~/.claude/skills/` |
| Cursor IDE | Compatible | Markdown | `.cursor/commands/` |
| Windsurf | Compatible | Markdown | `.windsurf/workflows/` |
| Continue.dev | Requires adaptation | YAML+MD | `config.yaml` |
| Cody | Requires conversion | JSON | `.vscode/cody.json` |

## Pricing

deAPI offers **10-20x cheaper** inference than competitors:

| Service | deAPI Price | OpenAI/Replicate |
|---------|-------------|------------------|
| Image (1024x1024) | $0.027 | $0.20-0.50 |
| TTS (1K chars) | $0.015 | $0.03-0.06 |
| Transcription | $0.006/min | $0.006/min |
| Video (per second) | $0.10 | $0.50+ |
| Embeddings | $0.10/1M tokens | $0.13/1M tokens |

**Free tier:** $5 credit on signup, no credit card required.

## Documentation

- [API Reference](docs/api-reference.md)
- [Models Reference](docs/models.md)
- [Pricing Details](docs/pricing.md)
- [Troubleshooting](docs/troubleshooting.md)

## Repository Structure

```
deapi/
├── skills/
│   └── deapi/
│       ├── SKILL.md              ← Skill metadata + routing
│       └── commands/             ← Command documentation (on-demand)
│           ├── transcribe.md
│           ├── generate-image.md
│           ├── generate-audio.md
│           ├── generate-video.md
│           ├── ocr.md
│           ├── remove-bg.md
│           ├── upscale.md
│           ├── transform-image.md
│           ├── embed.md
│           └── setup-delivery.md
├── docs/
│   ├── api-reference.md
│   ├── models.md
│   ├── pricing.md
│   └── troubleshooting.md
├── README.md
└── CLAUDE.md
```

## Requirements

- Claude Code CLI, Cursor IDE, Windsurf, or Continue.dev
- deAPI account with API key
- Internet connection

## Contributing

Contributions welcome! Please read our contributing guidelines before submitting PRs.

## License

MIT License - see [LICENSE](LICENSE)

## Links

- [deAPI Website](https://deapi.ai)
- [deAPI Documentation](https://docs.deapi.ai)
- [deAPI Status](https://status.deapi.ai)
