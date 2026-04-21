# Gemini CLI Extension Template

A companion repository template that lists an MCP server in the
[Gemini CLI Extension Gallery](https://geminicli.com/extensions) without
touching the original project's repository.

## What this template is for

The Gemini CLI Extension Gallery auto-discovers extensions from public GitHub
repositories that contain a valid `gemini-extension.json` manifest.  If you
own an MCP server but do not want to add that manifest to your main project
repo, fork or generate a new repository from this template and fill in the
placeholders instead.

## How to use this template

### 1. Create a new repository from this template

Click **Use this template → Create a new repository** at the top of this page,
or fork it.  Name the new repo something descriptive, for example
`your-mcp-server-gemini-extension`.

### 2. Edit `gemini-extension.json`

Open `gemini-extension.json` and replace every placeholder value.

| Field | Description |
|---|---|
| `name` | Unique extension identifier (lowercase, hyphens only). Must match the slug the Gallery will index. |
| `version` | Semantic version string (`MAJOR.MINOR.PATCH`). Increment this whenever you ship a change. |
| `description` | One-sentence summary shown in the Gallery listing. |
| `settings` | List of environment variables Gemini CLI will prompt the user to configure at install time. Set `"sensitive": true` for secrets such as API keys or private key paths. Remove the array entirely if your MCP server needs no configuration. |
| `mcpServers` | Map of one or more MCP server definitions. The key becomes the server name inside Gemini CLI. |

**`mcpServers` entry fields:**

| Field | Description |
|---|---|
| `command` | The executable Gemini CLI runs to start the server (e.g. `npx`, `docker`, `uvx`). |
| `args` | Arguments passed to `command`. Use `${YOUR_ENV_VAR_NAME}` to interpolate values the user provided through `settings`. |
| `env` | Additional environment variables to set in the server process. |
| `timeout` | Milliseconds Gemini CLI waits for the server to start (default `60000`). |

### 3. Add or edit slash commands (optional)

Any `.toml` file inside the `commands/` directory becomes a custom slash
command.  The filename (without `.toml`) is the command name.

Each file must have at least a `prompt` key:

```toml
description = "One-line description shown in the /help list."

prompt = """
The instruction text sent to Gemini when the user runs this command.
Reference MCP tools with their fully-qualified name: your_mcp_server/tool_name.
"""
```

Rename or delete `commands/example-command.toml` and add as many command files
as you need.  Remove the `commands/` directory entirely if you do not need
custom commands.

### 4. Push and wait for the Gallery to index your extension

Once your repository is public and `gemini-extension.json` is valid, the
Gemini CLI Extension Gallery will automatically discover and list your
extension.  See the
[official releasing guide](https://geminicli.com/docs/extensions/releasing/)
for the full requirements and any additional steps needed to trigger indexing.

## File reference

```
gemini-extension.json   # Extension manifest — the only required file
commands/               # Optional custom slash commands (one .toml per command)
└── example-command.toml
```

## Tips

- **Versioning:** bump `version` in `gemini-extension.json` every time you
  update the extension so users receive the latest configuration.
- **Sensitive settings:** mark any setting that holds a secret or private key
  path with `"sensitive": true`; Gemini CLI will mask it in the UI.
- **Multiple MCP servers:** add additional entries under `mcpServers` if your
  extension wraps more than one server.
- **Environment variable interpolation:** `${VAR_NAME}` in `args` is replaced
  at runtime with the value the user supplied for the matching `envVar`.

