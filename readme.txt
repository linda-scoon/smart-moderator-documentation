=== Smart Moderator (AI-Powered) ===
Contributors: scoonlinda
Tags: spam filter, comment moderation, ai, content moderation, antispam
Requires at least: 6.0
Tested up to: 6.8
Requires PHP: 8.0
Stable tag: 2.0.0
License: GPLv2 or later
License URI: https://www.gnu.org/licenses/gpl-2.0.html

AI-powered automatic moderation for posts and comments using OpenAI or Anthropic Claude. Stops spam and harmful content before it's published.

== Description ==

Smart Moderator automatically reviews new posts and comments using AI (OpenAI GPT or Anthropic Claude) to prevent spam, abuse, and harmful content before publication.

### 🧠 Key Features

* **AI-driven moderation** – content is reviewed by your configured AI model via secure API
* **Custom prompts** – define global moderation rules and add context for posts or comments separately
* **Immediate moderation** – content is approved or rejected as soon as it's saved
* **Enforce moderation result** – when enabled, re-checks and re-applies AI decision after 60 seconds to prevent overrides from other plugins
* **Full logs** – all moderation decisions are stored per post/comment for transparency
* **Secure storage** – API keys kept in database, never logged, transmitted only via HTTPS
* **Compatible** – works with WordPress comment moderation settings and other plugins

### ⚙️ How It Works

1. User submits a post or comment
2. Smart Moderator sends it to your AI endpoint (OpenAI, Anthropic, or custom)
3. AI returns "approve" or "reject" with optional reason
4. Plugin updates post/comment status:
   * Approved → published (if permissions allow)
   * Rejected → held as pending/unapproved
5. If **"Enforce moderation result"** is enabled, plugin re-checks after **60 seconds** to ensure no other plugin or user overrode the decision

### 🔐 Privacy and Data Handling

* API key stored locally in WordPress database
* Logs include only decision outcomes, reasons, and timestamps - not full content
* Smart Moderator makes requests only to your configured endpoint over HTTPS
* No user tracking or telemetry
* For posts: sends title + content
* For comments: sends comment text only (not commenter name/email)

### 💰 Supported AI Providers

**Anthropic Claude** (Recommended)
* Models: claude-haiku-4-5, claude-sonnet-4, claude-opus-4-1
* Cost: $1-$15 per million input tokens
* Free tier available
* Get API key: https://console.anthropic.com

**OpenAI GPT**
* Models: gpt-4o-mini, gpt-4o, gpt-4-turbo
* Cost: $0.15-$30 per million input tokens
* Free trial available (very limited)
* Get API key: https://platform.openai.com/api-keys

**Note:** As of version 2.0.0, Smart Moderator explicitly supports OpenAI and Anthropic Claude only. Custom endpoint support has been removed for improved reliability and maintainability.

== Installation ==

### Quick Install

1. Upload plugin to `/wp-content/plugins/` directory
2. Activate through **Plugins → Installed Plugins**
3. Go to **Tools → Smart Moderator**
4. Follow setup guide below for your chosen AI provider

### Setup for Anthropic Claude

**Step 1: Get API Key**
1. Visit https://console.anthropic.com
2. Sign up or sign in
3. Go to API Keys section
4. Create new key (name it "WordPress")
5. Copy the key immediately (starts with `sk-ant-`)

**Step 2: Configure Plugin**
1. Go to **Tools → Smart Moderator** in WordPress
2. Enter these exact settings:
   * **API Key:** Paste your full key (100+ characters)
   * **API Endpoint URL:** `https://api.anthropic.com/v1/messages`
   * **Auth Header Name:** `x-api-key`
   * **Auth Header Prefix:** Leave empty (do NOT type "Bearer")
   * **Extra Headers:** `{"anthropic-version":"2023-06-01"}`
   * **Model:** `claude-sonnet-4-20250514` (recommended)
3. Click **Save & Test Connection**
4. Look for green success message

**Recommended Models:**
* `claude-haiku-4-5` - Fastest, cheapest ($1/M tokens)
* `claude-sonnet-4-20250514` - Balanced ($3/M tokens) ✅
* `claude-opus-4-1-20250805` - Most powerful ($15/M tokens)

### Setup for OpenAI GPT

**Step 1: Get API Key**
1. Visit https://platform.openai.com
2. Sign up or sign in
3. Go to API Keys section
4. Create new secret key
5. Copy immediately (starts with `sk-` or `sk-proj-`)
6. Add billing info and credits to your account

**Step 2: Configure Plugin**
1. Go to **Tools → Smart Moderator**
2. Enter these exact settings:
   * **API Key:** Paste your full key
   * **API Endpoint URL:** `https://api.openai.com/v1/chat/completions`
   * **Auth Header Name:** `Authorization`
   * **Auth Header Prefix:** `Bearer` (capital B)
   * **Extra Headers:** Leave empty
   * **Model:** `gpt-4o` (recommended)
3. Click **Save & Test Connection**
4. Look for green success message

**Recommended Models:**
* `gpt-4o-mini` - Cheapest ($0.15/M tokens) ✅ For comments
* `gpt-4o` - Balanced ($2.50/M tokens) ✅
* `gpt-4-turbo` - More powerful ($10/M tokens)

== Frequently Asked Questions ==

= Does Smart Moderator require OpenAI? =

No. You can use either Anthropic Claude or OpenAI GPT. As of version 2.0.0, these are the only supported providers.

= What happens if the API fails? =

If the request fails or credentials are missing, Smart Moderator defaults to "approve" to avoid blocking your publishing workflow.

= Can admins override moderation? =

Yes - if "Enforce moderation result" is OFF, manual WordPress status changes will not be re-checked.

