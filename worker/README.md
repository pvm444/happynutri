# HappyNutri Buddy proxy (Cloudflare Worker)

Turns on **Smart mode** (Claude) in HappyNutri Buddy. The website never sees your API key.

## Deploy (about 5 minutes, free Cloudflare plan is fine)
1. Get an Anthropic API key at https://console.anthropic.com and **set a monthly spend limit** there.
2. Install Wrangler and sign in: `npm i -g wrangler && wrangler login`
3. From this folder: `wrangler secret put ANTHROPIC_API_KEY` (paste the key), then `wrangler deploy`
4. Wrangler prints a URL like `https://happynutri-buddy.<you>.workers.dev`.
5. In the app: **Planners → HappyNutri Buddy → ⚙️ Smart mode settings**, paste the URL, Save.
   (To make it permanent for everyone, put the URL in the `happynutri-buddy-api` meta tag in `index.html`.)

## What it protects
- Only the origins in `ALLOWED_ORIGINS` (default: happynutri.in, www, pvm444.github.io, localhost:8000) can call it.
- 20 questions / 10 min / IP (`RATE_LIMIT`); add a `RATE_KV` namespace for a shared limit.
- Caps on message count/length; grounded facts come from the app's own data.
- Child-safety system prompt: no diagnosis, supplements, dieting or weight-loss advice; emergency and self-harm topics are answered on-device and never sent here.

## Notes
- Change the model with `MODEL` in `wrangler.toml`. Cheaper/faster: `claude-haiku-5-5`.
- If the proxy is unreachable or slow (9 s to first byte), Buddy silently falls back to its offline brain.
