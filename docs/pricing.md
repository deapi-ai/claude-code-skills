# deAPI Pricing

## Overview

deAPI offers 10-20x cheaper inference compared to proprietary providers through decentralized GPU infrastructure.

## Free Tier

- **Initial credits:** $5 on signup
- **Rate limit:** 1-10 requests per minute
- **Daily limits:** Apply per endpoint
- **No credit card required**

## Premium Tier

- **Rate limit:** 300 requests per minute
- **Daily limits:** Unlimited
- **Priority queue:** Faster processing
- **Pay-as-you-go:** No minimum commitment

---

## Pricing by Endpoint

### Text-to-Image (`txt2img`)

| Resolution | Steps | Price |
|------------|-------|-------|
| 512x512 | 4 | $0.00136 |
| 512x512 | 20 | $0.0068 |
| 768x768 | 20 | $0.015 |
| 1024x1024 | 20 | $0.027 |
| 1024x1024 | 50 | $0.068 |

*Flux model pricing. Zimageturbo is ~30% cheaper.*

### Image-to-Image (`img2img`)

| Resolution | Price |
|------------|-------|
| 512x512 | $0.008 |
| 768x768 | $0.018 |
| 1024x1024 | $0.032 |

### Text-to-Speech (`txt2audio`)

| Character Count | Price |
|-----------------|-------|
| Per 1,000 chars | $0.015 |

*~750 words per 1,000 characters*

### Transcription (`vid2txt`, `aud2txt`)

| Duration | Price |
|----------|-------|
| Per minute | $0.006 |

*WhisperLargeV3 pricing*

### OCR (`img2txt`)

| Per Image | Price |
|-----------|-------|
| Standard | $0.002 |

### Background Removal (`img-rmbg`)

| Per Image | Price |
|-----------|-------|
| Standard | $0.005 |

### Image Upscaling (`img-upscale`)

| Scale | Price |
|-------|-------|
| 2x | $0.008 |
| 4x | $0.016 |

### Video Generation (`txt2video`, `img2video`)

| Duration | Price |
|----------|-------|
| Per second | $0.10 |

*4-second video = $0.40*

### Embeddings (`txt2embedding`)

| Model | Price per 1M tokens |
|-------|---------------------|
| bge-large | $0.10 |
| bge-base | $0.08 |
| bge-small | $0.05 |

---

## Cost Comparison

### vs. OpenAI

| Service | deAPI | OpenAI | Savings |
|---------|-------|--------|---------|
| Whisper transcription | $0.006/min | $0.006/min | Similar |
| Text embeddings | $0.10/1M | $0.13/1M | 23% |

### vs. Stability AI

| Service | deAPI | Stability | Savings |
|---------|-------|-----------|---------|
| Image gen (1024x1024) | $0.027 | $0.04 | 33% |

### vs. ElevenLabs

| Service | deAPI | ElevenLabs | Savings |
|---------|-------|------------|---------|
| TTS (1K chars) | $0.015 | $0.30 | 95% |

### vs. RunwayML

| Service | deAPI | Runway | Savings |
|---------|-------|--------|---------|
| Video gen (4s) | $0.40 | $0.50 | 20% |

---

## Billing

- **Minimum charge:** None
- **Billing cycle:** Monthly
- **Payment methods:** Credit card, crypto
- **Invoice:** Available on request

## Cost Optimization Tips

1. **Use smaller resolutions** for drafts, upscale final images
2. **Batch requests** when possible
3. **Use zimageturbo** for quick iterations
4. **Use bge-small** for high-volume embeddings
5. **Trim audio/video** before transcription
6. **Cache results** - URLs expire but content can be stored

---

## Usage Monitoring

Check your usage:
```bash
curl -s "https://api.deapi.ai/api/v1/client/usage" \
  -H "Authorization: Bearer $DEAPI_API_KEY"
```

Response:
```json
{
  "credits_remaining": 4.25,
  "credits_used_today": 0.75,
  "requests_today": 42,
  "plan": "free"
}
```
