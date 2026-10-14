# ADM sign-in libraries

Official [ADM](https://github.com/admlang/adm) libraries for signing users in with the identity
providers most applications offer. The protocols (OAuth 2.0, OpenID Connect, JOSE) are in
`std.proto.auth`; each library here knows one provider: its endpoints, scopes, credential
format, the quirks of its tokens, and where its profile data comes from. They are written in
ADM, published as `adm.auth.<provider>` and signed by the `admlang` publisher, and live outside
the standard library because providers change these details without notice.

| Library | Provider | Protocol | Notes |
|---|---|---|---|
| [`adm.auth.google`](google/) | Google | OpenID Connect | ID tokens from One Tap and mobile apps verified too; Workspace domain checks |
| [`adm.auth.github`](github/) | GitHub | OAuth 2.0 | no OpenID Connect: the profile comes from the user and emails API; device flow; GitHub Apps |
| [`adm.auth.microsoft`](microsoft/) | Microsoft (Entra ID) | OpenID Connect | tenants, per-tenant issuers, certificates as client credentials, Microsoft Graph |
| [`adm.auth.apple`](apple/) | Apple | OpenID Connect | the client secret is a JWT the library signs and renews; the name arrives once |

## Shared shape

Each library is a `Provider` built from the application's client id and credential, with a
`login` that yields `std.proto.auth.openid`'s `Login` (tokens, the ID token and its claims) and
a `Profile` the same for every provider: `id`, `email`, `emailVerified`, `name`, `picture`,
`locale`, and the provider's raw claims. PKCE and `state` are on for every provider; refresh
tokens renew through the same `Provider`. A program that offers several providers writes one
callback handler and gives it the provider by name.

## Tests

Each suite runs against a loopback provider, so `adm test` needs no account. Signing in for
real needs a browser, so the suites verify real tokens only when given one:

```bash
ADM_TEST_GOOGLE_ID_TOKEN=... adm test auth/google
```
