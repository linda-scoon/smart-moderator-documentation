# Smart Moderator - Quick Setup Reference Card

Print this or keep it handy during setup! ✂️

---

## 🔵 ANTHROPIC CLAUDE SETUP

**Get API Key:** https://console.anthropic.com (Sign up → API Keys → Create Key)

### Settings to Enter:

| Field | Value | Notes |
|-------|-------|-------|
| **API Key** | `sk-ant-api03-xxxxx...` | ~108 characters, paste your actual key |
| **API Endpoint URL** | `https://api.anthropic.com/v1/messages` | Copy exactly, no trailing slash |
| **Auth Header Name** | `x-api-key` | Lowercase, with hyphen |
| **Auth Header Prefix** | *(leave empty)* | **DO NOT type "Bearer"!** |
| **Extra Headers** | `{"anthropic-version":"2023-06-01"}` | Copy exactly, must be valid JSON |
| **Model** | `claude-sonnet-4-20250514` | Recommended ($3 per million tokens) |

### Other Model Options:
- `claude-haiku-4-5` - Cheapest ($1/M) ✅ Good for comments
- `claude-sonnet-4-5-20250929` - Most powerful ($3/M)
- `claude-opus-4-1-20250805` - Maximum intelligence ($15/M)

### Success Message:
```
Settings saved. ✓ Connection test successful! API is responding correctly.
```

### Common Error:
```
❌ "invalid x-api-key"
→ Check: Auth Header Prefix is EMPTY (not "Bearer")
→ Check: Delete all dots (•••) before pasting new key
```

---

## 🟢 OPENAI GPT SETUP

**Get API Key:** https://platform.openai.com/api-keys (Sign up → Create new secret key)

### Settings to Enter:

| Field | Value | Notes |
|-------|-------|-------|
| **API Key** | `sk-proj-xxxxx...` | Starts with `sk-` or `sk-proj-` |
| **API Endpoint URL** | `https://api.openai.com/v1/chat/completions` | Copy exactly, no trailing slash |
| **Auth Header Name** | `Authorization` | Capital 'A' |
| **Auth Header Prefix** | `Bearer` | Capital 'B' (space added automatically) |
| **Extra Headers** | *(leave empty)* | Not needed for OpenAI |
| **Model** | `gpt-4o` | Recommended ($2.50 per million tokens) |

### Other Model Options:
- `gpt-4o-mini` - Cheapest ($0.15/M) ✅ Best for comments
- `gpt-4-turbo` - More powerful ($10/M)
- `gpt-4` - Original ($30/M)

### Success Message:
```
Settings saved. ✓ Connection test successful! API is responding correctly.
```

### Common Error:
```
❌ "401 Unauthorized"
→ Check: Auth Header Prefix is "Bearer" (not empty)
→ Check: You have billing set up at platform.openai.com/account/billing
→ Check: Delete all dots (•••) before pasting new key
```

---

## 🔧 TROUBLESHOOTING QUICK FIXES

### ❌ Connection Test Fails

**For BOTH providers:**
1. Delete ALL dots (•••) in API Key field
2. Paste fresh API key
3. Double-check Auth Header Prefix:
   - Anthropic: **EMPTY**
   - OpenAI: **Bearer**
4. Save and test again

### ❌ "Could not resolve host"
- Your server can't reach the internet
- Contact hosting provider
- Ask them to allow outbound HTTPS to `api.anthropic.com` or `api.openai.com`

### ❌ Moderation Not Working
- Check "Moderate comments" checkbox is enabled (for comments)
- Enable debug logging: Add to `wp-config.php`:
  ```php
  define('WP_DEBUG_LOG', true);
  ```
- Check `/wp-content/debug.log` for errors

### ❌ Error: "Unsupported parameter: 'messages'"
- **Wrong endpoint URL** - Check it's EXACTLY:
  - OpenAI: `https://api.openai.com/v1/chat/completions`
  - Anthropic: `https://api.anthropic.com/v1/messages`
