# adm.auth.google

Sign in with Google in ADM over `std.proto.auth.openid`: the web flow, ID tokens from One Tap
and mobile apps, refresh, and Google Workspace domain checks.

```bash
adm get admlang:adm.auth.google
```

```adm
use adm.auth.google

let provider = new google.Provider(clientId, clientSecret, redirectURL = "https://example.com/auth/google")
let request = try provider.start()              // email and profile; more scopes as an argument
// send the browser to request.url, keep request with the session

let login = try provider.exchange(request, callbackURL)
let profile = login.profile
print("{profile.name} <{profile.email}>")

// a token Google Identity Services or a mobile app obtained
let claims = try provider.verifyIdToken(idToken)
```

## Tests

`adm test auth/google` runs against a loopback provider. `ADM_TEST_GOOGLE_ID_TOKEN` and
`ADM_TEST_GOOGLE_CLIENT_ID` add verification of a real token.
