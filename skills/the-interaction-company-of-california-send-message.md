---
name: Send a message to Poke
description: >-
  Send an instruction or question to Poke programmatically via the Poke API so
  Poke can act on it with access to the user's email, calendar, reminders, and
  connected integrations.
api: openapi/the-interaction-company-of-california-poke-openapi.yml
operations:
  - sendInboundMessage
generated: '2026-07-21'
method: generated
source: https://poke.com/docs/api
---

# Send a message to Poke

Use this skill to deliver a message to Poke from an external tool (desktop app,
CI/CD job, webhook, or a bridging service that has no native Poke integration).

## Prerequisites

- A **V2 API key** generated in Kitchen (not a legacy `pk_`-prefixed key from app
  Settings — those only work with the deprecated inbound-sms webhook).

## Steps

1. **Authenticate.** Send the key as a bearer token:
   `Authorization: Bearer YOUR_API_KEY`.
2. **Call `sendInboundMessage`** — `POST https://poke.com/api/v1/inbound/api-message`
   with a JSON body. The entire body is forwarded to the agent as context; the
   documented convention is a single `message` field:

   ```json
   { "message": "Your instruction or question here" }
   ```

3. **Check the result.** A success looks like:

   ```json
   { "success": true, "message": "Message sent successfully" }
   ```

   Treat `success: false` or a non-2xx status as a failure. A `401` means the
   bearer key is missing or invalid.

## Rules

- Do **not** use `POST /api/v1/inbound-sms/webhook` — it is deprecated and only
  accepts legacy V1 keys. Use `sendInboundMessage`.
- The API documents **no idempotency key**, so avoid blind retries that could
  deliver the same instruction twice; confirm delivery via the response instead.
