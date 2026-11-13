# Cortexa Developers

Public developer documentation for the Cortexa Scribe API, built with Mintlify.

## Preview locally

Use Node.js 22 LTS or 24 LTS. From this repository's root:

```bash
npx --yes mint@4.3.23 dev
```

Open the local URL printed by the CLI. To use another port:

```bash
npx --yes mint@4.3.23 dev --port 3100
```

## Check a change

```bash
npx --yes mint@4.3.23 validate
npx --yes mint@4.3.23 broken-links
```

Create a feature branch and open a pull request targeting `main`. Review the
preview before merging. Avoid pushing documentation changes directly to `main`.

## Content

| File | Purpose |
| --- | --- |
| `index.mdx` | Product overview, sandbox access, and resource ownership |
| `authentication.mdx` | Clinician consent, token exchange, scopes, and revocation |
| `quickstart.mdx` | First note with cURL and synthetic data |
| `recordings.mdx` | Complete recordings and segmented transcription |
| `workflows.mdx` | Documents, Ask AI, coding, attachments, and patient links |
| `capabilities.mdx` | Supported API workflows and integration scope |
| `errors.mdx` | Errors, request limits, retries, and pagination |
| `downloads.mdx` | OpenAPI and Postman usage |
| `openapi.json` | Generated partner contract for the API reference |
| `postman.json` | Generated request collection with empty credential variables |
| `docs.json` | Branding, navigation, API reference, and redirects |
| `logo/`, `favicon.svg` | Cortexa branding assets |

Edit partner-facing guides in this repository. The API implementation and contract
exporter remain in `Cortexa-AI/cortexa-server`. Internal deployment and administrator
runbooks belong in the server repository.

## Refresh the API contract

From a configured `cortexa-server` checkout, with the two repositories next to each
other, run:

```bash
python -m scripts.export_scribe_open_api \
  --base-url https://sandbox.api.cortexanote.com \
  --output-dir ../cortexa-developers
```

The command regenerates only `openapi.json` and `postman.json`. Preserve those files
as generated artifacts; change request or response definitions in the backend.
Export only the partner contract, not the internal `/v2/openapi.json`.

When operations change, update the API reference groups in `docs.json` and the
capabilities guide. Navigation lists each operation explicitly, so new operations
must be added to the appropriate group. Validate the docs and review the generated
contract diff in the same documentation PR.

The OpenAPI server and Postman `base_url` contain the origin only. Their paths
already include `/v2/scribe/open-api`; do not append the prefix to either base URL.

## Publishing handoff

The repository is ready for a Mintlify project with:

- GitHub repository: `Cortexa-AI/cortexa-developers`
- Production branch: `main`, after the documentation PR is merged
- Documentation directory: repository root
- Configuration file: `docs.json`

The project owner configures the Mintlify connection and custom domain. Local
validation does not establish that the hosted site or the API deployment is live.

API reference pages initially show request examples in simple mode. To enable
browser requests after the documentation domain is configured, allow that origin
in the API's CORS configuration and set `api.playground` in `docs.json` to:

```json
{"display": "interactive", "proxy": false}
```

Coordinate partner credentials, the clinician consent entry point, and production
access with the API owner. These guides document the partner contract; they do not
create partner accounts or deploy the consent flow.
