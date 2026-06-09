# Codex Commands Cheat Sheet

A practical command reference for OpenAI Codex CLI. This README separates shell commands, interactive slash commands, CLI flags, configuration keys, environment variables, and experimental features so developers can use each one in the right context.

Codex CLI changes over time. Use the official OpenAI Codex CLI docs as the primary source for command availability. Local `codex --help` output is useful for checking the installed version, but it can lag behind the latest CLI release or differ from Codex Desktop behavior.

## Quick Start

Install Codex CLI:

```bash
npm install -g @openai/codex
```

Sign in:

```bash
codex login
```

Start an interactive session:

```bash
codex
```

Run a non-interactive task:

```bash
codex exec "Review this repository and summarize the highest-risk issues"
```

Resume the latest session:

```bash
codex resume --last
```

## Command Types

| Type | Used in | Example |
|---|---|---|
| CLI command | Shell | `codex exec "Fix failing tests"` |
| CLI flag | Shell | `codex --model <model>` |
| Slash command | Interactive Codex TUI | `/status` |
| Config key | `~/.codex/config.toml` | `sandbox_mode = "workspace-write"` |
| Environment variable | Shell environment | `CODEX_HOME=~/.codex-dev` |
| Experimental command | Shell or TUI | `codex mcp-server` |
| Deprecated or legacy item | Shell or TUI | `--full-auto` |

## Installation and Authentication

| Command | What it does | Notes |
|---|---|---|
| `npm install -g @openai/codex` | Installs Codex CLI with npm. | Official install path. |
| `brew install --cask codex` | Installs Codex CLI with Homebrew. | Official macOS install path. |
| `codex login` | Starts the default ChatGPT sign-in flow. | Opens a browser when possible. |
| `codex login --device-auth` | Uses device-code authentication. | Useful for remote or headless environments. |
| `printenv OPENAI_API_KEY \| codex login --with-api-key` | Signs in with an API key from stdin. | Keep API keys out of committed files. |
| `codex login status` | Shows active authentication status. | Exits with status `0` when logged in. |
| `codex logout` | Removes stored Codex credentials. | Clears stored auth. |
| `codex update` | Updates Codex when supported by the installed build. | Version-dependent. Current local help may not list it. |

## Session Commands

| Command | What it does | Notes |
|---|---|---|
| `codex` | Starts an interactive session. | Run from the project root. |
| `codex "Explain this codebase"` | Starts the TUI with an initial prompt. | Useful for analysis sessions. |
| `codex resume` | Opens a picker for saved sessions. | Scoped to the current directory by default. |
| `codex resume --last` | Resumes the most recent session. | Add `--all` to ignore directory scope. |
| `codex resume --include-non-interactive` | Includes non-interactive sessions in the picker and `--last` selection. | Stable in current local CLI help. |
| `codex resume <SESSION_ID>` | Resumes a specific session. | Session IDs appear in status and session files. |
| `codex fork` | Forks a previous session. | Useful when trying a different approach. |
| `codex fork --last` | Forks the latest session. | Keeps the original session intact. |
| `codex exec resume --last "Continue the fix"` | Resumes a non-interactive session with a follow-up prompt. | Useful for automation. |
| `codex doctor` | Checks the installed Codex CLI environment. | Use when diagnosing local setup issues. |

## Project and Workspace Commands

| Command | What it does | Notes |
|---|---|---|
| `codex --cd <path>` | Starts Codex in a specific working directory. | Also available as `-C`. |
| `codex --add-dir <path>` | Grants access to another directory. | Repeatable. Prefer this over disabling the sandbox. |
| `codex --image screenshot.png "Fix this UI"` | Attaches an image to the initial prompt. | Also available as `-i`. |
| `codex completion zsh` | Prints shell completions. | Supports `bash`, `zsh`, `fish`, `powershell`, and `elvish`. |
| `codex app` | Opens Codex Desktop from the terminal. | Listed in current official docs; local CLI behavior may vary by install. |
| `codex app <PATH>` | Opens a workspace path in Codex Desktop. | Listed in current official docs. |
| `codex app --download-url` | Prints the Codex Desktop download URL. | Listed in current official docs. |