If "Enforce moderation result" is ON, the plugin will re-apply the AI decision after 60 seconds, even if an admin changed it manually.

= How much does this cost? =

Costs depend on your chosen provider and model:
* Anthropic claude-haiku-4-5: ~$0.10 per 1,000 comments
* OpenAI gpt-4o-mini: ~$0.15 per 1,000 comments
* OpenAI gpt-4o: ~$2.50 per 1,000 comments

Typical blog with 100 comments/month: $0.01-$0.25/month

= What content gets sent to the AI? =

For posts: title + content (combined as one text)
For comments: comment text only (not name/email)

= Does it work with comment spam plugins? =

Yes. Smart Moderator runs after most spam filters, so it acts as a second layer of defense.

= Can I moderate custom post types? =

Yes. The plugin moderates all public post types except pages and attachments.

= Connection test fails - what do I do? =

**For Anthropic users:**
1. Auth Header Prefix must be EMPTY (not "Bearer")
2. Re-enter API key: delete all dots first, then paste fresh key
3. Auth Header Name must be lowercase: `x-api-key`
4. Extra Headers must include: `{"anthropic-version":"2023-06-01"}`

**For OpenAI users:**
1. Auth Header Prefix must be `Bearer` (capital B)
2. Re-enter API key: delete all dots first, then paste fresh key
3. Auth Header Name must be: `Authorization` (capital A)
4. Check billing at https://platform.openai.com/account/billing

See full troubleshooting guide in plugin settings or README.

= Is this compatible with caching or security plugins? =

Yes. It does not modify front-end behavior or rely on cookies.

= Does this track users? =

No. Smart Moderator does not collect or transmit any user data except the content being moderated to your configured AI endpoint.

= Can I see what the AI decided? =

Yes. Moderation logs are stored as metadata on each post/comment, including the decision (approve/reject), reason, and timestamp. A UI for viewing logs is planned for a future version.

= Does it work with multisite? =

Yes. Each site in a multisite network has its own settings.

== Screenshots ==

1. Settings page - API configuration with connection test
2. Settings page - Moderation prompt configuration
3. Success message after connection test
4. Example of moderation in action

== Changelog ==

= 2.0.0 =
* **Major refactoring** - Complete architecture redesign following SOLID principles
* **Breaking change** - Now explicitly supports only OpenAI and Claude (removed generic API support)
* **New** - Proper OpenAI API implementation with full GPT-4, GPT-4o, O1 model support
* **New** - Proper Claude API implementation with full Claude 3.5 and Claude 3 family support
* **New** - Auto-detection of AI provider based on endpoint URL
* **Fixed** - Editor post updates now properly remoderated (bug fix)
* **Fixed** - Response format parsing now provider-specific and more reliable
* **Improved** - Better error messages with provider context
* **Improved** - Type-safe value objects (Moderation_Result, Moderation_Config)
* **Improved** - Strategy pattern for API clients (easy to extend in future)
* **Improved** - Comprehensive error logging with provider identification
* **Code quality** - Follows WordPress coding standards and PHP best practices
* **Code quality** - Full separation of concerns (API logic separated from WordPress integration)

= 1.1.0 =
* Initial public release
* AI-powered moderation for posts and comments
* Support for Anthropic Claude and OpenAI GPT APIs
* Custom moderation prompts per content type
* "Enforce moderation result" feature - re-checks after 60s to prevent overrides
* Comprehensive error handling with user-friendly messages
* Detailed moderation logging
* Secure API key storage
* Full WordPress.org compliance

== Upgrade Notice ==

= 2.0.0 =
Major architecture update. OpenAI and Claude are now the only supported providers. If you're using a custom endpoint, you'll need to migrate to OpenAI or Claude. Existing OpenAI and Claude configurations will continue to work without changes.

= 1.1.0 =
Initial release. Configure your AI provider in Tools → Smart Moderator to start filtering spam and harmful content automatically.

== Privacy Policy ==

Smart Moderator is designed with privacy in mind:

**What we collect:**
* API key (stored in WordPress database, never transmitted except to your configured endpoint)
* Moderation logs: decision, reason, timestamp per post/comment

**What we DON'T collect:**
* User personal data (names, emails, IP addresses)
* Full content in logs
* Analytics or usage telemetry

**What gets sent to AI providers:**
* Post title and content (for post moderation)
* Comment text only (for comment moderation)
* Your configured moderation prompt

**Third-party services:**
This plugin sends data to your chosen AI provider (Anthropic or OpenAI) for moderation. Please review their privacy policies:
* Anthropic Privacy Policy: https://www.anthropic.com/privacy
* OpenAI Privacy Policy: https://openai.com/policies/privacy-policy

All API communication is encrypted via HTTPS.

== Support ==

For support, please use the WordPress.org support forum for this plugin.

For bug reports or feature requests, please open an issue on GitHub: [Your GitHub URL]

Enable WordPress debug logging (WP_DEBUG_LOG) to see detailed connection information in `/wp-content/debug.log` if you encounter issues.

== Additional Info ==

**Compatibility:**
* WordPress 6.0+ (tested up to 6.7)
* PHP 8.0+
* Works with: WooCommerce, bbPress, BuddyPress, and most comment plugins
* Compatible with: All major caching plugins, security plugins

**Languages:**
* English (more translations welcome via translate.wordpress.org)

**Requirements:**
* Outbound HTTPS connections must be enabled on your server
* API key from Anthropic or OpenAI (free tiers available)

**Performance:**
* Minimal impact on site speed (moderation runs asynchronously)
* No front-end JavaScript or CSS
* No database queries on front-end

**Security:**
* No eval(), exec(), or dangerous functions
* All database queries use $wpdb->prepare()
* All output escaped
* All input validated and sanitized
* API keys never appear in logs or debug output