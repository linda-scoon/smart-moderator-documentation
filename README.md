# Smart Moderator Documentation

Documentation website for **Smart Moderator v2.0.0** - an AI-powered WordPress plugin that automatically moderates posts and comments using OpenAI GPT or Anthropic Claude APIs.

## About Smart Moderator

Smart Moderator is a WordPress plugin that:

- Automatically reviews posts and comments before publication
- Uses AI (OpenAI GPT or Anthropic Claude) to detect spam and harmful content
- **NEW in v2.0:** Supports GPT-5 models with automatic reasoning optimization
- Supports custom moderation prompts at global, per-post, and per-comment levels
- Provides enforcement mechanisms to prevent manual overrides of AI decisions
- Maintains full moderation logs for audit trails (ordered by most recent first)

## Project Structure

```
smart-moderator-documentation/
└── docs/
    └── index.html    # Main documentation website
```

This is a static HTML documentation site with embedded CSS - no build tools or dependencies required.

## Viewing the Documentation

### Local Development

Simply open `docs/index.html` in a web browser:

```bash
# macOS
open docs/index.html

# Linux
xdg-open docs/index.html

# Windows
start docs/index.html
```

### Deployment

The site can be deployed to any static hosting platform:

- **GitHub Pages** - Serve directly from the `docs/` directory
- **Netlify/Vercel** - Point to the repository root
- **Traditional web server** - Upload `docs/index.html` to your web root

## Documentation Contents

The documentation covers:

- **Features** - Key plugin capabilities
- **Installation** - Requirements and setup steps
- **Setup Guides** - Step-by-step configuration for Anthropic and OpenAI
- **Pricing** - AI provider comparison and cost estimates
- **FAQ** - Common questions and troubleshooting
- **Privacy & Security** - Data handling and security practices
- **Compatibility** - WordPress, WooCommerce, multisite support

## Requirements (Plugin)

- WordPress 6.0+ (tested up to 6.8)
- PHP 8.0+ (required)
- Outbound HTTPS connections
- API key from OpenAI or Anthropic

## Version 2.0.0 - Breaking Changes

**IMPORTANT:** Smart Moderator v2.0.0 now explicitly supports only OpenAI and Anthropic Claude APIs. The plugin is no longer API-agnostic. This change enables better optimization, removes 114 lines of code, and eliminates token limit issues.

## Supported AI Providers (2025)

### OpenAI Models
| Model | Cost (per 1M input tokens) | Notes |
|-------|---------------------------|-------|
| gpt-4o-mini | $0.15 | Fastest, most economical ✅ |
| gpt-4o | $2.50 | Balanced, recommended ✅ |
| gpt-5-nano | $0.05 input / $0.40 output | **NEW:** Ultra-fast reasoning |
| gpt-5-mini | Variable | **NEW:** Mid-tier reasoning |
| gpt-5 | Variable | **NEW:** Advanced reasoning |
| o1-mini | Variable | Reasoning model |

**Technical Note:** No token limits enforced for OpenAI. GPT-5 models automatically use `reasoning_effort: "low"` for efficient moderation.

### Claude Models
| Model | Cost (per 1M input tokens) | Notes |
|-------|---------------------------|-------|
| claude-haiku-4-5 | $1 | Fastest ✅ |
| claude-sonnet-4-20250514 | $3 | Balanced, recommended ✅ |
| claude-opus-4-5 | $15 | Maximum intelligence |
| claude-sonnet-3.5 | $3 | Previous generation |

**Technical Note:** Uses Claude's Messages API with 1024 token limit (API requirement).

## License

GPLv2 or later

## Author

Linda Scoon

---

Copyright 2025 Linda Scoon. All rights reserved.
