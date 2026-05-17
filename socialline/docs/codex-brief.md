# SocialLine Codex Project Brief

## Product
SocialLine is a WordPress plugin for automatically posting selected Custom Post Types to social media when posts go live.

## Current migration goal
Move development from ChatGPT-generated ZIP files into a proper GitHub/Codex workflow so future changes are tracked, linted, and packaged reliably.

## Current known stable behavior to preserve
- WordPress plugin named `SocialLine (Pro Test Build)`.
- Current functional version target before next feature work: `1.6.1-pro`.
- Auto-post selected CPTs when status transitions from non-published to `publish`.
- Facebook Page posting works using manual Page ID + Page Access Token.
- Facebook posts are link posts so the Page renders the Open Graph link card.
- Queue/trickle mode schedules posts over time.
- Admin can reset queue slot.
- Admin can requeue the most recent 10 published posts.
- Activity dashboard includes pagination/status filtering/search/bulk queue selected.
- Bulk queue selected should default to NOT force reposting. Force repost should be explicit.
- Settings and history are stored in WordPress DB and should not be removed on deactivate/reinstall.

## Existing data keys to preserve
Options:
- `socialline_settings`
- `socialline_log`
- `socialline_next_slot_ts`

Post meta:
- `_socialline_fb_posted`
- `_socialline_fb_response`
- `_socialline_fb_error`
- `_socialline_skip`

## Near-term feature target: 1.7.0-pro
Add Twitter/X and Instagram without dropping any current features.

### X/Twitter
- Use OAuth 1.0a if compatible with X Free developer tier.
- Add settings for API key, API secret, Access token, Access token secret.
- Add X daily cap select box: 5 or 10 posts per day.
- Default to slow queue/trickle behavior.
- If daily X cap is reached, delay queued X posts until the next day automatically.
- Track X post success/error separately in post meta.

Suggested post meta:
- `_socialline_x_posted`
- `_socialline_x_response`
- `_socialline_x_error`

### Instagram
- Instagram requires Facebook/Meta configuration.
- Requires IG Business or Creator account connected to a Facebook Page.
- Use featured image first.
- Fallback to first image in post content.
- Publish using Instagram Graph API: `/{ig-user-id}/media` then `/{ig-user-id}/media_publish`.
- Track IG post success/error separately in post meta.

Suggested post meta:
- `_socialline_ig_posted`
- `_socialline_ig_response`
- `_socialline_ig_error`

## Build/test requirements
- Do not generate partial plugin snippets.
- Regenerate the entire plugin when making changes.
- Run `php -l` against all PHP files before finalizing.
- Prefer splitting code into includes/classes before adding more platform logic.
- Add a simple package/build script that creates an installable ZIP.
- Never hard-code tokens, secrets, or client/customer credentials.
- Do not add uninstall cleanup yet.

## Recommended stabilization PR before 1.7.0
1. Move single-file plugin into a structured layout:
   - `socialline.php`
   - `includes/class-socialline-plugin.php`
   - `includes/class-socialline-admin.php`
   - `includes/class-socialline-queue.php`
   - `includes/class-socialline-facebook.php`
   - `includes/class-socialline-activity-table.php`
2. Add `bin/build-zip.sh`.
3. Add GitHub Actions workflow for PHP syntax checks.
4. Confirm plugin activates before adding X/Instagram.
