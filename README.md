# Smart Moderator

**Version:** 2.0.0
**License:** GPLv2 or later
**Tested up to:** WordPress 6.8
**Requires PHP:** 8.0+

AI-powered automatic moderation for posts and comments using OpenAI or Anthropic Claude.

---

## 🧠 Overview

**Smart Moderator** integrates AI content moderation directly into WordPress. It reviews posts and comments the moment they're created, decides whether to approve or reject them, and logs every decision for transparency.

You can connect it to any compatible LLM API endpoint (OpenAI, Anthropic, or self-hosted) and define custom moderation prompts for both posts and comments.

---

## ✨ Features

- **AI moderation in real time** – Automatically analyze posts and comments as soon as they're created
- **Admin bypass** – Administrators skip AI moderation entirely (no API cost for admin actions)
- **Admin override system** – Admins can manually approve AI-rejected content without re-moderation loops
- **Immediate decision + delayed enforcement** – The AI's result is applied immediately, then optionally re-enforced after 60 seconds (respects admin overrides)
- **"Enforce moderation result" toggle** – When enabled, Smart Moderator ensures its moderation decision remains final even if other plugins, workflows, or users alter the status afterward
- **API failure rate limiting** – Automatically pauses API calls after repeated failures to prevent hammering the endpoint
- **Custom prompts** – Configure separate moderation rules for posts and comments
- **Detailed moderation logs** – Every decision is recorded with timestamp, reason, and decision type
- **Privacy-safe API handling** – Keys are stored securely, never logged publicly, and sent only via HTTPS
- **Compatible with existing WordPress moderation** – Works alongside WordPress's built-in comment moderation settings

---

## ⚙️ How It Works

1. **A post or comment is created or updated**
2. **Check if the user is an administrator:**
   - ✅ **If admin** → Skip all moderation, publish immediately (no API cost)
   - ❌ **If non-admin** → Continue to step 3
3. **Smart Moderator sends content to your AI endpoint** (title + content for posts, comment text for comments)
4. **The AI responds** with either `approve` or `reject` (and optionally a short reason)
5. **Smart Moderator applies the decision immediately:**
   - `approve` → publish or approve
   - `reject` → set to pending/unapproved
6. **If "Enforce moderation result" is enabled,** the plugin schedules a check 60 seconds later to re-apply the AI's decision if anything changed

> 🕐 This delayed check ensures Smart Moderator has the final say, even if another plugin or manual edit attempts to override the moderation result.

> 🔑 **Admin Override System:** When an admin manually publishes previously AI-rejected content, the plugin sets an "admin override" flag to prevent re-moderation loops. The delayed enforcement respects these admin decisions. If a non-admin later edits that content, the override flag is cleared and AI moderation runs again.

---

## 🤖 Supported AI Models

Smart Moderator 2.0 supports the latest OpenAI and Anthropic Claude models.

### OpenAI Models (2025)

**Supported models:**
- **GPT-3.5 Turbo** - All versions (`gpt-3.5-turbo`)
- **GPT-4** - All versions (`gpt-4`, `gpt-4-turbo`)
- **GPT-4o** - All versions (`gpt-4o`, `gpt-4o-mini`, `chatgpt-4o-latest`)
- **GPT-5** - All versions (`gpt-5`, `gpt-5-mini`, `gpt-5-nano`)
- **O1 Series** - All versions (`o1`, `o1-preview`, `o1-mini`)

No token limits are enforced - models generate natural responses. GPT-5 models automatically use `reasoning_effort: "low"` for efficient moderation.

### Anthropic Claude Models (2025)

**Supported models:**
- **Claude 4.x** - Opus 4.5, Sonnet 4, Haiku 4.5
- **Claude 3.x** - Opus 3, Sonnet 3.5, Haiku 3
- **Claude 2.x** - 2.1, 2.0

Uses Claude's Messages API with a generous 1024 token limit (required by API).

---

## 🚀 Quick Start Setup

### Option 1: Anthropic Claude API (Recommended)

**Step 1: Get Your API Key**

