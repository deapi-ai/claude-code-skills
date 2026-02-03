# deAPI Endpoint Test Results

**Date:** 2026-02-03
**Balance before tests:** 524.68 credits

---

## Test Results Summary

| # | Endpoint | Model | Status | Request ID |
|---|----------|-------|--------|------------|
| 1 | GET /balance | - | **PASS** | - |
| 2 | POST /txt2embedding | `Bge_M3_FP16` | **PASS** | de6564d3-8c3d-4d32-8542-1c030a8b7201 |
| 3 | POST /txt2img | `Flux1schnell` | **PASS** | 0c8b9393-ea69-4928-9a69-0991f417e888 |
| 4 | POST /txt2audio | `Kokoro` | **PASS** | b27a51c5-ce0a-4f91-bc4c-8c401a6466d7 |
| 5 | POST /vid2txt | `WhisperLargeV3` | **PASS** | fb0ae170-8fe9-4bab-9f35-efc289c1cf5c |
| 6 | POST /img2txt | `Nanonets_Ocr_S_F16` | **PASS** | ac8474b7-c704-4cd8-894c-81f3d9117b8d |
| 7 | POST /img-rmbg | `Ben2` | **PASS** | 9cb1bf08-cdba-456a-bb48-75833e59dc60 |
| 8 | POST /img-upscale | `RealESRGAN_x4` | **PASS** | 15f413e1-d2bb-4bd8-99ef-f61ce3c9d943 |
| 9 | POST /img2img | `QwenImageEdit_Plus_NF4` | **PASS** | 83710b4c-48d3-4952-823d-d47df7ecf331 |
| 10 | POST /txt2video | - | **SKIPPED** | (expensive) |
| 11 | GET /request-status | - | **PASS** | - |

**Result: 10/10 tested endpoints working (1 skipped)**

---

## Model Name Corrections

During testing, discovered discrepancies between documented model names and actual API values:

| Endpoint | Documented Model | Actual Working Model |
|----------|-----------------|---------------------|
| /img-rmbg | `RMBG-1.4` | `Ben2` |
| /img-upscale | `RealESRGAN_x4plus` | `RealESRGAN_x4` |
| /vid2txt | (optional) | `WhisperLargeV3` (required) |

---

## Verified Working Parameters

### /txt2img
```json
{
  "prompt": "string",
  "model": "Flux1schnell",
  "width": 512,
  "height": 512,
  "guidance": 7.5,
  "steps": 4,
  "seed": 12345
}
```

### /txt2audio
```json
{
  "text": "string",
  "model": "Kokoro",
  "voice": "af_bella",
  "lang": "en-us",
  "speed": 1.0,
  "format": "mp3",
  "sample_rate": 24000
}
```

### /vid2txt
```json
{
  "video_url": "https://youtube.com/...",
  "model": "WhisperLargeV3",
  "include_ts": false
}
```

### /txt2embedding
```json
{
  "input": "string",
  "model": "Bge_M3_FP16"
}
```

### /img2img (multipart/form-data)
- `image`: file
- `prompt`: string
- `model`: "QwenImageEdit_Plus_NF4"
- `guidance`: 7.5
- `steps`: 20
- `seed`: 12345

### /img2txt (multipart/form-data)
- `image`: file
- `model`: "Nanonets_Ocr_S_F16"

### /img-rmbg (multipart/form-data)
- `image`: file
- `model`: "Ben2"

### /img-upscale (multipart/form-data)
- `image`: file
- `model`: "RealESRGAN_x4"

---

## Request Status Response

```json
{
  "data": {
    "status": "done",
    "preview": null,
    "result_url": "https://...",
    "result": null,
    "progress": 100
  }
}
```

Status values: `processing`, `done`, `failed`

---

## Notes

1. All endpoints require `Authorization: Bearer $DEAPI_API_KEY` header
2. Image endpoints use `multipart/form-data`, others use `application/json`
3. All POST endpoints return `{"data": {"request_id": "uuid"}}`
4. Poll `/request-status/{id}` until `status` = `done`
5. Result available at `result_url` when done
