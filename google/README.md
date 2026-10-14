# adm.auth.google

Sign in with Google in ADM over `std.proto.auth.openid`: the web flow, ID tokens from One Tap
and mobile apps, refresh, and Google Workspace domain checks.

```bash
adm get admlang:adm.auth.google
```

```adm
use adm.auth.google

let provider = new google.Provider(clientId, clientSecret, redirectURL = "https://example.com/auth/google")
let start = try provider.begin(scopes = ["email", "profile"])
// send the browser to start.url, keep start.state in the session

let login = try provider.complete(callbackURL, start)
let profile = login.profile
print("{profile.name} <{profile.email}>")

// a token Google Identity Services or a mobile app obtained
let claims = try provider.verifyIdToken(idToken)
```

## Tests

`adm test auth/google` runs against a loopback provider. `ADM_TEST_GOOGLE_ID_TOKEN` and
`ADM_TEST_GOOGLE_CLIENT_ID` add verification of a real token.
