# 🌿 Gardener Bot

An AI-powered chatbot for gardening services, built with [n8n](https://n8n.io) and [Claude](https://anthropic.com).

Classifies incoming customer messages into three routes and responds intelligently using Claude AI.

## Routes

- **FAQ** — General questions (services, hours, insurance, coverage) → Claude Haiku answers intelligently
- **Booking** — Wants to schedule a visit → Claude Haiku reads what's provided, asks only for what's missing
- **Quote** — Wants a price estimate → Claude Sonnet gives a detailed breakdown with price ranges

## How It Works

```
POST /webhook/{uuid}
{ "message": "customer message here" }
```

1. **Classify Message** — Claude Haiku reads the message and returns `faq`, `booking`, or `quote`
2. **Route Switch** — n8n routes to the correct handler
3. **Handler** — Claude generates a tailored response using a service-specific system prompt
4. **Response** — Returns `{ reply, route }` JSON

## Response Format

```json
{
  "reply": "The text to send the customer",
  "route": "faq | booking | quote"
}
```

## Models Used

- **Classifier:** `claude-haiku-4-5-20251001`
- **FAQ:** `claude-haiku-4-5-20251001`
- **Booking:** `claude-haiku-4-5-20251001`
- **Quote:** `claude-sonnet-4-5-20250929`

## Setup

1. Import `workflow/gardener-bot.json` into your n8n instance
2. Replace `YOUR_ANTHROPIC_API_KEY_HERE` in the workflow with your real Anthropic API key
3. Activate the workflow
4. Send POST requests to the webhook URL

## Customisation

Edit the system prompts in the HTTP Request node bodies to match your:
- Service area / postcodes covered
- Pricing ranges
- Business hours
- Services offered

## Security Note

The workflow JSON uses `YOUR_ANTHROPIC_API_KEY_HERE` as a placeholder. **Never commit real API keys to version control.**

## Next Steps

- [ ] WhatsApp integration via Twilio
- [ ] Calendly booking link integration
- [ ] Handoff to human agent for complex queries
