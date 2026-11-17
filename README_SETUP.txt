PROFESSOR X ZAMIN HOSTING BOT — CLOUDFLARE WORKER VERSION
=========================================================

FILES
- src/index.js: Telegram webhook + join gate + Pages deployment code
- package.json: JSZip and Wrangler dependencies
- wrangler.jsonc: Worker settings; replace KV namespace ID

IMPORTANT LIMITS / NOTES
- Telegram Bot API getFile download limit is 20 MB. Bot enforces 20 MB, not 25 MB.
- ZIP support uses JSZip. ZIP must contain index.html at root (or one wrapper folder).
- Static HTML/CSS/JS and common static assets only. PHP/Python backends are not supported.
- Up to 100 files, 50 MB extracted content, 5 active sites per Telegram user.
- Bot checks membership in @KINGPROFESSORBACK. Add the bot as an administrator in the channel so getChatMember can verify members reliably.
- Do not publish phishing, malware, impersonation, copyrighted material you do not have rights to, or illegal content.
- Keep all API tokens private. Never put secrets in GitHub source.

GITHUB + CLOUDFLARE DASHBOARD SETUP (PHONE BROWSER)
1. Upload all files from this ZIP to the ROOT of your GitHub repository. Keep src/index.js inside src/.
2. In Cloudflare, create a KV namespace:
   Workers & Pages > Storage & databases > KV > Create namespace
   Name: ZAMIN_SITES
3. Copy the KV namespace ID. Edit wrangler.jsonc in GitHub and replace
   PASTE_YOUR_KV_NAMESPACE_ID_HERE with that ID. Save/commit.
4. Go to Workers & Pages > Create application > Connect GitHub, select this repo.
5. Project settings:
   Build command: npm install
   Deploy command: npx wrangler deploy
   Root directory: /
   Save and Deploy.
6. Once Worker is deployed, open it > Settings > Variables and Secrets.
   Add these as SECRET values:
   BOT_TOKEN       = token from @BotFather
   CF_API_TOKEN    = Cloudflare API token with Account > Cloudflare Pages > Edit
   SETUP_KEY       = a long random password you create
   WEBHOOK_SECRET  = 32-64 random letters/numbers (Telegram webhook secret)
   Add this as a normal variable:
   CF_ACCOUNT_ID   = your Cloudflare account ID
   ADMIN_ID        = your numeric Telegram user ID (optional; reserved for future admin features)
   Select Deploy after saving variables.
7. Add your Telegram bot as an administrator in @KINGPROFESSORBACK.
8. Open the Worker URL in browser to confirm it says Worker is active.
9. Set webhook by visiting:
   https://YOUR-WORKER.workers.dev/set-webhook?key=YOUR_SETUP_KEY
   Replace both placeholders. You should see {"ok":true,...}.
10. Open your Telegram bot and send /start.

CLOUDFLARE TOKEN
Create a custom API token with only:
Account > Cloudflare Pages > Edit
Use Account resources: your account only if possible.
Never send token to anyone.

JOIN CHANNEL
Configured channel username: @KINGPROFESSORBACK
Join URL: https://t.me/KINGPROFESSORBACK

CLEANUP
If an uploaded website is harmful or violates policy, remove it from Cloudflare Pages and block the uploader. Add moderation and rate limits before inviting many users.
