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

## Rules

- **No real credentials in git.** Collection variables such as `auth` stay as placeholders. Keep real values in Postman's *current value*, the Postman Vault, or a `*.local.yaml` file (these are gitignored).
- Run `postman workspace lint` before committing.
- Run a collection locally: `postman collection run "Cumulocity API" -e postman/environments/<env>.yaml`