## Interactive Slash Commands

Slash commands work inside the interactive Codex TUI. They are not shell commands.

| Command | What it does | Notes |
|---|---|---|
| `/status` | Shows model, approval policy, sandbox, writable roots, token usage, and session details. | First command to run when diagnosing a session. |
| `/model` | Changes model and reasoning effort when available. | TUI-only. |
| `/ide` | Manages IDE integration when available. | TUI-only and feature-dependent. |
| `/goal` | Sets or views an experimental goal for a long-running task. | Official slash command. Requires `features.goals`. |
| `/permissions` | Changes approval behavior during a session. | TUI-only. |
| `/approve` | Approves a pending action when a session is waiting for approval. | TUI-only. |
| `/raw` | Sends raw input without Codex command parsing. | TUI-only. |
| `/diff` | Shows current Git diff, including untracked files. | Useful before review or commit. |
| `/review` | Reviews the current working tree. | Focuses on bugs, regressions, and missing tests. |
| `/resume` | Opens a saved-session picker. | TUI equivalent of `codex resume`. |
| `/new` | Starts a new conversation. | Keeps the CLI open. |
| `/clear` | Clears the terminal and starts a fresh chat. | Different from only clearing the screen. |
| `/compact` | Summarizes earlier context to reduce context usage. | Useful during long sessions. |
| `/mention <path>` | Adds a file or folder to the conversation. | Helps focus Codex on specific code. |
| `/init` | Generates an `AGENTS.md` scaffold. | Review before committing. |
| `/mcp` | Lists available MCP tools. | Add `verbose` for more detail. |
| `/statusline` | Configures TUI footer fields. | Persists to `tui.status_line`. |
| `/title` | Configures terminal title fields. | Persists to `tui.terminal_title`. |
| `/keymap` | Remaps TUI keyboard shortcuts. | Persists to `tui.keymap`. |
| `/theme` | Changes the TUI color theme. | TUI-only. |
| `/vim` | Toggles Vim keybindings. | TUI-only. |
| `/memories` | Opens memory management when available. | Feature-dependent. |
| `/skills` | Opens installed skills when available. | Feature-dependent. |
| `/hooks` | Opens hook configuration when available. | Feature-dependent. |
| `/agent` | Creates, switches, lists, or manages delegated agents when available. | Feature-dependent. |
| `/ps` | Lists background terminals or tasks when available. | Feature-dependent. |
| `/stop` | Stops running tasks when available. | Feature-dependent. |
| `/logout` | Signs out of Codex from inside the TUI. | TUI-only. |
| `/feedback` | Opens feedback flow from inside the TUI. | TUI-only. |
| `/exit` | Exits Codex. | Alias of `/quit`. |

## Code Editing and Review

| Command | What it does | Notes |
|---|---|---|
| `codex exec "Run tests and fix failures"` | Runs Codex non-interactively until the task finishes. | Good for scripted repair tasks. |
| `codex exec review` | Runs a review through the `codex exec` command tree. | Stable in current local CLI help. |
| `codex review` | Runs a non-interactive code review. | Stable in current local CLI help. |
| `codex review --uncommitted` | Reviews staged, unstaged, and untracked changes. | Useful before committing. |
| `codex review --base main` | Reviews changes against a base branch. | Useful for PR review. |
| `codex review --commit <SHA>` | Reviews one commit. | Useful for release or regression checks. |
| `codex review --title "Title"` | Sets an optional title in the review summary. | Stable in current local CLI help. |
| `codex exec --output-last-message result.md "Summarize the patch"` | Writes the final response to a file. | Good for CI output. |
| `codex exec --output-schema schema.json "Return JSON"` | Validates final output against a JSON Schema. | Use for structured automation. |
| `codex apply <TASK_ID>` | Applies the latest diff from a Codex Cloud task. | Requires task access. Alias: `codex a`. |

