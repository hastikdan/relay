# Relay

Publisher dashboard for [Relay](https://hastikdan.github.io/relay/) — the AI agent traffic intelligence layer.

Live at: **https://hastikdan.github.io/relay/**

## What it does

- **Overview** — agent request volume, bandwidth saved, cost savings, daily timeseries chart
- **Agents** — which AI systems are crawling your site (OpenAI, Anthropic, Google, Perplexity, etc.)
- **Content** — your most AI-crawled pages ranked by request volume
- **Setup** — step-by-step Cloudflare Worker installation guide with your API key
- **Settings** — license policy, SOM toggle, domain, billing plan

## Stack

Static HTML + vanilla JS. No build step. Deployed via GitHub Pages.

## Local development

Open `new/index.html` in a browser (or serve with any static server):

```bash
npx serve .
# → http://localhost:3000/new/   (login)
# → http://localhost:3000/app/   (dashboard)
```

The app auto-detects `localhost` and points at `http://localhost:8000` instead of the production API.

## Configuration

Two constants to update before deploying to a custom domain:

**`new/index.html`** — replace the Google Client ID:
```js
window.__RELAY_GOOGLE_CLIENT_ID__ = 'your-google-client-id.apps.googleusercontent.com'
```

**Both HTML files** — API URL auto-switches between localhost and production. No changes needed unless you use a custom backend URL.
