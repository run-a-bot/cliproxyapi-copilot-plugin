# patch.md

## meta

- name: run-a-bot-cliproxyapi-copilot-plugin
- target: cliproxyapi-copilot-plugin Go fork
- version: 1.0.0

## commits

### 1. Modernize plugin for CLIProxyAPI v8 and schema version 6

#### background

The upstream plugin was pinned to CLIProxyAPI v7 (`v7.2.118`) with plugin ABI schema version 2.
CLIProxyAPI has since evolved to v8, which introduces schema version 6 (enabling stream payload
memory optimizations and raw JSON preservation for management responses), unified v8 OAuth
endpoints (`/v8/management/oauth/auth-url?provider=copilot`), and updated Copilot client headers.
This patch updates dependencies to `github.com/router-for-me/CLIProxyAPI/v8`, upgrades to
`pluginabi.SchemaVersion` (v6), refreshes Copilot client headers, points git remotes to the
`run-a-bot` fork, modernizes the configuration template to the v8 layout, and adds `patch.md`
for rebase tracking.

#### files

##### go.mod (modify)

Update module path to `github.com/run-a-bot/cliproxyapi-copilot-plugin` and replace
`github.com/router-for-me/CLIProxyAPI/v7 v7.2.118` with `github.com/router-for-me/CLIProxyAPI/v8 v8.0.27`.

##### cmd/cliproxyapi-copilot/main.go (modify)

Update import paths from `CLIProxyAPI/v7` to `CLIProxyAPI/v8`.

##### cmd/cliproxyapi-copilot/host.go (modify)

Update import paths from `CLIProxyAPI/v7` and `arthur-sommer-etc` to `CLIProxyAPI/v8` and `run-a-bot`.

##### cmd/cliproxyapi-copilot/dispatch.go (modify)

Update import paths to `CLIProxyAPI/v8` and `run-a-bot`. `pluginabi.SchemaVersion` now
evaluates to schema version 6 under the v8 SDK. Update repository metadata URL.

##### internal/provider/executor.go (modify)

Update import paths to `CLIProxyAPI/v8` and `run-a-bot`. Refresh Copilot client headers
(`GitHubCopilotChat/0.38.0`, `copilot-chat/0.38.0`).

##### internal/provider/executor_test.go (modify)

Update import paths to `CLIProxyAPI/v8` and `run-a-bot`.

##### internal/provider/models.go (modify)

Update import paths to `CLIProxyAPI/v8` and `run-a-bot`.

##### internal/provider/models_test.go (modify)

Update import paths to `run-a-bot`.

##### internal/provider/oauth.go (modify)

Update import paths to `CLIProxyAPI/v8` and `run-a-bot`.

##### internal/provider/oauth_test.go (modify)

Update import paths to `CLIProxyAPI/v8` and `run-a-bot`.

##### internal/provider/service.go (modify)

Update import paths to `run-a-bot`.

##### internal/provider/storage.go (modify)

Update import paths to `CLIProxyAPI/v8` and `run-a-bot`.

##### internal/provider/token.go (modify)

Update import paths to `run-a-bot`.

##### internal/provider/token_count.go (modify)

Update import paths to `CLIProxyAPI/v8` and `run-a-bot`.

##### internal/translate/translate.go (modify)

Update import paths from `CLIProxyAPI/v7` to `CLIProxyAPI/v8`.

##### config/config.yaml (modify)

Update configuration template to the v8 nested layout (`server:`, `management:`, `access:`, `routing:`).

##### docker-compose.yml (modify)

Update image tag to `eceasy/cli-proxy-api:v8.0.27`.

##### scripts/package-release.sh (modify)

Use `SCRIPT_DIR="$(realpath "$(dirname "$0")")"` for POSIX-compliant script absolute path resolution.

##### README.md, docs/claude-code-setup.md, docs/install-existing-deployment.md (modify)

Update repository URLs, documentation, and version references to CLIProxyAPI v8.

##### patch.md (create)

Track fork-specific modifications and rebase instructions.

#### verify

- `go test ./...` in the plugin repository passes all unit tests.
- Plugin compilation with `CGO_ENABLED=1 go build -buildmode=c-shared` succeeds.
- Remotes point `origin` to `https://github.com/run-a-bot/cliproxyapi-copilot-plugin` and `upstream` to `https://github.com/arthur-sommer-etc/cliproxyapi-copilot-plugin.git`.
