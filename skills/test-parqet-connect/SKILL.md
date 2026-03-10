---
name: test-parqet-connect
description: Build, test, or maintain Parqet Connect integrations. Use when implementing OAuth 2.0 authorization-code flow, storing/refreshing tokens, generating clients from the Parqet Connect OpenAPI spec, or syncing portfolios/activities/quotes/performance via the Connect API.
---

# Test Parqet Connect

## Overview

Build Parqet Connect integrations end-to-end: OAuth setup, API client generation, and portfolio/activity sync workflows.

## Workflow

1. Gather integration setup details
2. Implement OAuth 2.0 authorization-code flow
3. Build API client from OpenAPI
4. Implement sync flows
5. Test privately and prepare publish checklist

## 1. Gather integration setup details

- Create an integration in the Developer Console and capture Client ID, scopes, and redirect URIs.
- Choose scopes based on required capabilities (see `references/openapi-notes.md`).
- Determine the Connect API base URL and OAuth endpoints from Parqet docs/Developer Console (not provided in the OpenAPI spec).

## 2. Implement OAuth 2.0 authorization-code flow

- Use an OAuth SDK (Node: `openid-client`, Python: `Authlib`).
- Build an authorization URL with required scopes.
- Handle the redirect, exchange `code` for tokens, and persist tokens per user.
- Implement refresh-token handling and expiry checks.
- Use `state` and PKCE if required by the SDK or integration settings.
- Parqet Connect is OAuth2 (not OIDC). If using `openid-client`, prefer `client.oauthCallback(...)` to avoid `id_token` expectations.
- Check **Credentials Location** in the console. If it is **body**, configure `token_endpoint_auth_method: "client_secret_post"`.
- Redirect URIs must match exactly. For local dev, ensure the console includes your exact callback path (e.g. `/callback` vs `/auth/callback`).
- If you need a standard Parqet OAuth button, use the following markup and styles (replace `client_id`, `redirect_uri`, and PKCE params): 

```html
<style>
.connect-with-parqet {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5em;
  background-color: #009991;
  color: white;
  text-decoration: none;
  cursor: pointer;
  font-weight: 600;
  white-space: nowrap;
  border-radius: 0.375rem;
  border: none;
}

.connect-with-parqet:hover {
  background-color: #5bcec2;
}

.connect-with-parqet:focus-visible {
  outline: 2px solid #009991;
  outline-offset: 2px;
}

.connect-with-parqet--xs {
  padding: 0.25rem 0.5rem;
  font-size: 0.75rem;
}

.connect-with-parqet--sm {
  padding: 0.25rem 0.5rem;
  font-size: 0.875rem;
}

.connect-with-parqet--md {
  padding: 0.5rem 0.75rem;
  font-size: 0.875rem;
}

.connect-with-parqet--lg {
  padding: 0.5rem 1rem;
  font-size: 0.875rem;
}

.connect-with-parqet--xl {
  padding: 0.625rem 1.25rem;
  font-size: 0.875rem;
}

.connect-with-parqet__icon {
  width: 1.6em;
  height: 1.6em;
  margin-block: -0.25em;
  flex-shrink: 0;
}
</style>
<a
  href="https://connect.parqet.com/oauth2/authorize?response_type=code&client_id=YOUR_CLIENT_ID&code_challenge=YOUR_CODE_CHALLENGE&code_challenge_method=S256&state=YOUR_STATE&scope=portfolio:read+portfolio:write&redirect_uri=YOUR_REDIRECT_URI"
  target="_self"
  class="connect-with-parqet connect-with-parqet--md"
>
  <img src="https://developer.parqet.com/img/parqet-icon-trans.svg" alt="" aria-hidden="true" class="connect-with-parqet__icon" />
  Connect with Parqet
</a>
```

## 3. Build API client from OpenAPI

- Prefer generating a typed client from `references/openapi.json` (OpenAPI 3).
- Keep the base URL configurable via environment variables.
- Send `Authorization: Bearer <access_token>` on all requests.
- Use `references/openapi-notes.md` for a concise endpoint list and payload constraints.
- For browser-based apps, expect CORS restrictions. Use a local proxy/backend and keep tokens server-side.

## 4. Implement sync flows

- User & portfolio discovery: `GET /user`, `GET /portfolios`.
- Portfolio creation: `POST /portfolios` when needed.
- Activities sync: map broker transactions to activity variants (`isin`, `crypto_symbol`, `custom_asset`).
- Custom holdings: create holding first, then submit activities or user-managed quotes.
- Respect batch limits (`activities` max 100, `quotes` max 500).
- Use cursor pagination for activity listing.

## 5. Test privately and prepare publish checklist

- Authorize the private integration on your own account for end-to-end testing.
- For publishing requirements, follow `references/quickstart.md`.

## References

- `references/openapi.json` - Parqet Connect OpenAPI spec (from https://developer.parqet.com/api-spec/current.json).
- `references/openapi-notes.md` - Condensed endpoint, payload, and limit notes.
- `references/quickstart.md` - Developer Console steps and publishing checklist.