## Model and Mode Controls

| Command | What it does | Notes |
|---|---|---|
| `codex --model <model>` | Overrides the model for one interactive session. | Also available as `-m`. |
| `codex exec --model <model> "Task"` | Overrides the model for one non-interactive run. | Useful for task-specific model choice. |
| `codex --profile <name>` | Loads a named config profile. | Also available as `-p`. |
| `codex --oss` | Uses a local open source provider. | Requires local provider setup. |
| `codex features list` | Lists feature flags and effective state. | Official command. |
| `codex features enable <feature>` | Enables a feature flag persistently. | Writes to config. |
| `codex features enable goals` | Enables the experimental goals feature when supported. | Required before `/goal` is available. |
| `codex features disable <feature>` | Disables a feature flag persistently. | Writes to config. |

Useful config keys:

```toml
model = "<model>"
model_reasoning_effort = "medium"
model_reasoning_summary = "auto"
model_verbosity = "medium"
model_provider = "openai"
review_model = "<model>"
```

## Permissions and Sandbox

| Command | What it does | Notes |
|---|---|---|
| `codex --sandbox read-only` | Prevents file edits. | Good for audits and explanations. |
| `codex --sandbox workspace-write` | Allows edits inside the workspace. | Common development mode. |
| `codex --sandbox danger-full-access` | Disables sandbox restrictions. | Use only in an isolated environment. |
| `codex --ask-for-approval on-request` | Lets Codex ask when it needs higher-permission action. | Practical interactive default. |
| `codex --ask-for-approval never` | Prevents approval prompts. | Use with a safe sandbox for automation. |
| `codex --add-dir <path>` | Grants access to another directory. | Prefer this over disabling the sandbox. |
| `codex --dangerously-bypass-approvals-and-sandbox` | Runs without approvals or sandboxing. | Dangerous outside a hardened container or VM. |
| `codex --yolo` | Alias for bypassing approvals and sandboxing. | Same risk as the full bypass flag. |
| `codex --full-auto` | Runs with low-friction sandboxed automatic execution in builds that support it. | Prefer explicit sandbox and approval flags in new scripts. |
| `/sandbox-add-read-dir C:\absolute\path` | Grants read access to another directory during a Windows TUI session. | Windows-only slash command. |

Useful config keys:

```toml
sandbox_mode = "workspace-write"
approval_policy = "on-request"

[sandbox_workspace_write]
writable_roots = ["/path/to/extra/workspace"]
network_access = false
```

## MCP and Integrations

| Command | What it does | Notes |
|---|---|---|
| `codex mcp list` | Lists configured MCP servers. | Add `--json` for machine-readable output. |
| `codex mcp get <name>` | Shows one MCP server config. | Useful for debugging. |
| `codex mcp add <name> -- <command...>` | Adds a stdio MCP server. | Command after `--` starts the server. |
| `codex mcp add <name> --url https://...` | Adds a streamable HTTP MCP server. | Use token env vars for secrets. |
| `codex mcp login <name>` | Starts OAuth login for an HTTP MCP server. | Server must support OAuth. |
| `codex mcp login <name> --scopes scope1,scope2` | Requests specific OAuth scopes. | Experimental MCP flow. |
| `codex mcp logout <name>` | Removes stored OAuth credentials. | HTTP MCP servers only. |
| `codex mcp remove <name>` | Removes an MCP server definition. | Does not remove external server code. |
| `codex mcp-server` | Runs Codex as an MCP server over stdio. | Experimental. |

Example:

```bash
codex mcp add docs -- node ./mcp-docs-server.js
codex mcp list
```

## Common CLI Flags

