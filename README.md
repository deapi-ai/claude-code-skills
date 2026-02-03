# deAPI Skills - AI Media Generation for Claude Code

> **Skill for Claude Code** that adds AI media generation capabilities.
> Install once, then just describe what you want - transcribe videos, generate images,
> text-to-speech, OCR. Claude automatically picks the right tool.
> Also works with Cursor, Windsurf & Continue.dev. Up to 20x cheaper inference via [deAPI](https://deapi.ai).
> **Free $5 credit** on signup, no credit card required.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Table of Contents

- [Why deAPI Skills?](#why-deapi-skills)
- [Quick Start](#quick-start)
- [Use Cases](#use-cases)
- [Command Reference](#command-reference)
- [Installation](#installation)
- [Compatibility](#compatibility)
- [Pricing](#pricing)
- [Documentation](#documentation)
- [License](#license)

## Why deAPI Skills?

**Install once, just talk to Claude.**

After installation, Claude automatically knows how to:
- Transcribe YouTube videos
- Generate images from text
- Convert text to speech
- Extract text from images (OCR)
- And more...

Just describe what you want in natural language. Claude picks the right tool.
You can also use commands directly if you prefer.

**Bonus:** Up to 20x cheaper than big clouds like OpenAI.

## Quick Start

**1. Get your API key** from [deapi.ai](https://deapi.ai) (free $5 credit)

**2. Set environment variable:**
```bash
export DEAPI_API_KEY=your_key_here
```

**3. Install skill:**
```bash
git clone https://github.com/deapi-ai/claude-code-skills.git
cp -r claude-code-skills/deapi ~/.claude/skills/
```

**4. Restart Claude Code** (new session or reopen terminal)

**5. Try it:**
> "Transcribe this YouTube video: https://youtube.com/watch?v=dQw4w9WgXcQ"

Or use command directly: `/transcribe https://youtube.com/watch?v=...`

**Done!** Claude automatically picks the right tool based on your request.

## Use Cases

### Transcribe YouTube videos

Just tell Claude what you want:
> "Transcribe this YouTube video and summarize key points"

Claude automatically uses `/transcribe` and processes the result.

### Generate AI images

> "Create a hero image for my landing page - minimalist, tech startup vibe"

Claude generates the image using Flux model via `/generate-image`.

### Text-to-speech / AI voice

> "Read this blog post aloud with a professional male voice"

Claude converts text to natural speech with `/generate-audio`.

### Extract text from images (OCR)

> "Extract all text from this screenshot"

Claude uses `/ocr` to pull text from any image.

### Generate video from text

> "Create a short video of a cat walking through a garden"

Claude generates video with `/generate-video`.

### Configure webhooks for production

> "Set up webhooks for my server to receive results"

Claude configures delivery with `/deapi-setup`.

## Command Reference

> You don't need to memorize these. Claude automatically picks the right command
> based on your request. This is just for reference.

| Command | What it does | Example |
|---------|--------------|---------|
| `/transcribe` | Transcribe YouTube, audio, video | `/transcribe https://youtube.com/watch?v=...` |
| `/generate-image` | AI image generation from text | `/generate-image a sunset over mountains` |
| `/generate-audio` | Text-to-speech conversion | `/generate-audio "Hello world" --voice am_adam` |
| `/generate-video` | Text/image to video | `/generate-video a cat walking through garden` |
| `/ocr` | Extract text from images | `/ocr https://example.com/document.png` |
| `/remove-bg` | Remove image background | `/remove-bg https://example.com/photo.jpg` |
| `/upscale` | Upscale image resolution | `/upscale https://example.com/small.jpg --scale 4` |
| `/transform-image` | Style transfer | `/transform-image https://... watercolor style` |
| `/embed` | Generate text embeddings | `/embed "text to embed"` |
| `/deapi-setup` | Configure webhooks/websockets | `/deapi-setup` |

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
git clone https://github.com/deapi-ai/claude-code-skills.git
cp -r claude-code-skills/deapi ~/.claude/skills/
```

Then restart Claude Code (start a new session or reopen terminal).

### Individual Command Installation

Install only specific commands if you prefer:

```bash
# Create commands directory if needed
mkdir -p ~/.claude/commands

# Just transcription
cp claude-code-skills/deapi/commands/transcribe.md ~/.claude/commands/

# Just image generation
cp claude-code-skills/deapi/commands/generate-image.md ~/.claude/commands/
```

Then restart Claude Code to load the new commands.

## Compatibility

deAPI Skills work with multiple AI coding assistants:

### Claude Code (Primary)

```bash
# Full skill installation (recommended)
cp -r claude-code-skills/deapi ~/.claude/skills/

# Or just commands
cp claude-code-skills/deapi/commands/*.md ~/.claude/commands/
```

### Cursor IDE

```bash
mkdir -p .cursor/commands
cp claude-code-skills/deapi/commands/*.md .cursor/commands/
```

### Windsurf

```bash
mkdir -p .windsurf/workflows
cp claude-code-skills/deapi/commands/*.md .windsurf/workflows/
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

### Platform Support Matrix

| Platform | Status | Format | Location |
|----------|--------|--------|----------|
| Claude Code | Fully supported | Markdown | `~/.claude/skills/` |
| Cursor IDE | Compatible | Markdown | `.cursor/commands/` |
| Windsurf | Compatible | Markdown | `.windsurf/workflows/` |
| Continue.dev | Requires adaptation | YAML+MD | `config.yaml` |
| Cody | Requires conversion | JSON | `.vscode/cody.json` |

## Pricing

**Up to 20x more cost effective** than proprietary APIs.

Example prices:
- Transcription: **$0.021/hour**
- Image generation: from **$0.002/image**

Free $5 credit on signup, no credit card required.
→ [deapi.ai](https://deapi.ai)

## Documentation

- [API Reference](docs/api-reference.md)
- [Models Reference](docs/models.md)
- [Pricing Details](docs/pricing.md)
- [Troubleshooting](docs/troubleshooting.md)

## Repository Structure

```
claude-code-skills/
├── deapi/                        ← Copy this to ~/.claude/skills/
│   ├── SKILL.md                  ← Skill metadata + routing
│   └── commands/                 ← Command documentation (on-demand)
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
