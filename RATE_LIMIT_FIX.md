# Rate Limiting Fix for Embedding API Calls

## Problem

The system was experiencing HTTP 429 (Too Many Requests) errors from the Mistral AI API when generating embeddings for MCP server tools during startup. This caused:

1. Frequent fallback to simple embedding method (100 dimensions)
2. Vector dimension mismatches in the database (switching between 256, 100, and other dimensions)
3. Repeated clearing and reconfiguration of vector data
4. Poor user experience with many error messages in logs

## Root Cause

The `vectorSearchService.ts` was making multiple concurrent embedding API requests without any rate limiting or retry logic, causing the Mistral API (configured in `mcp_settings.json`) to reject requests with HTTP 429 errors.

## Solution Implemented

### 1. **Request Queue with Rate Limiting**
- Added a queue system that ensures requests are made sequentially with a 2-second delay between them
- Prevents overwhelming the API with concurrent requests
- Processes requests in order without losing any

### 2. **Exponential Backoff Retry Logic**
- Implements automatic retry for 429 errors (up to 3 attempts)
- Uses exponential backoff: 2s → 4s → 8s delays between retries
- Only falls back to simple embedding after all retries are exhausted

### 3. **Better Error Handling**
- Reduced log verbosity for rate limit errors
- Only logs detailed errors on final failure
- Shows clear retry progress messages

### 4. **Server-Level Batching**
- Added 1-second delays between processing different servers
- Tracks success/failure counts for better visibility
- Continues processing even if individual tools fail

### 5. **Mistral API Support**
- Added explicit support for Mistral's `mistral-embed` model (256 dimensions)
- Properly detects and handles dimension requirements

## Configuration

The system uses configuration from `mcp_settings.json`:

```json
{
  "systemConfig": {
    "smartRouting": {
      "enabled": true,
      "openaiApiBaseUrl": "https://api.mistral.ai/v1/",
      "openaiApiKey": "YOUR_KEY",
      "openaiApiEmbeddingModel": "mistral-embed"
    }
  }
}
```

## Rate Limiting Parameters

Configurable constants in `vectorSearchService.ts`:

- `RATE_LIMIT_DELAY`: 2000ms (2 seconds between requests)
- `MAX_RETRIES`: 3 (maximum retry attempts)
- `RETRY_BACKOFF_MULTIPLIER`: 2 (exponential backoff factor)

## Expected Behavior After Fix

1. **Startup**: Embeddings will be generated sequentially with proper delays
2. **Rate Limits**: System will automatically retry with backoff instead of immediately failing
3. **Logs**: Cleaner log output with progress indicators
4. **Dimension Stability**: Consistent vector dimensions (256 for mistral-embed)
5. **Fallback**: Only used after all retries exhausted, not on first error

## Testing

1. Build the application: `pnpm backend:build`
2. Start the server: `pnpm dev`
3. Monitor logs for:
   - "Rate limit hit (429), retrying in Xms" messages
   - "Saved X tool embeddings for server: Y" success messages
   - Absence of frequent "Falling back to simple embedding method" warnings

## Performance Impact

- **Slower initial sync**: Yes, but controlled and predictable
- **Better reliability**: Significantly improved with retry logic
- **API costs**: Reduced due to fewer failed requests
- **Database stability**: No more dimension mismatch issues

## Future Improvements

1. Make rate limit parameters configurable via environment variables
2. Implement adaptive rate limiting based on API response headers
3. Add batch embedding support for APIs that support it
4. Cache embeddings to avoid regenerating for unchanged tools