| Flag | What it does | Notes |
|---|---|---|
| `--cd, -C <path>` | Sets the working directory. | Use before the task starts. |
| `--config, -c key=value` | Overrides config for one run. | Values parse as TOML when possible. |
| `--enable <feature>` | Enables a feature flag for one run. | Repeatable. Equivalent to `-c features.<name>=true`. |
| `--disable <feature>` | Disables a feature flag for one run. | Repeatable. Equivalent to `-c features.<name>=false`. |
| `--model, -m <model>` | Overrides the model. | Global flag. |
| `--profile, -p <name>` | Loads a config profile. | Global flag. |
| `--sandbox, -s <mode>` | Sets sandbox policy. | `read-only`, `workspace-write`, or `danger-full-access`. |
| `--ask-for-approval, -a <policy>` | Sets approval behavior. | `untrusted`, `on-request`, or `never`. |
| `--image, -i <path>` | Attaches images to the initial prompt. | Repeatable. |
| `--add-dir <path>` | Adds an accessible directory. | Repeatable. |
| `--search` | Enables live web search. | Uses native web search. |
| `--no-alt-screen` | Keeps terminal scrollback visible. | Useful in terminal multiplexers. |
| `--json` | Prints JSONL events in `codex exec`. | Automation-friendly. |
| `--output-last-message <file>` | Writes the final message to a file. | `codex exec` only. |
| `--output-schema <file>` | Validates the final response shape. | `codex exec` only. |
| `--local-provider <provider>` | Selects a local OSS provider. | Current local help lists `lmstudio` and `ollama`. |
| `--remote <ws://host:port>` | Connects the TUI to a remote app-server endpoint. | Used with remote app-server workflows. |
| `--remote-auth-token-env <ENV_VAR>` | Sends a bearer token to a remote app-server endpoint. | Requires `--remote`. |
| `--yolo` | Alias for `--dangerously-bypass-approvals-and-sandbox`. | Dangerous outside a hardened container or VM. |

## Environment Variables

| Variable | What it does | Notes |
|---|---|---|
| `OPENAI_API_KEY` | Provides an OpenAI API key. | Use with `codex login --with-api-key`. |
| `CODEX_HOME` | Changes Codex config, auth, logs, and session storage. | Defaults to `~/.codex`. |
| `CODEX_CA_CERTIFICATE` | Points Codex at a custom CA bundle. | Falls back to `SSL_CERT_FILE` when unset. |
| `SSL_CERT_FILE` | Fallback custom CA path. | Useful behind enterprise TLS inspection. |
| `RUST_LOG` | Controls Rust logging verbosity. | Useful for debugging. |
| `CODEX_REMOTE_AUTH_TOKEN` | Example bearer-token variable for remote TUI auth. | Pass the variable name with `--remote-auth-token-env`. |
| Custom MCP token env vars | Supplies MCP bearer tokens. | Referenced by `bearer_token_env_var`. |

## Real Workflows

Start a new project:

```bash
mkdir my-app
cd my-app
git init
codex --sandbox workspace-write --ask-for-approval on-request "Create a small starter app with tests"
```

Explain an existing repository without edits:

```bash
codex --sandbox read-only "Explain the architecture and identify the main entry points"
```

Fix a failing test:

```bash
codex exec --sandbox workspace-write --ask-for-approval never "Run the relevant tests, fix the failure, and summarize the patch"
```

Review a pull request locally:

```bash
git fetch origin
git checkout feature-branch
codex review --base main
```

Run a review through `codex exec`:

```bash
codex exec review --uncommitted --json
```

Resume previous work:

```bash
codex resume --last
```

Run headless and save the final response:

```bash
codex exec --json --output-last-message codex-result.md "Audit this package and report actionable issues"
```

Use one extra directory safely:

```bash
codex --sandbox workspace-write --add-dir ../shared "Update this project and the shared package together"
```

Reduce context during a long TUI session:

```text
/status
/compact
/status
```

