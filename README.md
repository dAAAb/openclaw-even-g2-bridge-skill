# 🕶️ Even Realities G2 × OpenClaw Bridge

Connect [Even Realities G2](https://www.evenrealities.com/) smart glasses to [OpenClaw](https://github.com/openclaw/openclaw) AI agents via a Cloudflare Worker.

**Talk to your AI agent through your glasses.** Same agent, same memory, same tools — just voice instead of typing.

## How It Works

```
G2 Glasses → (voice→text) → Cloudflare Worker → OpenClaw Gateway → Your Agent
                                    ↓                                    ↓
                             G2 display (text)                 Telegram (rich content)
```

- **Chat**: Ask questions, get answers on your glasses display
- **Long tasks**: "Write me a blog post" → G2 says "Working on it..." → result in Telegram
- **Images**: "Draw a lobster" → DALL-E generates → sent to Telegram

## Quick Start

### 1. Enable Gateway HTTP API

```bash
openclaw config set gateway.http.endpoints.chatCompletions.enabled true
openclaw gateway restart
```

### 2. Deploy Worker

```bash
cd scripts/
npx wrangler deploy
```

### 3. Set Secrets

```bash
npx wrangler secret put GATEWAY_URL        # https://your-openclaw-gateway.example.com
npx wrangler secret put GATEWAY_TOKEN      # Your OpenClaw Gateway auth token
npx wrangler secret put G2_TOKEN           # Any token you choose for G2 auth
npx wrangler secret put ANTHROPIC_API_KEY  # Fallback API key

# Optional: Telegram delivery for rich content
npx wrangler secret put TELEGRAM_BOT_TOKEN
npx wrangler secret put TELEGRAM_CHAT_ID

# Optional: Image generation
npx wrangler secret put OPENAI_API_KEY
```

### 4. Configure G2 Glasses

In Even app → Settings → Add Agent:
- **Name**: Your agent name
- **URL**: `https://your-worker.workers.dev`
- **Token**: The `G2_TOKEN` you set

## OpenClaw Skill

This is also an [OpenClaw Skill](https://clawhub.com). Install it to let your agent help with G2 bridge setup and troubleshooting.

## Security

Two-layer token authentication:
```
G2 --[G2_TOKEN]--> Worker --[GATEWAY_TOKEN]--> Gateway
```

Glasses only know `G2_TOKEN`. If lost, change only that token. Gateway credentials stay in Worker secrets.

## Resources

- [Even Realities](https://www.evenrealities.com/)
- [OpenClaw](https://github.com/openclaw/openclaw)
- [Even Hub SDK](https://evenhub.evenrealities.com/)
- [G2 BLE Protocol (community)](https://github.com/i-soxi/even-g2-protocol)
- [Even Realities Discord](https://discord.gg/arDkX3pr)

## License

MIT
