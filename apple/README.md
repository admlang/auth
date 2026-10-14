# adm.auth.apple

Sign in with Apple in ADM over `std.proto.auth.openid`: the web and native flows, the client
secret Apple wants as a signed JWT, the user's name that arrives only once, private relay
addresses, and Apple's server-to-server notifications.

```bash
adm get admlang:adm.auth.apple
```

```adm
use adm.auth.apple

let provider = new apple.Provider(servicesId, teamId, keyId, privateKey,
	redirectURL = "https://example.com/auth/apple")
let start = try provider.begin(scopes = ["name", "email"])
let login = try provider.complete(callbackForm, start)   // Apple posts the result as a form
let profile = login.profile
print(profile.name)                               // present on the first sign-in only: store it
print(profile.email)                              // may be a privaterelay.appleid.com address

// a token a native app obtained
let claims = try provider.verifyIdToken(idToken, audience = bundleId)
```

## Tests

`adm test auth/apple` runs against a loopback provider, with a key the suite generates.
`ADM_TEST_APPLE_ID_TOKEN` and `ADM_TEST_APPLE_CLIENT_ID` add verification of a real token.
