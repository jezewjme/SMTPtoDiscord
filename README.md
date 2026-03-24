# SMTPtoDiscord

SMTPtoDiscord is a small Docker-based bridge that captures incoming SMTP emails and forwards them to Discord webhooks.

## How It Works

1. `smtp-bridge` runs Mailpit and accepts SMTP traffic on port `25`.
2. Mailpit stores the email and exposes it through its API at `http://smtp-bridge:8025/api/v1`.
3. `discord-forwarder` polls Mailpit every 10 seconds for new messages.
4. For each new message, the forwarder builds a Discord embed (subject, from, to, date, body text) and posts it to a webhook.
5. If `DISCORD_WEBHOOK_URL_<username>` variables are set, the forwarder routes by recipient username; otherwise it falls back to `DISCORD_WEBHOOK_URL`.

## Quick Configuration

- Set `DISCORD_WEBHOOK_URL` in your environment before starting.
- Optional per-recipient routing:
  - `DISCORD_WEBHOOK_URL_alerts` for emails sent to `alerts@...`
  - `DISCORD_WEBHOOK_URL_support` for emails sent to `support@...`
- Start with your compose file:
  - `docker compose -f smtp-discord-bridge.yml up -d --build`

## Notes

- No webhook URL values are stored in this repo; they are read from environment variables at runtime.
- Mailpit web UI is available on port `8025`.
