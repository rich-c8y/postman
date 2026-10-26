# Postman collections

Postman collections, versioned with git using Postman's native Git format (v3 YAML: one file per request).

| Path | What it is |
|---|---|
| `Cumulocity API/` | The Cumulocity REST API collection |
| `postman/` | Workspace globals, plus folders for environments, specs, mocks and flows |
| `.postman/resources.yaml` | Links this repo to the Postman workspace *Rich's Workspace* |

## Everyday workflow

The Postman CLI is at `/Users/rich/.local/bin/postman`.

```bash
# Get the latest from Postman into this repo
postman workspace pull

# Check local changes, preview them against the cloud, then publish
postman workspace lint
postman workspace diff
postman workspace push

# Save the change in git
git add -A && git commit -m "Describe the change"
git push
```

You can also edit in the Postman app and commit from there: open the workspace, go to the **Git** panel, and commit or push.

## Credentials: kept outside this repo

This repo is public, so it holds no tenant names, users, passwords or tokens. Requests use these placeholders:

| Variable | Meaning |
|---|---|
| `{{url}}` | Tenant URL, e.g. `https://example.cumulocity.com` |
| `{{auth}}` | Full `Authorization` header value: `Bearer <token>` or `Basic <base64 tenant/user:password>` |
| `{{tenant}}` | Tenant ID, e.g. `t12345` |

[`postman/environments/Cumulocity.environment.yaml`](postman/environments/Cumulocity.environment.yaml) is an empty template listing them. **Never put values in it**: it is committed and pushed to Postman Cloud.

### From the terminal: go-c8y-cli sessions

The real values live in [go-c8y-cli](https://goc8ycli.netlify.app/) sessions in `~/.cumulocity/`, outside the repo, with the password encrypted.

One-time setup:

```bash
brew install reubenmiller/go-c8y-cli/go-c8y-cli
c8y cli install          # adds set-session to your shell; restart the terminal
c8y sessions create      # enter host, user and password for each tenant
```

Each time:

```bash
set-session                        # choose a tenant (asks for your passphrase)
bin/run                            # run the whole collection
bin/run -i "Get collection of alarms"
bin/run --env-var deviceId=12345   # any other `postman collection run` flags
```

`bin/run` reads the active session (`C8Y_HOST`, `C8Y_TENANT`, `C8Y_TOKEN` or `C8Y_USER`/`C8Y_PASSWORD`). It writes them to a temporary file only you can read, deletes the file when the run ends, and passes `--no-report-events` so run results (which include request headers) are not uploaded to Postman Cloud.

### In the Postman app

The app can't read go-c8y-cli sessions. Use the **Postman Vault** (it stays encrypted on your Mac and is never synced), or select the `Cumulocity` environment and fill in **current values** only. Current values stay local; initial values are synced to the cloud.

## Rules

- **No real credentials in git or in Postman initial values.** See [Credentials](#credentials-kept-outside-this-repo).
- Run `postman workspace lint` before committing.