1. Go to [console.anthropic.com](https://console.anthropic.com/)
2. Click "Sign Up" if you don't have an account, or "Sign In" if you do
3. Once logged in, click on your profile/organization name in the top right
4. Select "API Keys" from the menu
5. Click "Create Key" button
6. Give your key a name (e.g., "WordPress Smart Moderator")
7. **IMPORTANT:** Copy the API key immediately - you won't be able to see it again!
   - It will look like: `sk-ant-api03-xxxxxxxxxxxxxxxxxxxxxxxxxxx`
   - Save it in a secure location (password manager recommended)

**Step 2: Configure Smart Moderator**

1. In WordPress admin, go to **Tools → Smart Moderator**
2. Enter these **exact** settings:

   **API Key:**
   ```
   sk-ant-api03-your-actual-key-here
   ```
   ⚠️ Paste your full API key (100+ characters)

   **API Endpoint URL:**
   ```
   https://api.anthropic.com/v1/messages
   ```
   ⚠️ Copy this exactly - no trailing slash!

   **Auth Header Name:**
   ```
   x-api-key
   ```
   ⚠️ All lowercase, with hyphen

   **Auth Header Prefix:**
   ```
   
   ```
   ⚠️ **LEAVE THIS COMPLETELY EMPTY!** Do not type "Bearer" or anything else!

   **Extra Headers:**
   ```json
   {"anthropic-version":"2023-06-01"}
   ```
   ⚠️ Copy this exactly as shown - it must be valid JSON

   **Model:**
   ```
   claude-sonnet-4-20250514
   ```
   **Recommended models** (choose based on your needs):
   - `claude-haiku-4-5` - Fastest, most economical ($1 per million input tokens)
   - `claude-sonnet-4-20250514` - Balanced performance ($3 per million) ✅ **Recommended**
   - `claude-sonnet-4-5-20250929` - Most powerful coding/reasoning ($3 per million)
   - `claude-opus-4-1-20250805` - Maximum intelligence ($15 per million)

**Step 3: Save and Test**

1. Click **"Save & Test Connection"** button at the bottom
2. Wait a few seconds for the test to complete
3. ✅ **Success:** You should see a green notification:
   ```
   "Settings saved. ✓ Connection test successful! API is responding correctly."
   ```
4. ❌ **If you see an error,** see the [Troubleshooting](#-troubleshooting) section below

**Example API Request** (for technical reference):
```bash
curl -X POST https://api.anthropic.com/v1/messages \
  -H "Content-Type: application/json" \
  -H "x-api-key: sk-ant-api03-your-key-here" \
  -H "anthropic-version: 2023-06-01" \
  -d '{
    "model": "claude-sonnet-4-20250514",
    "max_tokens": 100,
    "messages": [{"role": "user", "content": "Hello"}]
  }'
```

---

### Option 2: OpenAI GPT API

**Step 1: Get Your API Key**

1. Go to [platform.openai.com](https://platform.openai.com/)
2. Click "Sign up" if you don't have an account, or "Log in" if you do
3. Once logged in, click on your profile picture in the top right
4. Select "Your profile" → "User API keys" OR go directly to [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
5. Click "Create new secret key" button
6. Give your key a name (e.g., "WordPress Smart Moderator")
7. **IMPORTANT:** Copy the API key immediately - you won't be able to see it again!
   - It will look like: `sk-proj-xxxxxxxxxxxxxxxxxxxxxxxxxxx` or `sk-xxxxxxxxxxx`
   - Save it in a secure location (password manager recommended)

⚠️ **Note:** You'll need to add billing information and credits to your OpenAI account before the API will work.

**Step 2: Configure Smart Moderator**

1. In WordPress admin, go to **Tools → Smart Moderator**
2. Enter these **exact** settings:

   **API Key:**
   ```
   sk-proj-your-actual-key-here
   ```
   ⚠️ Paste your full API key (starts with `sk-` or `sk-proj-`)

   **API Endpoint URL:**
   ```
   https://api.openai.com/v1/chat/completions
   ```
   ⚠️ Copy this exactly - no trailing slash!

   **Auth Header Name:**
   ```
   Authorization
   ```
   ⚠️ Capital 'A', no spaces

   **Auth Header Prefix:**
   ```
   Bearer
   ```
   ⚠️ **Type exactly:** `Bearer` (capital B, a space will be added automatically after it)

   **Extra Headers:**
   ```
   
   ```
   ⚠️ **LEAVE THIS EMPTY** - OpenAI doesn't need extra headers

   **Model:**
   ```
   gpt-4o
   ```
   **Recommended models** (choose based on your needs):
   - `gpt-4o-mini` - Fastest, most economical ($0.15 per million input tokens) ✅ **Good for comments**
   - `gpt-4o` - Balanced, general purpose ($2.50 per million) ✅ **Recommended**
   - `gpt-4-turbo` - Enhanced performance ($10 per million)
   - `gpt-4` - Original GPT-4 ($30 per million)

**Step 3: Save and Test**

1. Click **"Save & Test Connection"** button at the bottom
2. Wait a few seconds for the test to complete
3. ✅ **Success:** You should see a green notification:
   ```
   "Settings saved. ✓ Connection test successful! API is responding correctly."
   ```
4. ❌ **If you see an error,** see the [Troubleshooting](#-troubleshooting) section below

**Example API Request** (for technical reference):
```bash
curl https://api.openai.com/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer sk-proj-your-key-here" \
  -d '{
    "model": "gpt-4o",
    "messages": [{"role": "user", "content": "Hello"}]
  }'
```

---

## 🛠 Troubleshooting

### ❌ Connection Test Fails with "invalid x-api-key" or "Authentication Error"

This is the **most common error** and is usually caused by incorrect configuration.

**For Anthropic Users:**

1. **Check Auth Header Prefix field:**
   - It must be **completely empty**
   - If you see anything in this field (like "Bearer"), **delete it all**

2. **Re-enter your API key:**
   - The API key field shows dots (`•••`) after saving to protect your key
   - To fix: **Delete ALL the dots** first, then paste your fresh API key
   - Your key should start with `sk-ant-api03-`
   - It should be 100+ characters long

3. **Verify Auth Header Name:**
   - Must be exactly: `x-api-key` (all lowercase, with hyphen)
   - NOT: `X-API-Key` or `x_api_key` or `Authorization`

4. **Check Extra Headers:**
   - Must include: `{"anthropic-version":"2023-06-01"}`
   - Make sure it's valid JSON (use a JSON validator if unsure)

**For OpenAI Users:**

1. **Check Auth Header Prefix field:**
   - Must be exactly: `Bearer` (capital B)
   - The plugin automatically adds a space after it

2. **Re-enter your API key:**
   - The API key field shows dots (`•••`) after saving
   - To fix: **Delete ALL the dots** first, then paste your fresh API key
   - Your key should start with `sk-` or `sk-proj-`

3. **Verify Auth Header Name:**
   - Must be exactly: `Authorization` (capital A)
   - NOT: `authorization` or `X-API-Key`

4. **Check your OpenAI account:**
   - Make sure you have billing set up: [platform.openai.com/account/billing](https://platform.openai.com/account/billing)
   - Ensure you have credits available
   - Some accounts may have rate limits or usage tiers

---

### ❌ Connection Test Fails with "Unsupported parameter: 'messages'"

**Error message:**
```
Unsupported parameter: 'messages'. In the Responses API, this parameter has moved to 'input'.
```

**What this means:**
You have entered an **incorrect API endpoint URL**. There is no "Responses API" in OpenAI or Anthropic's official APIs.

**How to fix:**

1. Go to **Tools → Smart Moderator**
2. Check the **"API Endpoint URL"** field
3. Make sure it's **exactly** one of these:

**For OpenAI:**
```
https://api.openai.com/v1/chat/completions
```

**For Anthropic Claude:**
```
https://api.anthropic.com/v1/messages
```

**Common mistakes:**
- ❌ `https://api.openai.com/v1/responses/create` (doesn't exist)
- ❌ `https://api.openai.com/v1/completions` (legacy endpoint, not compatible)
- ❌ Missing `/v1/` in the URL
- ❌ Extra trailing slash at the end

**Verify with official documentation:**
- OpenAI Chat Completions: [platform.openai.com/docs/guides/chat-completions](https://platform.openai.com/docs/guides/chat-completions)
- Anthropic Messages API: [docs.anthropic.com/en/api/messages](https://docs.anthropic.com/en/api/messages)

---

### ❌ Connection Test Fails with "Could not resolve host" or Network Error

**Possible causes:**
- WordPress server can't reach the API endpoint
- Firewall blocking outbound HTTPS requests
- DNS issues

**Solutions:**

1. **Test from your server:**
   ```bash
   curl -I https://api.anthropic.com/v1/messages
   # or
   curl -I https://api.openai.com/v1/chat/completions
   ```
   If this fails, your server cannot reach the internet.

2. **Contact your hosting provider:**
   - Ensure outbound HTTPS connections are allowed
   - Some hosts block external API calls by default
   - Ask them to whitelist `api.anthropic.com` or `api.openai.com`

3. **Check if you need a proxy:**
   - Some enterprise environments require proxy configuration
   - This is rare for typical WordPress hosting

---

### 🐛 Check Debug Logs

Enable WordPress debug logging to see detailed connection information:

1. Add to your `wp-config.php`:
   ```php
   define('WP_DEBUG', true);
   define('WP_DEBUG_LOG', true);
   define('WP_DEBUG_DISPLAY', false);
   ```

2. Check `/wp-content/debug.log` after saving settings

3. Look for lines starting with "Smart Moderator:"

**What to check in logs:**
- API key length (should be 100+ characters for most providers)
- Headers being sent (ensure no "Bearer" prefix for Anthropic)
- Response status code (should be 200 for success)
- Exact error message from the API

---

### ⚠️ Moderation Not Running

**Check comment moderation is enabled:**

1. Go to **Tools → Smart Moderator**
2. Look for the checkbox labeled **"Moderate comments"**
3. Make sure it's checked
4. Save settings

**Check post types:**

- Plugin moderates public post types (posts, by default)
- Pages and attachments are excluded
- Custom post types that are public will be moderated

**Check error logs:**

- Look for API errors in `/wp-content/debug.log`
- Plugin falls back to "approve" if API fails (so content won't be blocked)

---

### 💰 API Quota Exceeded / Rate Limit Errors

Both Anthropic and OpenAI have rate limits and usage quotas.

**Anthropic Rate Limits:**
- Free tier: Limited requests per minute
- Paid tier varies by usage level
- Check your usage: [console.anthropic.com/settings/usage](https://console.anthropic.com/settings/usage)

**OpenAI Rate Limits:**
- Free trial: Very limited (3-5 requests per minute)
- Tier 1 (after $5 payment): ~500 RPM, ~500K TPM
- Tier 2 and higher: Increases automatically with usage
- Check your limits: [platform.openai.com/settings/organization/limits](https://platform.openai.com/settings/organization/limits)

**Solutions:**
- Upgrade your API plan
- Reduce moderation scope (e.g., comments only, not posts)
- Use a cheaper/faster model (e.g., `gpt-4o-mini` or `claude-haiku-4-5`)

---

### 🚫 API Failure Rate Limiting

Smart Moderator automatically detects repeated API failures and pauses API calls to prevent hammering the endpoint.

**How it works:**
- After 5 consecutive API failures within 1 hour, rate limiting activates
- During rate limiting, content defaults to "approve" (doesn't block content)
- Rate limit automatically resets on the first successful API response
- Failure count stored in transient (`smartmoderator_api_failures`)

**What you'll see:**
- Moderation reason: "API temporarily unavailable (rate limited after 5 consecutive failures)"
- Content will be auto-approved during this period
- Normal operation resumes automatically once API is accessible

**If this happens frequently:**
- Check your API endpoint URL is correct
- Verify your API key is valid and has credits
- Check if your hosting provider blocks external HTTPS requests
- Review API error logs at `/wp-content/debug.log` (enable WP_DEBUG first)

---

### 🔍 What Gets Sent to the AI?

**For Posts:**
- Post title + post content (combined)
- No excerpts, no custom fields, no metadata

**For Comments:**
- Comment text only
- Not the commenter name, email, or IP address

**Example of what the AI sees for a post:**
```
My Blog Post Title
This is the content of my blog post. It can be multiple paragraphs...
```

---

## 📊 Understanding Moderation Behavior

### What Content Gets Moderated?

**Posts:**
- All public post types (typically "posts") created by **non-admin users**
- Pages are excluded
- Attachments are excluded
- Custom post types that are marked as "public" will be moderated
- **Admins bypass all moderation** (users with `manage_options` capability)

**Comments:**
- Only if "Moderate comments" is enabled in settings
- Comments created by **non-admin users** are moderated
- **Admin comments bypass moderation** (no API call for admin comments)

**Who is an "Admin"?**
- Users with the `manage_options` capability (typically the "Administrator" role)
- These users skip AI moderation entirely to:
  - Save API costs on trusted user actions
  - Allow admins to quickly publish content without delays
  - Enable admins to override AI decisions without re-moderation loops

### How Does the AI Make Decisions?

The AI receives your content with a prompt like:

```
[Your Base Prompt - defined in settings]

Respond only with "approve" or "reject", optionally followed by a short reason after a dash.

Additional rules: [Your Extra Rules for Posts/Comments]

Content:
[The actual post title + content, or comment text]
```

**The AI responds with something like:**
- `approve` → Content is published/approved
- `approve - looks good` → Content is published/approved (reason logged)
- `reject` → Content is held for moderation
- `reject - contains spam` → Content is held (reason logged)

**Decision parsing:**
- Case-insensitive: `Approve`, `APPROVE`, `approve` all work
- Reason is optional: The AI can just say "approve" or "reject"
- Anything not starting with "reject" is treated as "approve"

### The "Enforce Moderation Result" Feature

When **enabled** (checkbox in settings):

1. **Immediate moderation happens** as usual (approve or reject)
2. **60 seconds later**, WordPress checks if the status changed
3. **If changed**, it re-applies the AI's original decision
4. **Exception:** Admin overrides are respected and never undone

**Example scenario (non-admin override):**
- AI rejects a spam comment → comment is set to "pending"
- Another plugin automatically approves it 30 seconds later
- After 60 seconds total, Smart Moderator sees the change
- It **re-rejects** the comment, setting it back to "pending"

**Example scenario (admin override):**
- AI rejects a post → post is set to "pending"
- An admin manually publishes it → admin override flag is set
- After 60 seconds, Smart Moderator sees the admin override flag
- It **respects the admin's decision** and leaves the post published

**When this is useful:**
- You have other plugins that might override moderation
- You want AI to have final say (except for admin overrides)
- You're testing the plugin and manually changing statuses

**When to disable it:**
- You want any manual edit to override AI
- You don't have conflicting plugins
- You prefer flexibility over enforcement

**Admin Override Behavior:**
- When an admin publishes previously AI-rejected content, an "admin override" flag is set
- This flag prevents the delayed enforcement from reverting the admin's decision
- If a non-admin later edits that content, the override flag is cleared and AI moderation runs again
- Admins editing admin-overridden content maintain the bypass (no re-moderation)

### Moderation Logs

Every moderation decision is logged per post/comment:

**Where to view logs:**
- Not yet visible in UI (coming in a future version)
- Stored in post/comment metadata (`_smartmoderator_log`)
- Multiple entries if content is moderated multiple times

**What's logged:**
- Decision: `approve` or `reject`
- Reason: AI's explanation (or default message)
- Timestamp: When the decision was made

**Logs DO NOT contain:**
- Full post/comment content
- API keys
- API responses (except decision + reason)

---

## 💰 Cost Estimates

Moderation costs depend on your chosen model and usage.

**Typical costs per item:**

| Provider | Model | Cost per 1K comments* | Cost per 100 posts* |
|----------|-------|----------------------|-------------------|
| Anthropic | claude-haiku-4-5 | ~$0.10 | ~$0.05 |
| Anthropic | claude-sonnet-4 | ~$0.30 | ~$0.15 |
| OpenAI | gpt-4o-mini | ~$0.15 | ~$0.08 |
| OpenAI | gpt-4o | ~$2.50 | ~$1.25 |

*Estimates based on typical comment length (~100 tokens) and post length (~500 tokens). Actual costs vary.

**How to minimize costs:**
- Use faster/cheaper models for comments (`gpt-4o-mini` or `claude-haiku-4-5`)
- Use more powerful models only for posts
- Disable post moderation if you only need comment filtering
- Turn off "Enforce moderation result" to avoid duplicate API calls

**Monitoring usage:**
- Anthropic: [console.anthropic.com/settings/usage](https://console.anthropic.com/settings/usage)
- OpenAI: [platform.openai.com/usage](https://platform.openai.com/usage)

---

## 🔐 Privacy & Data Handling

**What Smart Moderator stores:**
- API key in WordPress options table (encrypted at database level if you use encryption)
- Moderation logs: decision, reason, timestamp per post/comment
- Last check cache: snippet of content (200 chars max) + AI response for debugging

**What Smart Moderator NEVER stores or logs:**
- Full post/comment content in logs
- API keys in debug logs
- User IP addresses
- Commenter email addresses

**What gets sent to the API:**
- Post: title + content only
- Comment: comment text only
- Your configured moderation prompt

**What the API providers see:**
- Content being moderated
- Your API key (for authentication)
- Standard HTTPS request metadata

**Data transmission:**
- All API calls use HTTPS (encrypted in transit)
- No data is sent to any third party except your configured API endpoint
- No tracking, analytics, or telemetry

**For GDPR/Privacy compliance:**
- User content is processed for legitimate moderation purposes
- No personal data is collected beyond what WordPress already stores
- API providers (OpenAI/Anthropic) process content per their terms of service
- Review their privacy policies: [Anthropic Privacy](https://www.anthropic.com/privacy) | [OpenAI Privacy](https://openai.com/policies/privacy-policy)

---

## ⚙️ Admin Settings Explained

Access via **Tools → Smart Moderator**.

### API Configuration

**API Key**
- Your authentication key from OpenAI or Anthropic
- Masked with dots (`•••`) after saving for security
- Never visible again after saving - if you lose it, create a new one
- To update: delete all dots, paste new key, save

**API Endpoint URL**
- Full URL to the API endpoint
- Anthropic: `https://api.anthropic.com/v1/messages`
- OpenAI: `https://api.openai.com/v1/chat/completions`
- No trailing slash

**Auth Header Name**
- HTTP header name for authentication
- Anthropic: `x-api-key`
- OpenAI: `Authorization`
- Case-sensitive

**Auth Header Prefix**
- Prefix added before API key in the header
- Anthropic: **Empty** (no prefix)
- OpenAI: `Bearer` (plugin adds space automatically)

**Extra Headers**
- Additional HTTP headers as JSON object
- Anthropic: `{"anthropic-version":"2023-06-01"}`
- OpenAI: Leave empty
- Must be valid JSON

**Model**
- Optional model identifier
- Anthropic: `claude-sonnet-4-20250514`, `claude-haiku-4-5`, etc.
- OpenAI: `gpt-4o`, `gpt-4o-mini`, etc.
- Leave empty to use provider's default (not recommended)

### Moderation Settings

**Base Prompt**
- Shared moderation rules for all content
- Example: *"Reject content containing spam, hate speech, or illegal activity. Approve everything else."*
- This is sent before every post and comment

**Extra Rules for Posts**
- Additional instructions only for post moderation
- Example: *"Posts must be at least 100 words and on-topic."*

**Extra Rules for Comments**
- Additional instructions only for comment moderation
- Example: *"Reject comments shorter than 10 characters or containing only links."*

**Moderate Comments**
- Checkbox to enable/disable comment moderation
- When unchecked, comments are not sent to AI
- Posts are always moderated (cannot disable)

**Enforce Moderation Result**
- Checkbox to enable delayed re-enforcement
- When enabled: AI's decision is re-applied after 60 seconds
- Prevents other plugins or manual changes from overriding AI
- When disabled: AI decision is applied once, no re-checks

### Test Connection Button

**What it does:**
- Sends a test request to your API endpoint
- Checks authentication and configuration
- Does NOT charge your API account (uses minimal tokens)

**Success message:**
```
Settings saved. ✓ Connection test successful! API is responding correctly.
```

**Failure messages:**
- Authentication errors → Check API key and headers
- Network errors → Check endpoint URL and server connectivity
- API errors → Check model name and provider status

---

## 🧪 Technical Details

**WordPress Hooks Used:**
- `save_post` - Triggers post moderation (priority 999)
- `wp_after_insert_post` - Schedules delayed enforcement (priority 9999)
- `comment_post` - Triggers comment moderation
- `edit_comment` - Handles edited comments
- `wp_update_comment` - Handles REST API comment updates

**Scheduled Events:**
- `smartmoderator_publish_post` - Re-applies post decision after 60s
- `smartmoderator_publish_comment` - Re-applies comment decision after 60s

**Post Metadata:**
- `_smartmoderator_log` - Array of moderation decisions
- `_smartmoderator_processed` - Temporary flag during moderation
- `_smartmoderator_ai_decision` - Last AI decision ('approve' or 'reject')
- `_smartmoderator_admin_override` - Flag set when admin overrides AI rejection

**Comment Metadata:**
- `_smartmoderator_log` - Array of moderation decisions
- `_smartmoderator_ai_decision` - Last AI decision ('approve' or 'reject')
- `_smartmoderator_admin_override` - Flag set when admin overrides AI rejection

**Transients:**
- `smartmoderator_api_failures` - Tracks consecutive API failures for rate limiting (1 hour TTL)

**Fallback Behavior:**
- If API fails or is unreachable → defaults to **"approve"**
- After 5 consecutive API failures → rate limiting activates (skips API calls temporarily)
- Rate limit resets automatically on first successful API response
- This prevents blocking legitimate content due to API issues
- Errors are logged to PHP error log for debugging

---

## 📋 WordPress.org Compliance

This plugin follows all WordPress.org plugin directory guidelines:

- ✅ **Database Security:** All queries use `$wpdb->prepare()` for SQL injection protection
- ✅ **Input Validation:** All user input is validated and sanitized using WordPress functions
- ✅ **Output Escaping:** All output is properly escaped (`esc_html`, `esc_attr`, `esc_url`, etc.)
- ✅ **No User Tracking:** No user data is collected or transmitted except content being moderated
- ✅ **No Locked Features:** All functionality is available without payment
- ✅ **GPL Licensed:** Fully open source under GPLv2 or later
- ✅ **Secure API Handling:** API keys stored securely, transmitted over HTTPS only
- ✅ **Human Readable Code:** Well-documented, follows WordPress coding standards
- ✅ **No Remote Code Execution:** No `eval()`, `create_function()`, or similar dangerous functions
- ✅ **Accessibility:** Admin interface follows WCAG 2.0 AA standards

---

## 🔗 Useful Links

**Official API Documentation:**
- Anthropic Claude API: [docs.anthropic.com/en/api/messages](https://docs.anthropic.com/en/api/messages)
- OpenAI Chat Completions API: [platform.openai.com/docs/guides/chat-completions](https://platform.openai.com/docs/guides/chat-completions)
- OpenAI API Reference: [platform.openai.com/docs/api-reference/chat](https://platform.openai.com/docs/api-reference/chat)

**Get API Keys:**
- Anthropic Console: [console.anthropic.com](https://console.anthropic.com/)
- OpenAI Platform: [platform.openai.com/api-keys](https://platform.openai.com/api-keys)

**Pricing Information:**
- Anthropic Pricing: [anthropic.com/pricing](https://www.anthropic.com/pricing)
- OpenAI Pricing: [openai.com/api/pricing](https://openai.com/api/pricing)

**Monitor Usage:**
- Anthropic Usage Dashboard: [console.anthropic.com/settings/usage](https://console.anthropic.com/settings/usage)
- OpenAI Usage Dashboard: [platform.openai.com/usage](https://platform.openai.com/usage)

**API Status & Updates:**
- Anthropic Status: [status.anthropic.com](https://status.anthropic.com)
- OpenAI Status: [status.openai.com](https://status.openai.com)

**Support:**
- Report bugs or issues: [github.com/linda-scoon/smart-moderator/issues](https://github.com/linda-scoon/smart-moderator/issues)
- Plugin Documentation: [linda-scoon.github.io/smart-moderator-documentation](https://linda-scoon.github.io/smart-moderator-documentation/)

---

## 🧰 Developer Notes

You can filter or extend behavior using standard WordPress APIs.

### Example Filters:

```php
// Override AI decision based on content
add_filter('smartmoderator_decision_override', function ($decision, $content) {
    if (stripos($content, 'safe word') !== false) {
        return 'approve';
    }
    return $decision;
}, 10, 2);

// Modify prompt before sending to AI
add_filter('smartmoderator_prompt', function ($prompt, $content, $context) {
    if ($context === 'posts') {
        $prompt .= "\n\nPosts must be professional and well-formatted.";
    }
    return $prompt;
}, 10, 3);

// Custom logging
add_action('smartmoderator_decision_made', function ($decision, $content_id, $context) {
    error_log("Smart Moderator: $context #$content_id -> $decision");
}, 10, 3);
```

---

## 📝 Changelog

### 2.0.0 - Major Architecture Refactor
* **Breaking Change:** Now explicitly supports only OpenAI and Claude (removed generic API support for improved reliability)
* **Architecture:** Complete refactoring following SOLID principles and WordPress best practices
* **New:** Proper OpenAI client with full GPT-4, GPT-4o, O1 model support
* **New:** Proper Claude client with full Claude 3.5 and Claude 3 family support
* **New:** Auto-detection of AI provider based on endpoint URL
* **Fixed:** Editor post updates now properly remoderated
* **Fixed:** Response format parsing now provider-specific and more reliable
* **Improved:** Better error messages with provider context
* **Improved:** Type-safe value objects for results and configuration
* **Improved:** Strategy pattern implementation for easy future extensions
* **Code Quality:** Full separation of concerns (API logic separated from WordPress integration)
* **Code Quality:** Comprehensive docblocks and following WordPress coding standards

### 1.1.0 - Initial Release
* **AI-Powered Moderation:** Real-time content analysis for posts and comments using OpenAI, Anthropic Claude, or compatible LLM APIs
* **Admin Bypass System:** Administrators skip all AI moderation (saves API costs, prevents loops)
* **Admin Override Flag:** Prevents re-moderation when admins consciously override AI decisions
* **Delayed Enforcement:** Optional 60-second re-check to ensure moderation decisions remain final
* **API Failure Rate Limiting:** Automatically pauses API calls after 5 consecutive failures to prevent endpoint hammering
* **OpenAI Compatibility:** Auto-detects and uses `max_completion_tokens` for newer OpenAI models (o1, gpt-4o variants)
* **Custom Prompts:** Separate moderation rules for posts and comments
* **Detailed Logging:** Every decision recorded with timestamp, reason, and decision type
* **Complete Uninstall:** Removes all metadata, options, and transients when uninstalled
* **WordPress.org Compliance:** Full adherence to WordPress plugin directory guidelines
* **Comprehensive Documentation:** Official API references, troubleshooting guides, and setup instructions

---

## 👏 Credits

Smart Moderator uses:
- WordPress HTTP API for secure API communication
- WP-Cron for scheduled enforcement
- Standard WordPress hooks and filters

No external libraries or dependencies required.

---

**Need help?** Check the [Troubleshooting](#-troubleshooting) section or enable debug logging to see what's happening behind the scenes.