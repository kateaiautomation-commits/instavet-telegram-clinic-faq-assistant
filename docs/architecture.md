# Architecture Notes

## Event flow

1. A user sends a question to the Telegram bot.
2. Make.com's Telegram **Watch Updates** module receives the message through a webhook.
3. The message text is mapped into a sanitized JSON request body.
4. Make.com sends an authenticated `POST` request to Gemini's `generateContent` endpoint.
5. The HTTP module parses the JSON response.
6. The first generated text response is mapped into Telegram's **Send a Text Message or a Reply** module.
7. The original Telegram Chat ID routes the answer back to the correct user.

## Data mapping

| Source | Destination | Purpose |
|---|---|---|
| Telegram `Message > Text` | Gemini user message | Sends the user's question to the model |
| Telegram `Message > Chat > ID` | Telegram reply `Chat ID` | Returns the reply to the same conversation |
| Gemini `candidates[1] > content > parts[1] > text` | Telegram reply `Text` | Sends the assistant's answer |

## Gemini request

- Method: `POST`
- Body type: `application/json`
- Authentication: API key in a request header
- Response parsing: enabled
- Temperature: `0.2`
- Maximum output tokens: `500`
- Thinking level: `low`

The API key is stored in Make.com's credential manager and is never placed inside the public request template.

## Production considerations

Before using this workflow for a real clinic:

- Replace all demo information with clinic-approved data.
- Add explicit consent and privacy notices where required.
- Minimize stored personal information.
- Add logging that excludes confidential medical details.
- Create a reliable human handoff process.
- Test urgent-message routing with a licensed veterinary professional.
- Add failure alerts, rate limits, and response-time monitoring.

