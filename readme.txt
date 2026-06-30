=== Smart Moderator (AI-Powered) ===
Contributors: scoonlinda
Tags: spam filter, comment moderation, ai, content moderation, antispam
Requires at least: 6.0
Tested up to: 6.8
Requires PHP: 8.0
Stable tag: 2.1.0
License: GPLv2 or later
License URI: https://www.gnu.org/licenses/gpl-2.0.html

AI-powered automatic moderation for posts and comments using OpenAI or Anthropic Claude. Stops spam and harmful content before it's published.

== Description ==

Smart Moderator automatically reviews new posts and comments using AI (OpenAI GPT or Anthropic Claude) to prevent spam, abuse, and harmful content before publication.

### 🧠 Key Features

* **AI-driven moderation** – content is reviewed by your configured AI model via secure API
* **Custom prompts** – define global moderation rules and add context for posts or comments separately
* **Immediate moderation** – content is approved or rejected as soon as it's saved
* **Rejected content reverts to draft** – rejected posts become drafts (visible only to their author and editors), so authors can revise and resubmit
* **Theme & plugin friendly** – fire your own cleanup routine on rejection via the `smartmoderator_reject_post` hook, or change the fallback status with a filter (see Developer Hooks)
* **Audit log with retention** – every decision is stored in a dedicated table and kept for a configurable period (default 30 days), then pruned automatically
* **Secure storage** – API keys kept in database, never logged, transmitted only via HTTPS
* **Compatible** – works with WordPress comment moderation settings and other plugins

### ⚙️ How It Works

1. User submits a post or comment
2. Smart Moderator sends it to your AI endpoint (OpenAI, Anthropic, or custom)
3. AI returns "approve" or "reject" with optional reason
4. Plugin updates post/comment status:
   * Approved → published (if permissions allow)
   * Rejected post → reverted to **draft** (author-only visibility); rejected comment → held for moderation
5. Themes and plugins can take over the rejection outcome via the `smartmoderator_reject_post` action (for example, a classifieds theme that returns an ad to draft and notifies the author). When no listener is registered, the plugin applies its own default.

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

If the request fails, credentials are missing, or the AI can't make a determination, Smart Moderator does nothing and leaves the content exactly as it was. It only ever changes status when the AI explicitly approves or rejects — so a temporary outage never auto-publishes spam or wrongly rejects a good ad, and it doesn't waste API tokens re-checking.

= Can admins override moderation? =

Yes. Moderation runs once when content is saved, and admins (users who can manage options) bypass moderation entirely. Manual WordPress status changes are respected and are not second-guessed by the plugin.

Earlier versions had an "Enforce moderation result" option that re-applied the AI decision 60 seconds after saving. That aggressive behaviour fought other plugins and themes, so it is now off by default. Developers who genuinely need it can re-enable it with the `smartmoderator_reenforce_decision` filter (see Developer Hooks).

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

Yes. Moderation decisions are stored in a dedicated log table (decision, reason, timestamp) and shown on the **Tools → Smart Moderator** logs screen. You choose how long logs are kept under **Keep moderation logs for** (default 30 days); older entries are pruned automatically once a day.

= My logs keep disappearing — how do I keep them longer? =

Set a longer period under **Keep moderation logs for** (up to 90 days, or "Forever"). Logs are stored in their own table rather than in post/comment meta, so keeping them longer does not slow down the rest of your site. Developers can override the period with the `smartmoderator_log_retention_days` filter.

= Does it work with multisite? =

Yes. Each site in a multisite network has its own settings.

== Screenshots ==

1. Settings page - API configuration with connection test
2. Settings page - Moderation prompt configuration
3. Success message after connection test
4. Example of moderation in action

== Changelog ==

= 2.1.0 =
* **Security** - Outbound API requests now use wp_safe_remote_post(), blocking loopback/private/link-local hosts (SSRF mitigation for the configurable endpoint).
* **Security/Compliance** - Admin JavaScript and CSS are now enqueued (no inline scripts/styles); removed verbose debug logging; prefixed all internal class names to avoid collisions.
* **Fixed** - API failures now correctly leave content untouched and engage rate limiting (previously some error paths could auto-approve).
* **Changed** - On API error, missing config, or rate limiting, the plugin now does nothing (leaves content unchanged) instead of defaulting to "approve". It only changes status on an explicit AI approve/reject.
* **New** - No-code rejection settings: choose the status rejected posts are set to (default Draft), and optionally name a theme action to trigger on rejection — no `functions.php` editing required.
* **Changed** - Rejected posts are now reverted to **draft** (author-only visibility) instead of pending, so authors can revise and resubmit.
* **New** - Developer hooks for integrating with themes/plugins: `smartmoderator_reject_post` (action), `smartmoderator_rejected_post_status` (filter), `smartmoderator_reenforce_decision` (filter), `smartmoderator_log_retention_days` (filter), and `smartmoderator_log_max_rows` (filter).
* **Changed** - The default-on "Enforce moderation result" 60-second re-enforcement has been removed from the UI. It fought other plugins/themes and is now off by default, available only via the `smartmoderator_reenforce_decision` filter.
* **New** - Moderation logs are now stored in a dedicated table with a configurable retention period (default 30 days) instead of being wiped in full every day. New "Keep moderation logs for" setting.
* **Improved** - Logging no longer accumulates in post/comment meta, keeping core tables lean on high-volume sites.

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

= 2.1.0 =
Rejected posts now become drafts (author-only) instead of pending, and the 60-second re-enforcement is off by default. Logs move to a dedicated table with a retention setting (default 30 days). New developer hooks let themes/plugins customise rejection handling.

= 2.0.0 =
Major architecture update. OpenAI and Claude are now the only supported providers. If you're using a custom endpoint, you'll need to migrate to OpenAI or Claude. Existing OpenAI and Claude configurations will continue to work without changes.

= 1.1.0 =
Initial release. Configure your AI provider in Tools → Smart Moderator to start filtering spam and harmful content automatically.

== Developer Hooks ==

Smart Moderator exposes hooks so themes and plugins can integrate without modifying the plugin.

**`smartmoderator_reject_post`** (action) — fires when a post is rejected, passing the post ID, the post object, and the AI's reason. If any callback is attached, it fully owns the outcome and the plugin's default status change is skipped.

`add_action( 'smartmoderator_reject_post', function ( $post_id, $post, $reason ) { /* set draft + email author */ }, 10, 3 );`

The decision and reason are also saved as post meta (`_smartmoderator_ai_decision`, `_smartmoderator_ai_reason`).

**`smartmoderator_rejected_post_status`** (filter) — the post status applied to rejected content when no `smartmoderator_reject_post` listener is registered. Default `draft`.

`add_filter( 'smartmoderator_rejected_post_status', fn() => 'pending' );`

**`smartmoderator_reenforce_decision`** (filter) — return `true` to re-apply the AI decision 60 seconds after saving (off by default).

`add_filter( 'smartmoderator_reenforce_decision', '__return_true' );`

**`smartmoderator_log_retention_days`** (filter) — number of days to keep logs (0 = keep indefinitely).

**`smartmoderator_log_max_rows`** (filter) — hard cap on stored log rows (0 = no cap).

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