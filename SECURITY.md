# Security

## Never commit credentials

Do not upload or commit any of the following:

- Gemini API keys
- Telegram bot tokens
- Make.com connection exports
- Webhook URLs
- Telegram Chat IDs or personal messages
- Environment files containing secrets

## Make.com blueprint review

Before publishing an exported Make.com blueprint:

1. Open the JSON file in a text editor.
2. Search for `key`, `token`, `secret`, `authorization`, `webhook`, `chat_id`, and `connection`.
3. Remove or replace sensitive values with clear placeholders.
4. Import the sanitized copy into a test scenario.
5. Confirm that it requires the user to create their own connections.

If a credential is accidentally committed, revoke or rotate it immediately. Deleting the visible file is not enough because Git history may still contain the secret.

