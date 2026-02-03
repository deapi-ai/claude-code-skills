---
name: generate-image
description: Generate images from text prompts using Flux or ZImageTurbo models
argument-hint: <prompt> [--model flux|turbo] [--size 512|768|1024]
---

# Image Generation via deAPI

Generate image from prompt: **$ARGUMENTS**

## Step 1: Parse arguments

Extract from `$ARGUMENTS`:
- `prompt`: The text description (required)
- `--model`: `flux` (default, high quality) or `turbo` (faster)
- `--size`: `512`, `768`, or `1024` (default: 1024)

## Step 2: Send request

```bash
curl -s -X POST "https://api.deapi.ai/api/v1/client/txt2img" \
  -H "Authorization: Bearer $DEAPI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "{prompt}",
    "model": "{model_name}",
    "width": {size},
    "height": {size},
    "steps": {steps},
    "guidance_scale": 7.5,
    "seed": {random_seed}
  }'
```

**Model mapping:**
| User flag | API model name | Steps | Speed |
|-----------|----------------|-------|-------|
| `flux` (default) | `Flux1schnell` | 4-10 (default: 4) | ~15s |
| `turbo` | `ZImageTurbo_INT8` | 4-10 (default: 4) | ~8s |

**Important:** Generate a random seed (0-999999) for each request.

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
1. Get image URL from `result_url`
2. Display/download the generated image
3. Show the image to the user

## Step 5: Offer follow-up

Ask user:
- "Would you like variations with a modified prompt?"
- "Should I upscale this image?"
- "Would you like to try a different style?"

## Error handling

| Error | Action |
|-------|--------|
| 401 Unauthorized | Check if `$DEAPI_API_KEY` is set correctly |
| 429 Rate Limited | Wait 60s and retry |
| 500 Server Error | Wait 30s and retry once |
| Empty prompt | Ask user to provide a description |
| NSFW rejected | Inform user, suggest alternative prompt |
