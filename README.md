# Smart Moderator Documentation

Documentation website for **Smart Moderator** - an AI-powered WordPress plugin that automatically moderates posts and comments using OpenAI GPT or Anthropic Claude APIs.

## About Smart Moderator

Smart Moderator is a WordPress plugin that:

- Automatically reviews posts and comments before publication
- Uses AI (OpenAI GPT or Anthropic Claude) to detect spam and harmful content
- Supports custom moderation prompts at global, per-post, and per-comment levels
- Provides enforcement mechanisms to prevent manual overrides of AI decisions
- Maintains full moderation logs for audit trails

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
- PHP 8.0+
- Outbound HTTPS connections
- API key from OpenAI or Anthropic

## Supported AI Providers

| Provider | Model | Cost (per 1M tokens) |
|----------|-------|---------------------|
| Anthropic | claude-haiku-4-5 | $1 |
| Anthropic | claude-sonnet-4 | $3 |
| OpenAI | gpt-4o-mini | $0.15 |
| OpenAI | gpt-4o | $2.50 |

## License

GPLv2 or later

## Author

Linda Scoon

---

Copyright 2025 Linda Scoon. All rights reserved.
