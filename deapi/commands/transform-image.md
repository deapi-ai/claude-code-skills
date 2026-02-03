---
name: transform-image
description: Transform images with style transfer and AI modifications
argument-hint: <image-url> <style-prompt>
---

# Image Transformation via deAPI

Transform image: **$ARGUMENTS**

## Step 1: Parse arguments

Extract from `$ARGUMENTS`:
- `image_url`: URL to the source image (required)
- `style_prompt`: Description of desired transformation (required)

**Example style prompts:**
- "convert to watercolor painting"
- "make it look like a vintage photograph"
- "transform into anime style"
- "add cyberpunk neon aesthetic"
- "make it look like oil painting"

## Step 2: Send request

```bash
curl -s -X POST "https://api.deapi.ai/api/v1/client/img2img" \
  -H "Authorization: Bearer $DEAPI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "image_url": "{image_url}",
    "prompt": "{style_prompt}",
    "strength": 0.7,
    "guidance_scale": 7.5,
    "model": "QwenImageEdit_Plus_NF4"
  }'
```

**Strength parameter:**
| Value | Effect |
|-------|--------|
| 0.3-0.5 | Subtle changes, preserves original |
| 0.5-0.7 | Balanced transformation (default) |
| 0.7-0.9 | Strong style application |

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
1. Get transformed image URL from `result_url`
2. Display before/after comparison if possible
3. Provide download link to user

## Step 5: Offer follow-up

Ask user:
- "Would you like to try a different style?"
- "Should I adjust the transformation strength?"
- "Would you like to upscale the result?"

## Error handling

| Error | Action |
|-------|--------|
| 401 Unauthorized | Check if `$DEAPI_API_KEY` is set correctly |
| 429 Rate Limited | Wait 60s and retry |
| 500 Server Error | Wait 30s and retry once |
| Invalid URL | Ask user to verify the image URL |
| Missing prompt | Ask user to describe desired transformation |
| NSFW rejected | Inform user, suggest alternative style |