Set a long-running goal:

```bash
codex features enable goals
codex
```

```text
/goal ship the parser refactor without changing public behavior
```

Use this when a longer task needs a persistent target that Codex can track across the session.

## Experimental, Legacy, and Version-Dependent Items

| Item | Status | Notes |
|---|---|---|
| `codex cloud` | Experimental | Requires Codex Cloud access. |
| `codex cloud exec --env <ENV_ID> "Task"` | Experimental | Submits a new Codex Cloud task from the shell. |
| `codex cloud list --json` | Experimental | Lists cloud tasks in machine-readable form. |
| `codex cloud status <TASK_ID>` | Experimental | Shows task status. |
| `codex cloud diff <TASK_ID>` | Experimental | Shows a unified task diff. |
| `codex cloud apply <TASK_ID>` | Experimental | Applies a cloud task locally. |
| `codex app-server` | Experimental | Local development and debugging command. |
| `codex app-server generate-ts` | Experimental | Generates app-server TypeScript bindings. |
| `codex app-server generate-json-schema` | Experimental | Generates app-server JSON Schema. |
| `codex app-server remote-control` | Experimental | Runs remote-control app-server tooling. |
| `codex mcp-server` | Experimental | Runs Codex as an MCP server. |
| `codex sandbox` | Experimental | OS-specific sandbox helpers. |
| `codex execpolicy` | Preview | Tests execpolicy rules. |
| `codex debug app-server send-message-v2` | Debug tooling | Sends an app-server protocol message for debugging. |
| `codex debug models --bundled` | Debug tooling | Lists bundled model definitions. |
| `codex plugin marketplace` | Experimental | Manages plugin marketplace sources. |
| `codex cloud-tasks` | Deprecated alias | Prefer `codex cloud`. |
| `/apps`, `/plugins`, `/skills` | Feature-dependent | Availability depends on installed connectors, plugins, or skills. |
| `/goal` | Experimental | Official slash command. Requires `features.goals`; enable it with `/experimental` or `codex features enable goals`. |
| `/approvals` | Legacy alias | Prefer `/permissions`. |
| `/clean` | Legacy alias | Prefer `/stop`. |
| `--full-auto` | Compatibility/deprecated depending on docs/version | Prefer explicit `--sandbox` and approval flags. |
| `--yolo` | Dangerous compatibility alias | Same risk profile as `--dangerously-bypass-approvals-and-sandbox`. |
| `--experimental-json` | Alias | Prefer `--json`. |
| `codex --upgrade` | Older help content | Treat as outdated unless `codex --help` for the installed version confirms it. |
| `codex update` | Version-dependent update command | Use only when the installed build or official docs list it. |

## Related Resources

- [Codex CLI reference](https://developers.openai.com/codex/cli/reference)
- [Codex slash commands](https://developers.openai.com/codex/cli/slash-commands)
- [Codex configuration reference](https://developers.openai.com/codex/config-reference)
- [Codex authentication](https://developers.openai.com/codex/auth)
- [OpenAI Codex GitHub repository](https://github.com/openai/codex)
- [Model Context Protocol documentation](https://modelcontextprotocol.io/)

## Suggested Repository Metadata

Recommended repository name:

```text
codex-commands-cheat-sheet
```

Recommended GitHub description:

```text
OpenAI Codex CLI commands cheat sheet for developers: slash commands, flags, config, sandboxing, MCP, and workflows.
```

Recommended topics:

```text
codex, openai, codex-cli, cli, developer-tools, cheat-sheet, mcp, ai-coding
```

## Update Policy

- Verify unstable commands against the official Codex CLI reference.
- Treat official OpenAI Codex CLI docs as the primary source.
- Use `codex --help` and subcommand help only as local installed-version checks.
- Keep shell commands, slash commands, flags, config keys, and environment variables in separate sections.
- Mark experimental, deprecated, legacy, community, or unverified commands clearly.