- Common mistakes: `/responses/create` or `/completions` (wrong!)
- No trailing slash at the end

---

## 💰 COST CALCULATOR

### Typical Comment (~100 tokens):
- `claude-haiku-4-5`: $0.0001 each
- `gpt-4o-mini`: $0.00015 each
- `claude-sonnet-4`: $0.0003 each
- `gpt-4o`: $0.00025 each

### Typical Post (~500 tokens):
- `claude-haiku-4-5`: $0.0005 each
- `gpt-4o-mini`: $0.00075 each
- `claude-sonnet-4`: $0.0015 each
- `gpt-4o`: $0.00125 each

### Monthly Estimate (100 posts + 1000 comments):
- `claude-haiku-4-5`: ~$0.15/month
- `gpt-4o-mini`: ~$0.23/month
- `claude-sonnet-4`: ~$0.45/month
- `gpt-4o`: ~$1.38/month

---

## 📊 WHAT DATA IS SENT?

### For Posts:
```
Post Title
Post content goes here...
```
*Only title + content. No author info, dates, or metadata.*

### For Comments:
```
Comment text goes here...
```
*Only comment text. No commenter name, email, or IP.*

---

## ⚙️ KEY SETTINGS EXPLAINED

**Enforce Moderation Result:**
- ☐ OFF: AI moderates once, admins can override
- ☑ ON: AI moderates, then re-checks after 60 seconds to enforce decision (respects admin overrides)

**Moderate Comments:**
- ☐ OFF: Only posts are moderated
- ☑ ON: Both posts and comments are moderated

**Admin Bypass (Automatic):**
- 🔑 Administrators skip all AI moderation (no API cost)
- 🔑 Admins can manually approve AI-rejected content without re-moderation loops
- 🔑 Non-admin edits to admin-approved content trigger fresh AI moderation

---

## 📝 SAMPLE PROMPTS

### Base Prompt (for all content):
```
Reject content containing spam, hate speech, explicit content, 
illegal activity, or personal attacks. Approve appropriate content 
for general audiences.
```

### Extra Rules for Posts:
```
Posts must be at least 50 words.
Reject purely promotional content.
```

### Extra Rules for Comments:
```
Reject comments shorter than 5 words.
Reject comments with only links.
```

---

## 🔗 HELPFUL LINKS

**Get API Keys:**
- Anthropic: https://console.anthropic.com
- OpenAI: https://platform.openai.com/api-keys

**Official API Documentation:**
- Anthropic Messages API: https://docs.anthropic.com/en/api/messages
- OpenAI Chat Completions: https://platform.openai.com/docs/guides/chat-completions
- OpenAI API Reference: https://platform.openai.com/docs/api-reference/chat

**Pricing:**
- Anthropic: https://www.anthropic.com/pricing
- OpenAI: https://openai.com/api/pricing

**Check Usage:**
- Anthropic: https://console.anthropic.com/settings/usage
- OpenAI: https://platform.openai.com/usage

**API Status:**
- Anthropic Status: https://status.anthropic.com
- OpenAI Status: https://status.openai.com

---

## ✅ SETUP CHECKLIST

- [ ] Got API key from provider
- [ ] Pasted key in plugin settings (no dots!)
- [ ] Entered correct endpoint URL
- [ ] Set correct Auth Header Name
- [ ] Set correct Auth Header Prefix (or left empty)
- [ ] Added Extra Headers (if Anthropic)
- [ ] Chose a model
- [ ] Clicked "Save & Test Connection"
- [ ] Saw green success message
- [ ] Configured moderation prompts
- [ ] Enabled "Moderate comments" (if wanted)
- [ ] Decided on "Enforce moderation result"
- [ ] Tested with a sample post/comment

---

**🎉 Done! Your site is now protected by AI moderation!**

---

*Smart Moderator v2.0.0 | wordpress.org/plugins/smart-moderator*
