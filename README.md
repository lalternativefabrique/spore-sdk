# spore-sdk


SDKs for the [Spore](https://sporee.fr) transactional email API. The generated
clients share the checked-in OpenAPI 3 contract at `openapi.json`.

| Package | Runtime / registry |
| --- | --- |
| `sdk-node` | Node.js and Bun — `@lalternative/spore-sdk` |
| `sdk-python` | Python — `spore-email` |
| `sdk-php` | PHP and Laravel — `lalternative/spore` |
| `sdk-ruby` | Ruby and Rails — `spore-email` |
| `sdk-rust` | Rust — `spore-email` |
| `sdk-go` | Go — `github.com/lalternativefabrique/spore-sdk/sdk-go` |

Spore publishes the contract without authentication at `/openapi.json`.
Refresh it from production, or from a local Spore instance, and regenerate all
generated clients with:

```bash
./refresh-contract.sh
./refresh-contract.sh http://localhost:4110
```

To regenerate without fetching the network, use
`pnpm --filter @lalternative/spore-codegen generate`. The Go client is
maintained by hand and tested against the same HTTP contract separately.

Publish from a clean `main`: `npm publish` in `sdk-node`,
`./publish.sh python` (needs `UV_PUBLISH_TOKEN`) for PyPI, and
`./publish.sh php` for Packagist, which pushes the `sdk-php`
subtree and a `v<version>` tag to `lalternativefabrique/spore-php`, the
repository Packagist reads.

## License

[MIT](LICENSE)
