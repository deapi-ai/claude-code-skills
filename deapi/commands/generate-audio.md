---
name: generate-audio
description: Convert text to speech using various AI voices
argument-hint: <text> [--voice af_bella|am_adam|bf_emma|bm_george]
---

# Text-to-Speech via deAPI

Generate audio from text: **$ARGUMENTS**

## Step 1: Parse arguments

Extract from `$ARGUMENTS`:
- `text`: The text to convert to speech (required)
- `--voice`: Voice ID (default: `af_bella`)

**Available voices:**
| Voice ID | Description |
|----------|-------------|
| `af_bella` | American Female - Bella (warm, friendly) |
| `af_nicole` | American Female - Nicole (professional) |
| `af_sarah` | American Female - Sarah (casual) |
| `am_adam` | American Male - Adam (deep, authoritative) |
| `am_michael` | American Male - Michael (energetic) |
| `bf_emma` | British Female - Emma (elegant) |
| `bf_isabella` | British Female - Isabella (refined) |
| `bm_george` | British Male - George (sophisticated) |
| `bm_lewis` | British Male - Lewis (warm) |

## Step 2: Send request

```bash
curl -s -X POST "https://api.deapi.ai/api/v1/client/txt2audio" \
  -H "Authorization: Bearer $DEAPI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "text": "{text}",
    "voice": "{voice}",
    "model": "Kokoro"
  }'
```

## Step 3: Poll status (feedback loop)

Extract `request_id` from response, then poll every 10 seconds:

```bash
curl -s "https://api.deapi.ai/api/v1/client/request-status/{request_id}" \
  -H "Authorization: Bearer $DEAPI_API_KEY"
```

**Status handling:**
- `processing` → wait 10s, poll again
- `done` → proceed to Step 4
- `failed` → report error message to user, STOP

## Step 4: Fetch and present result

When `status = "done"`:
1. Get audio URL from `result_url`
2. Provide download link to user
3. Show audio duration if available

## Step 5: Offer follow-up

Ask user:
- "Would you like to try a different voice?"
- "Should I generate this in another language?"
- "Would you like to adjust the text?"

## Error handling

| Error | Action |
|-------|--------|
| 401 Unauthorized | Check if `$DEAPI_API_KEY` is set correctly |
| 429 Rate Limited | Wait 60s and retry |
| 500 Server Error | Wait 30s and retry once |
| Empty text | Ask user to provide text to convert |
| Invalid voice | Show available voices, ask user to choose |
| Text too long | Suggest splitting into smaller segments |
