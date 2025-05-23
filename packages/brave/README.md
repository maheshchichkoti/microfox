# Brave SDK

TypeScript SDK for Brave Search APIs.

## Installation

```bash
npm install @microfox/brave
```

## Environment Variables

To use this package, you need to set the following environment variables:

- `BRAVE_API_KEY`: Your Brave Search API key. Obtain this by subscribing to a plan (including the free plan) at https://brave.com/search/api/. **(Required)**

## Quick Start

```typescript
import { createBraveSDK } from '@microfox/brave';

// Initialize with API key
const braveSDK = createBraveSDK({
  apiKey: process.env.BRAVE_API_KEY,
});

// Or use environment variable
const braveSDK = createBraveSDK(); // Uses BRAVE_API_KEY from environment
```

## Available Features

The SDK provides access to the following Brave Search API endpoints:

- Web Search
- Local POI Search (Pro plan required)
- Local Descriptions Search (Pro plan required)
- Summarizer Search (Pro AI plan required)
- Image Search
- Video Search
- News Search
- Suggest Search
- Spellcheck Search

## Rate Limits

The SDK automatically handles rate limiting based on your subscription plan:

- Free Plan: 1 request per second
- Pro Plan: 20 requests per second

The SDK includes built-in rate limiting protection through sequential processing. When making multiple requests, use sequential processing instead of `Promise.all` to avoid hitting rate limits:

```typescript
// ❌ Don't do this
const results = await Promise.all([
  braveSDK.webSearch({ q: 'query1' }),
  braveSDK.webSearch({ q: 'query2' }),
]);

// ✅ Do this instead
const results = [];
for (const query of ['query1', 'query2']) {
  results.push(await braveSDK.webSearch({ q: query }));
  await new Promise(resolve => setTimeout(resolve, 1000)); // 1 second delay
}
```

## API Reference

For detailed documentation on all available functions and their parameters, please refer to the following files:

- [request](./docs/request.md)
- [webSearch](./docs/webSearch.md)
- [localPoiSearch](./docs/localPoiSearch.md)
- [localDescriptionsSearch](./docs/localDescriptionsSearch.md)
- [summarizerSearch](./docs/summarizerSearch.md)
- [imageSearch](./docs/imageSearch.md)
- [videoSearch](./docs/videoSearch.md)
- [newsSearch](./docs/newsSearch.md)
- [suggestSearch](./docs/suggestSearch.md)
- [spellcheckSearch](./docs/spellcheckSearch.md)
- [createBraveSDK](./docs/createBraveSDK.md)

## Best Practices

1. Always use sequential processing for multiple requests to respect rate limits
2. Configure appropriate headers for your use case
3. Handle API errors appropriately
4. Use the appropriate subscription plan for your needs
5. Consider caching responses when appropriate
6. Monitor rate limit headers in responses for quota management
