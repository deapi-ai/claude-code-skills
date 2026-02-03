# deAPI Models Reference

## Image Generation Models

### Flux1schnell
- **API name:** `Flux1schnell`
- **Endpoint:** `txt2img`
- **Quality:** High
- **Speed:** ~15 seconds
- **Steps:** 4-10 (default: 4)
- **Best for:** Professional quality images, detailed scenes, photorealism
- **Max resolution:** 1024x1024
- **Requires:** `seed` parameter (random 0-999999)

### ZImageTurbo_INT8
- **API name:** `ZImageTurbo_INT8`
- **Endpoint:** `txt2img`
- **Quality:** Good
- **Speed:** ~8 seconds
- **Steps:** 4-10 (default: 4)
- **Best for:** Quick iterations, drafts, concept exploration
- **Max resolution:** 1024x1024
- **Requires:** `seed` parameter (random 0-999999)

### QwenImageEdit_Plus_NF4
- **API name:** `QwenImageEdit_Plus_NF4`
- **Endpoint:** `img2img`
- **Quality:** High
- **Best for:** Style transfer, image transformation, editing
- **Features:** Strength control for transformation intensity

---

## Transcription Models

### WhisperLargeV3
- **API name:** `WhisperLargeV3`
- **Endpoint:** `vid2txt`, `aud2txt`
- **Quality:** Highest accuracy
- **Languages:** 100+ languages with auto-detection
- **Features:**
  - Timestamp generation (`include_ts: true`)
  - Speaker diarization (multi-speaker)
  - Punctuation restoration
- **Best for:** Professional transcription, multi-language content

---

## Text-to-Speech Models

### Kokoro
- **API name:** `Kokoro`
- **Endpoint:** `txt2audio`
- **Quality:** Natural, human-like
- **Output:** WAV/MP3
- **Features:**
  - Multiple voices
  - Emotion control
  - Speed adjustment

### Available Voices

| Voice ID | Gender | Accent | Style |
|----------|--------|--------|-------|
| `af_bella` | Female | American | Warm, friendly |
| `af_nicole` | Female | American | Professional |
| `af_sarah` | Female | American | Casual |
| `af_sky` | Female | American | Young, energetic |
| `am_adam` | Male | American | Deep, authoritative |
| `am_michael` | Male | American | Energetic |
| `bf_emma` | Female | British | Elegant |
| `bf_isabella` | Female | British | Refined |
| `bm_george` | Male | British | Sophisticated |
| `bm_lewis` | Male | British | Warm |

---

## Video Generation Models

### Ltxv_13B_0_9_8_Distilled_FP8
- **API name:** `Ltxv_13B_0_9_8_Distilled_FP8`
- **Endpoint:** `txt2video`, `img2video`
- **Quality:** High
- **Duration:** Up to 10 seconds
- **Resolution:** Up to 720p
- **FPS:** 24
- **Best for:** Short clips, animations, social content

---

## Embedding Models

### Bge_M3_FP16
- **API name:** `Bge_M3_FP16`
- **Endpoint:** `txt2embedding`
- **Dimensions:** 1024
- **Quality:** Highest accuracy
- **Best for:** Semantic search, RAG applications, multilingual
- **Max tokens:** 8192
- **Features:** Multilingual support

---

## OCR Model

### Nanonets_Ocr_S_F16
- **API name:** `Nanonets_Ocr_S_F16`
- **Endpoint:** `img2txt`
- **Languages:** Multi-language support
- **Features:**
  - Printed text
  - Handwriting (limited)
  - Document layout preservation
- **Best for:** Documents, screenshots, signs

---

## Upscaling Model

### Real-ESRGAN
- **Endpoint:** `img-upscale`
- **Scale factors:** 2x, 4x
- **Quality:** Excellent detail preservation
- **Best for:** Enhancing AI-generated images, photo restoration

---

## Background Removal Model

### Ben2
- **API name:** `Ben2`
- **Endpoint:** `img-rmbg`
- **Output:** PNG with transparency
- **Quality:** Professional edge detection
- **Best for:** Product photos, portraits, object isolation

---

## Model Selection Quick Reference

| Task | API Model Name | User-friendly |
|------|----------------|---------------|
| High-quality images | `Flux1schnell` | flux |
| Quick image drafts | `ZImageTurbo_INT8` | turbo |
| Image editing | `QwenImageEdit_Plus_NF4` | - |
| Transcription | `WhisperLargeV3` | - |
| Text-to-speech | `Kokoro` | - |
| Video generation | `Ltxv_13B_0_9_8_Distilled_FP8` | - |
| Semantic search | `Bge_M3_FP16` | - |
| OCR | `Nanonets_Ocr_S_F16` | - |
| Background removal | `Ben2` | - |
