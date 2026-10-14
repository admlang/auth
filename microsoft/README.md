# adm.auth.microsoft

Sign in with Microsoft in ADM over `std.proto.auth.openid`: Entra ID (work and school
accounts), personal Microsoft accounts, tenants and their issuers, certificates as client
credentials, and the profile from Microsoft Graph.

```bash
adm get admlang:adm.auth.microsoft
```

```adm
use adm.auth.microsoft

let provider = new microsoft.Provider(clientId, clientSecret, tenant = "organizations",
	redirectURL = "https://example.com/auth/microsoft")
let start = try provider.begin(scopes = ["openid", "profile", "email", "User.Read"])
let login = try provider.complete(callbackURL, start)
print(login.profile.email)
print(login.claims.get("tid"))                    // the user's tenant
```

## Tests

`adm test auth/microsoft` runs against a loopback provider. `ADM_TEST_MICROSOFT_ID_TOKEN` and
`ADM_TEST_MICROSOFT_CLIENT_ID` add verification of a real token.
