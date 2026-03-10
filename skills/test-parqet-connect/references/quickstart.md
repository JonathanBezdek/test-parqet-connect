# Parqet Connect Quickstart (Condensed)

## Create and test an integration

1. Sign in to the Parqet Developer Console with your Parqet account.
2. Create a new integration.
   - Add a clear logo and name so users can recognize it.
   - Select the required scopes (read and/or write).
   - Add your OAuth redirect URLs (where users land after authorization).
3. Configure your OAuth client with the Client ID from the integration.
   - Node.js: openid-client
   - Python: Authlib
4. Test with the private integration.
   - New integrations are private by default.
   - You can authorize the integration for your own Parqet account via OAuth.
   - No other Parqet user can install it while it is private.

## Publish an integration

1. Ensure your organization in Settings is up to date.
   - Organization name should match the company or developer responsible for the integration.
2. Contact Parqet to publish: connect@parqet.com.
   - Include your Client ID.
   - Include a short description of what the integration does.
   - Include a step-by-step test guide for the Parqet team.
   - Provide test credentials for your app.
