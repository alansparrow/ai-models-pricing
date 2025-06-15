# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a data repository that maintains AI model pricing information for multiple providers in a structured format. Each provider has its own directory containing pricing data, allowing clients to fetch only the data they need.

## Directory Structure

```
ai-models-pricing/
├── claude/
│   └── pricing-data.json
├── openai/
│   └── pricing-data.json
├── grok/
│   └── pricing-data.json
└── ... (other providers)
```

## Data Structure

Each provider directory contains a `pricing-data.json` file with the following structure:

```json
{
  "metadata": {
    "id": "unique-provider-identifier",  // Unique identifier for the provider
    "provider": "Provider Name",
    "providerUrl": "https://provider-website.com",
    "apiEndpoint": "https://api.provider.com",  // Optional: API base URL
    "source": "URL to official pricing documentation",
    "lastUpdated": "ISO 8601 timestamp",  // When the pricing was last retrieved/updated
    "version": "semantic version",
    "description": "Description of the pricing data",
    "currency": "USD",
    "unit": "per token (or other unit)",
    "notes": "Additional notes about pricing or features"  // Optional
  },
  "models": [
    {
      "modelId": "model-unique-id",
      "name": "Model Display Name",
      "input": 0.0,  // Cost per input unit
      "output": 0.0, // Cost per output unit
      "cache": {     // Optional cache pricing
        "5m": { "write": 0.0, "read": 0.0 },
        "1h": { "write": 0.0, "read": 0.0 }
      },
      "originalRates": {  // Original pricing format from source
        "input": "$X/unit",
        "output": "$Y/unit"
      }
    }
  ]
}
```

## Working with the Data

### Adding a New Provider

1. Create a new directory with the provider name (lowercase, no spaces)
2. Create `pricing-data.json` inside the directory
3. Fill in all metadata fields including:
   - Unique `id` for the provider
   - Provider name and URLs
   - Source documentation
4. List all available models with their pricing

Example:
```bash
mkdir openai
# Create openai/pricing-data.json with the structure above
```

### Updating Pricing Data

When updating pricing information:

1. Navigate to the specific provider directory
2. Verify the source from official provider documentation
3. Update the `metadata.lastUpdated` to the current ISO timestamp
5. Maintain both computed per-unit values and original rate strings
6. Ensure all monetary values are in the specified currency
7. Keep the existing data structure intact for backwards compatibility

### Data Validation

Before committing any changes:

- Verify JSON syntax is valid in each `pricing-data.json`
- Check that all required metadata fields are present:
  - `id`: Unique provider identifier
  - `provider`: Official provider name
  - `providerUrl`: Provider's website
  - `source`: Documentation URL
  - `lastUpdated`: ISO timestamp of last update
  - `currency` and `unit`
- Verify numerical values are positive
- Validate model IDs are unique within each provider

## Repository Conventions

- Each provider gets its own directory (lowercase naming)
- The `id` field should be a unique identifier for the provider
- All pricing data files must be named `pricing-data.json`
- Keep the structure simple and focused on data maintainability
- Document any data source changes in commit messages
- Preserve the existing JSON formatting style
- Provider directories should be named consistently (e.g., `openai`, not `OpenAI` or `open-ai`)

## Client Usage

Clients can fetch data for specific providers:
- Claude pricing: `/claude/pricing-data.json`
- OpenAI pricing: `/openai/pricing-data.json`
- Grok pricing: `/grok/pricing-data.json`

This structure allows efficient data retrieval without loading unnecessary provider information.