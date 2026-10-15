# adm.auth.github

Sign in with GitHub in ADM over `std.proto.auth.oauth2`. GitHub has no OpenID Connect, so the
library completes the OAuth flow and reads the profile from the user and emails API. It also
offers the device flow for command-line tools, GitHub App user tokens, and GitHub Enterprise
Server through a base URL.

```bash
adm get admlang:adm.auth.github
```

```adm
use adm.auth.github

let provider = new github.Provider(clientId, clientSecret, redirectURL = "https://example.com/auth/github")
let request = try provider.start()              // read:user and user:email
let login = try provider.exchange(request, callbackURL)
print(login.profile.email)                        // the primary verified address

// a command-line tool
let device = try provider.startDevice(["repo"])
print("Open {device.url} and enter {device.code}")
let signedIn = try device.wait()
```

## Tests

`adm test auth/github` runs against a loopback provider. `ADM_TEST_GITHUB_TOKEN` adds a profile
read with a real token.
