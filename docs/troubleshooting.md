# deAPI Troubleshooting Guide

## Common Issues

### Authentication Errors

#### 401 Unauthorized

**Symptoms:**
```json
{"error": "Unauthorized", "message": "Invalid or missing API key"}
```

**Solutions:**
1. Verify API key is set:
   ```bash
   echo $DEAPI_API_KEY
   ```
2. Check for typos or extra whitespace
3. Regenerate API key at https://deapi.ai/dashboard
4. Ensure key hasn't expired

#### 403 Forbidden

**Symptoms:**
```json
{"error": "Forbidden", "message": "Insufficient permissions"}
```

**Solutions:**
1. Check your plan limits
2. Verify endpoint access on your plan
3. Contact support if issue persists

---

### Rate Limiting

#### 429 Too Many Requests

**Symptoms:**
```json
{"error": "Rate limit exceeded", "retry_after": 60}
```

**Solutions:**
1. Wait for `retry_after` seconds
2. Implement exponential backoff
3. Reduce request frequency
4. Upgrade to premium for higher limits

**Best practice:**
```bash
# Implement backoff
sleep_time=1
max_retries=5
for i in $(seq 1 $max_retries); do
  response=$(curl -s ...)
  if [[ $response != *"429"* ]]; then break; fi
  sleep $sleep_time
  sleep_time=$((sleep_time * 2))
done
```

---

### Job Processing Issues

#### Job Stuck in "processing"

**Symptoms:**
- Status remains `processing` for extended time
- No progress after 5+ minutes

**Solutions:**
1. Check expected processing times:
   - Image generation: 10-60 seconds
   - Video generation: 1-5 minutes
   - Transcription: ~1x media duration
2. Cancel and retry if exceeds 2x expected time
3. Try with smaller input (lower resolution, shorter media)

#### Job Failed

**Symptoms:**
```json
{"status": "failed", "error": "Processing error"}
```

**Solutions:**
1. Check input validity (URL accessible, correct format)
2. Verify input size limits
3. Review error message for specific issues
4. Retry once after 30 seconds
5. Try alternative parameters

---

### Input Issues

#### Invalid URL

**Symptoms:**
```json
{"error": "Invalid URL", "message": "Could not fetch resource"}
```

**Solutions:**
1. Verify URL is publicly accessible (not behind auth)
2. Check URL format (https://, no spaces)
3. Test URL in browser
4. For YouTube: ensure video is public
5. Use direct file URLs when possible

#### Unsupported Format

**Symptoms:**
```json
{"error": "Unsupported format"}
```

**Supported formats:**
| Type | Formats |
|------|---------|
| Image | PNG, JPG, JPEG, WebP, GIF, BMP |
| Audio | MP3, WAV, M4A, FLAC, OGG |
| Video | MP4, WebM, MOV, AVI |

**Solutions:**
1. Convert to supported format
2. Use online converter if needed
3. Check file isn't corrupted

#### Content Too Large

**Symptoms:**
```json
{"error": "Content too large", "max_size": "50MB"}
```

**Solutions:**
1. Compress image/video
2. Reduce resolution
3. Split audio/video into segments
4. Use streaming URL if available

---

### Output Issues

#### Result URL Expired

**Symptoms:**
- `result_url` returns 404 or access denied
- Link worked before but now fails

**Solutions:**
1. Download results promptly (URLs expire in 24h)
2. Store results locally after fetching
3. Re-run job if URL expired

#### Poor Quality Output

**Image generation:**
1. Add more detail to prompt
2. Increase `steps` (20-50)
3. Adjust `guidance_scale` (7-12)
4. Use `flux` instead of `zimageturbo`
5. Add negative prompt

**Transcription:**
1. Ensure clear audio
2. Reduce background noise
3. Use `WhisperLargeV3` model

**TTS:**
1. Add punctuation for natural pauses
2. Try different voice
3. Break long text into segments

---

### Network Issues

#### Timeout

**Symptoms:**
- Connection timeout
- Request hangs

**Solutions:**
1. Check internet connection
2. Verify deAPI status: https://status.deapi.ai
3. Increase client timeout settings
4. Try from different network

#### SSL/TLS Errors

**Symptoms:**
```
SSL certificate problem: unable to get local issuer certificate
```

**Solutions:**
1. Update CA certificates
2. Check system time is correct
3. Use `-k` flag for testing (not production):
   ```bash
   curl -k -s ...
   ```

---

## Debugging Tips

### Enable Verbose Output

```bash
curl -v -X POST "https://api.deapi.ai/api/v1/client/txt2img" \
  -H "Authorization: Bearer $DEAPI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"prompt": "test"}'
```

### Check Request/Response

```bash
# Save full response
curl -s -w "\n%{http_code}" -o response.json \
  "https://api.deapi.ai/api/v1/client/request-status/{id}" \
  -H "Authorization: Bearer $DEAPI_API_KEY"
```

### Validate JSON

```bash
echo '{"prompt": "test"}' | jq .
```

---

## Getting Help

1. **Documentation:** https://docs.deapi.ai
2. **Status page:** https://status.deapi.ai
3. **Discord:** https://discord.gg/deapi
4. **Email:** support@deapi.ai

When reporting issues, include:
- Request ID (if available)
- Endpoint used
- Input parameters (redact sensitive data)
- Full error response
- Timestamp
