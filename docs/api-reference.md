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
  "guidance": 7.5,
  "seed": 123456,
  "negative_prompt": "blurry, low quality"
}
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `prompt` | string | Yes | - | Image description |
| `model` | string | Yes | - | `Flux_2_Klein_4B_BF16`, `Flux1schnell`, or `ZImageTurbo_INT8` |
| `width` | int | Yes | 1024 | Image width (256-2048, depends on model) |
| `height` | int | Yes | 1024 | Image height (256-2048, depends on model) |
| `steps` | int | Yes | 4 | Inference steps (model-dependent) |
| `guidance` | float | No | 7.5 | Prompt adherence (not supported by Klein) |
| `seed` | int | Yes | - | Random seed (0-999999) |
| `negative_prompt` | string | No | - | What to avoid (not supported by Klein) |

**Models:**
| Model | Slug | Steps | Max size | Guidance | Negative prompt |
|-------|------|-------|----------|----------|-----------------|
| FLUX.2 Klein 4B | `Flux_2_Klein_4B_BF16` | 4 (fixed) | 1536 (step: 16) | No | No |
| Flux.1 Schnell | `Flux1schnell` | 1-10 (default: 4) | 2048 (step: 128) | No | Yes |
| Z-Image-Turbo | `ZImageTurbo_INT8` | 1-50 (default: 8) | 2048 (step: 16) | No | Yes |

---

### Image-to-Image (`img2img`)

Transform existing images with style prompts.

**Request:**
```bash
POST /img2img
Content-Type: multipart/form-data

-F "image=@photo.jpg"
-F "prompt=convert to watercolor painting"
-F "model=Flux_2_Klein_4B_BF16"
-F "steps=4"
-F "seed=123456"
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `image` | file | Yes | - | Source image file(s) (max 10MB). Klein supports up to 3 images |
| `prompt` | string | Yes | - | Transformation description |
| `model` | string | Yes | - | `Flux_2_Klein_4B_BF16` or `QwenImageEdit_Plus_NF4` |
| `guidance` | float | No | 7.5 | Prompt adherence (Klein: not supported, Qwen: 1-20) |
| `steps` | int | Yes | - | Inference steps (Klein: 4 fixed, Qwen: 1-50 default 40) |
| `seed` | int | Yes | - | Random seed (0-999999) |
| `width` | int | No | - | Output width (Klein only, 256-1536, step: 16) |
| `height` | int | No | - | Output height (Klein only, 256-1536, step: 16) |

**Models:**
| Model | Slug | Steps | Max images | Guidance | Custom output size |
|-------|------|-------|------------|----------|--------------------|
| FLUX.2 Klein 4B | `Flux_2_Klein_4B_BF16` | 4 (fixed) | 3 | No | Yes (256-1536) |
| Qwen Image Edit Plus | `QwenImageEdit_Plus_NF4` | 1-50 (default: 40) | 1 | Yes | No |

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
  "model": "Kokoro",
  "lang": "en-us",
  "speed": 1.0,
  "format": "mp3",
  "sample_rate": 24000
}
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `text` | string | Yes | - | Text to convert |
| `voice` | string | Yes | `af_bella` | Voice ID (54+ available) |
| `model` | string | Yes | - | `Kokoro` |
| `lang` | string | Yes | - | Language code: `en-us`, `en-gb`, `ja`, `zh`, etc. |
| `speed` | float | Yes | 1.0 | Speech speed (0.5-2.0) |
| `format` | string | Yes | `mp3` | Output: `mp3`, `wav`, `flac`, `ogg` |
| `sample_rate` | int | Yes | 24000 | Sample rate: `22050`, `24000`, `44100`, `48000` |

---

### Video Transcription (`vid2txt`)

Transcribe video content from URL.

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
| `video_url` | string | Yes | - | Video URL (YouTube, Twitch, Kick, X) |
| `include_ts` | bool | Yes | false | Include timestamps |
| `model` | string | No | `WhisperLargeV3` | Whisper model |

---

### Audio Transcription (`aud2txt`)

Transcribe audio from URL (X Spaces, etc.).

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
| `include_ts` | bool | Yes | false | Include timestamps |
| `model` | string | No | `WhisperLargeV3` | Whisper model |

---

### OCR (`img2txt`)

Extract text from images.

**Request:**
```bash
POST /img2txt
Content-Type: multipart/form-data

-F "image=@document.png"
-F "model=Nanonets_Ocr_S_F16"
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `image` | file | Yes | - | Image file (max 10MB) |
| `model` | string | Yes | - | `Nanonets_Ocr_S_F16` |

---

### Background Removal (`img-rmbg`)

Remove image backgrounds.

**Request:**
```bash
POST /img-rmbg
Content-Type: multipart/form-data

-F "image=@photo.jpg"
-F "model=Ben2"
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `image` | file | Yes | - | Image file (max 10MB) |
| `model` | string | Yes | - | `Ben2` |

**Output:** PNG with transparent background

---

### Image Upscaling (`img-upscale`)

Increase image resolution.

**Request:**
```bash
POST /img-upscale
Content-Type: multipart/form-data

-F "image=@photo.jpg"
-F "model=RealESRGAN_x4"
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `image` | file | Yes | - | Image file |
| `model` | string | Yes | - | `RealESRGAN_x2` (2x) or `RealESRGAN_x4` (4x) |

---

### Text-to-Video (`txt2video`)

Generate videos from text.

**Request:**
```bash
POST /txt2video
Content-Type: multipart/form-data

-F "prompt=a cat walking through a garden"
-F "model=Ltxv_13B_0_9_8_Distilled_FP8"
-F "width=768"
-F "height=512"
-F "guidance=7.5"
-F "steps=30"
-F "frames=120"
-F "seed=123456"
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `prompt` | string | Yes | - | Video description |
| `model` | string | Yes | - | `Ltxv_13B_0_9_8_Distilled_FP8` |
| `width` | int | Yes | 768 | Video width |
| `height` | int | Yes | 512 | Video height |
| `guidance` | float | Yes | 7.5 | Prompt adherence |
| `steps` | int | Yes | 30 | Inference steps |
| `frames` | int | Yes | 120 | Number of frames |
| `seed` | int | Yes | - | Random seed |
| `fps` | int | No | 24 | Frames per second |

---

### Image-to-Video (`img2video`)

Animate static images.

**Request:**
```bash
POST /img2video
Content-Type: multipart/form-data

-F "first_frame_image=@photo.jpg"
-F "prompt=gentle movement, cinematic"
-F "model=..."
-F "width=768"
-F "height=512"
-F "guidance=7.5"
-F "steps=30"
-F "frames=120"
-F "seed=123456"
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `first_frame_image` | file | Yes | - | Source image file |
| `prompt` | string | Yes | - | Motion description |
| `model` | string | Yes | - | Model name |
| `width` | int | Yes | - | Video width |
| `height` | int | Yes | - | Video height |
| `guidance` | float | Yes | 7.5 | Prompt adherence |
| `steps` | int | Yes | 30 | Inference steps |
| `frames` | int | Yes | 120 | Number of frames |
| `seed` | int | Yes | - | Random seed |

---

### Text Embeddings (`txt2embedding`)

Generate vector embeddings.

**Request:**
```bash
POST /txt2embedding
Content-Type: application/json

{
  "input": "Your text to embed",
  "model": "Bge_M3_FP16"
}
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `input` | string/array | Yes | - | Text to embed (single or array) |
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
