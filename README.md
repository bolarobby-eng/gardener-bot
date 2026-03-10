# 🌿 Gardener Bot

An AI-powered chatbot built in [n8n](https://n8n.io) that handles inbound customer messages for a gardening service. It classifies each message and routes it to the appropriate handler using [Claude](https://anthropic.com) (Anthropic).

## How It Works

```
Customer message (POST webhook)
        │
        ▼
  Classify Message  ←── Claude Haiku (claude-haiku-4-5-20251001)
        │
        ▼
   Route Switch
   /     |     \
  FAQ  Booking  Quote
   │     │       │
Claude  Claude  Claude
Haiku  Haiku  Sonnet
```

### Routes

| Route | Trigger | Model | Handler |
|-------|---------|-------|---------|
| **FAQ** | General questions (hours, insurance, coverage) | Haiku | AI with service knowledge system prompt |
| **Booking** | Requests to schedule a visit | Haiku | AI that reads provided details, asks only for what's missing |
| **Quote** | Price enquiries / garden descriptions | Sonnet | AI with pricing guide system prompt |

## Webhook

```
POST /webhook/793a998f-2e3b-473a-8b68-8720f9e92087
Content-Type: application/json

{"message": "How much to cut a medium lawn?"}
```

### Response

```json
{
  "reply": "For a medium garden, lawn mowing typically costs £60–90...",
  "route": "quote"
}
```

- **`reply`** — the text to send back to the customer
- **`route`** — which branch was triggered (`faq`, `booking`, or `quote`)

## Setup

### Prerequisites
- n8n instance (v2.11+)
- Anthropic API key with access to Claude 4.x models

### Import

1. In n8n, go to **Workflows → Import**
2. Upload `gardener-bot-workflow.json`
3. Add your Anthropic API key as a credential (type: `Anthropic API`)
4. Update the HTTP Request nodes to use your credential
5. Activate the workflow

### Customise

Edit the system prompts inside the `FAQ Call`, `Booking Call`, and `Quote Call` HTTP Request nodes to match your:
- Service area / postcode coverage
- Pricing
- Business hours
- Services offered

## Models Used

- **Classifier:** `claude-haiku-4-5-20251001` (fast, cheap)
- **FAQ:** `claude-haiku-4-5-20251001`
- **Booking:** `claude-haiku-4-5-20251001`
- **Quote:** `claude-sonnet-4-5-20250929` (more thorough reasoning for estimates)

## Next Steps

- [ ] WhatsApp integration via Twilio
- [ ] Calendly booking link in booking reply
- [ ] Postcode coverage check before booking confirmation
