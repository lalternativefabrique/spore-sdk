# spore (Go SDK)

Minimal Go client for the [Spore](https://sporee.fr) transactional email API.

The TypeScript, Python, PHP, Ruby and Rust SDKs live next to this module under
`sdk-*`. This Go client remains intentionally small and hand-written
(stdlib only), so any Go service can use it without inheriting heavy dependencies.

## Install

```sh
go get github.com/lalternativefabrique/spore-sdk/sdk-go@latest
```

## Usage

```go
import spore "github.com/lalternativefabrique/spore-sdk/sdk-go"

client := spore.NewClient(os.Getenv("SPORE_API_KEY"))

res, err := client.SendEmail(ctx, spore.SendEmailRequest{
    From:    "hello@example.com",
    To:      []string{"alice@example.com"},
    Subject: "Hello",
    HTML:    "<p>Hi!</p>",
}, spore.WithIdempotencyKey(uuid.NewString()))
```

The server resolves the identity from the domain of `From`. `IdentityID` is
optional and deprecated — set it only if you need to pin a send to a specific
identity (legacy code, debugging).

### Options

- `WithBaseURL(url)` — override the API base URL (default `https://api.sporee.fr`).
- `WithHTTPClient(c)` — inject a custom `*http.Client` (retry, telemetry, timeout).
- `WithIdempotencyKey(k)` — set the `Idempotency-Key` header on a single send.

### Errors

Non-2xx responses are returned as `*spore.APIError`:

```go
if apiErr, ok := spore.IsAPIError(err); ok {
    log.Printf("status=%d body=%s", apiErr.StatusCode, apiErr.Body)
}
```

## Scope

Currently covers `POST /emails`. Other endpoints (identities, addresses,
suppressions, `GET /emails/:id`) can be added as needed — the API surface
mirrors the OpenAPI spec in the Spore monorepo at `apps/core/docs/swagger.yaml`.

## Release

Tag the module path explicitly:

```sh
git tag sdk-go/v0.1.0
git push origin sdk-go/v0.1.0
```

Consumers then pin with `go get github.com/lalternativefabrique/spore-sdk/sdk-go@v0.1.0`.
