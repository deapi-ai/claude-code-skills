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
| `model` | string | Yes | - | `Flux1schnell` or `ZImageTurbo_INT8` |
| `width` | int | Yes | 1024 | Image width (512-1024) |
| `height` | int | Yes | 1024 | Image height (512-1024) |
| `steps` | int | Yes | 4 | Inference steps (4-10) |
| `guidance` | float | Yes | 7.5 | Prompt adherence (1-20) |
| `seed` | int | Yes | - | Random seed (0-999999) |
| `negative_prompt` | string | No | - | What to avoid |

---

### Image-to-Image (`img2img`)

Transform existing images with style prompts.

**Request:**
```bash
POST /img2img
Content-Type: multipart/form-data

-F "image=@photo.jpg"
-F "prompt=convert to watercolor painting"
-F "model=QwenImageEdit_Plus_NF4"
-F "guidance=7.5"
-F "steps=20"
-F "seed=123456"
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `image` | file | Yes | - | Source image file (max 10MB) |
| `prompt` | string | Yes | - | Transformation description |
| `model` | string | Yes | - | `QwenImageEdit_Plus_NF4` |
| `guidance` | float | Yes | 7.5 | Prompt adherence (1-20) |
| `steps` | int | Yes | 20 | Inference steps |
| `seed` | int | Yes | - | Random seed (0-999999) |

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
-F "model=RMBG-1.4"
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `image` | file | Yes | - | Image file (max 10MB) |
| `model` | string | Yes | - | `RMBG-1.4` |

**Output:** PNG with transparent background

---

### Image Upscaling (`img-upscale`)

Increase image resolution.

**Request:**
```bash
POST /img-upscale
Content-Type: multipart/form-data

-F "image=@photo.jpg"
-F "model=RealESRGAN_x4plus"
```

**Parameters:**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `image` | file | Yes | - | Image file |
| `model` | string | Yes | - | `RealESRGAN_x2plus` (2x) or `RealESRGAN_x4plus` (4x) |

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
