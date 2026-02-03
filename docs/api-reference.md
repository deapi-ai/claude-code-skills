# deAPI API Reference

## Base URL

```
https://api.deapi.ai/api/v1/client/
```

## Authentication

All endpoints require Bearer token authentication:

```
Authorization: Bearer $DEAPI_API_KEY
```

---

## Endpoints

### Text-to-Image (`txt2img`)

Generate images from text prompts.

**Request:**
```bash
POST /txt2img
Content-Type: application/json

{
  "prompt": "a sunset over mountains, photorealistic",
  "model": "Flux1schnell",
  "width": 1024,
  "height": 1024,
  "steps": 4,
  "guidance_scale": 7.5,
  "seed": 123456,
  "negative_prompt": "blurry, low quality"
}
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `prompt` | string | Yes | - | Image description |
| `model` | string | Yes | - | `Flux1schnell` or `ZImageTurbo_INT8` |
| `width` | int | No | 1024 | Image width (512-1024) |
| `height` | int | No | 1024 | Image height (512-1024) |
| `steps` | int | No | 4 | Inference steps (4-10) |
| `guidance_scale` | float | No | 7.5 | Prompt adherence (1-20) |
| `seed` | int | **Yes** | - | Random seed (0-999999) |
| `negative_prompt` | string | No | - | What to avoid |

---

### Image-to-Image (`img2img`)

Transform existing images with style prompts.

**Request:**
```bash
POST /img2img
Content-Type: application/json

{
  "image_url": "https://example.com/photo.jpg",
  "prompt": "convert to watercolor painting",
  "strength": 0.7,
  "guidance_scale": 7.5,
  "model": "QwenImageEdit_Plus_NF4"
}
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `image_url` | string | Yes | - | Source image URL |
| `prompt` | string | Yes | - | Transformation description |
| `strength` | float | No | 0.7 | Change intensity (0.1-1.0) |
| `guidance_scale` | float | No | 7.5 | Prompt adherence |
| `model` | string | Yes | - | `QwenImageEdit_Plus_NF4` |

---

### Text-to-Audio (`txt2audio`)

Convert text to speech.

**Request:**
```bash
POST /txt2audio
Content-Type: application/json

{
  "text": "Hello, welcome to our service.",
  "voice": "af_bella",
  "model": "Kokoro"
}
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `text` | string | Yes | - | Text to convert |
| `voice` | string | No | `af_bella` | Voice ID |
| `model` | string | Yes | - | `Kokoro` |

---

### Video Transcription (`vid2txt`)

Transcribe video content.

**Request:**
```bash
POST /vid2txt
Content-Type: application/json

{
  "video_url": "https://youtube.com/watch?v=xxx",
  "include_ts": true,
  "model": "WhisperLargeV3"
}
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `video_url` | string | Yes | - | Video URL (YouTube, direct) |
| `include_ts` | bool | No | false | Include timestamps |
| `model` | string | No | `WhisperLargeV3` | Whisper model |

---

### Audio Transcription (`aud2txt`)

Transcribe audio files.

**Request:**
```bash
POST /aud2txt
Content-Type: application/json

{
  "audio_url": "https://example.com/audio.mp3",
  "include_ts": true,
  "model": "WhisperLargeV3"
}
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `audio_url` | string | Yes | - | Audio URL |
| `include_ts` | bool | No | false | Include timestamps |
| `model` | string | No | `WhisperLargeV3` | Whisper model |

---

### OCR (`img2txt`)

Extract text from images.

**Request:**
```bash
POST /img2txt
Content-Type: application/json

{
  "image_url": "https://example.com/document.png",
  "model": "Nanonets_Ocr_S_F16"
}
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `image_url` | string | Yes | - | Image URL |
| `model` | string | Yes | - | `Nanonets_Ocr_S_F16` |

---

### Background Removal (`img-rmbg`)

Remove image backgrounds.

**Request:**
```bash
POST /img-rmbg
Content-Type: application/json

{
  "image_url": "https://example.com/photo.jpg",
  "model": "Ben2"
}
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `image_url` | string | Yes | - | Image URL |
| `model` | string | Yes | - | `Ben2` |

**Output:** PNG with transparent background

---

### Image Upscaling (`img-upscale`)

Increase image resolution.

**Request:**
```bash
POST /img-upscale
Content-Type: application/json

{
  "image_url": "https://example.com/photo.jpg",
  "scale": 2
}
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `image_url` | string | Yes | - | Image URL |
| `scale` | int | No | 2 | Upscale factor (2 or 4) |

---

### Text-to-Video (`txt2video`)

Generate videos from text.

**Request:**
```bash
POST /txt2video
Content-Type: application/json

{
  "prompt": "a cat walking through a garden",
  "duration": 4,
  "fps": 24,
  "model": "Ltxv_13B_0_9_8_Distilled_FP8"
}
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `prompt` | string | Yes | - | Video description |
| `duration` | int | No | 4 | Duration in seconds |
| `fps` | int | No | 24 | Frames per second |
| `model` | string | Yes | - | `Ltxv_13B_0_9_8_Distilled_FP8` |

---

### Image-to-Video (`img2video`)

Animate static images.

**Request:**
```bash
POST /img2video
Content-Type: application/json

{
  "image_url": "https://example.com/photo.jpg",
  "motion_prompt": "gentle movement, cinematic",
  "duration": 4,
  "fps": 24
}
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `image_url` | string | Yes | - | Source image URL |
| `motion_prompt` | string | No | - | Motion description |
| `duration` | int | No | 4 | Duration in seconds |
| `fps` | int | No | 24 | Frames per second |

---

### Text Embeddings (`txt2embedding`)

Generate vector embeddings.

**Request:**
```bash
POST /txt2embedding
Content-Type: application/json

{
  "text": "Your text to embed",
  "model": "Bge_M3_FP16"
}
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `text` | string | Yes | - | Text to embed |
| `model` | string | Yes | - | `Bge_M3_FP16` |

---

### Request Status (`request-status`)

Check job status.

**Request:**
```bash
GET /request-status/{request_id}
```

**Response:**
```json
{
  "request_id": "abc123",
  "status": "done",
  "result_url": "https://...",
  "created_at": "2024-01-15T10:30:00Z",
  "completed_at": "2024-01-15T10:30:45Z"
}
```

**Status values:**
- `processing` - Job in progress
- `done` - Job completed successfully
- `failed` - Job failed (check `error` field)

---

## Result Delivery Methods

| Method | Best for | Setup |
|--------|----------|-------|
| Polling | CLI, scripts | Default, no config needed |
| Webhooks | Backend, serverless | Add `webhook_url` to requests |
| WebSockets | Real-time UI | Pusher connection |

See [webhooks docs](https://docs.deapi.ai/execution-modes-and-integrations/webhooks)
and [websockets docs](https://docs.deapi.ai/execution-modes-and-integrations/websockets).
