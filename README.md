# Codex Commands Cheat Sheet

A practical command reference for OpenAI Codex CLI. This README separates shell commands, interactive slash commands, TUI shortcuts, CLI flags, configuration keys, environment variables, and integration APIs so each item is used in the right place.

Last audited: **August 21, 2026**

Latest stable release checked: **v0.148.0**

## Recent Stable Release Highlights

Stable releases after v0.140.0 added session controls, plugin workflows, remote execution, migration tools, and new integrations. Pre-release builds are excluded from this summary.

| Release | Highlights |
|---|---|
| `v0.141.0` | Encrypted Noise relays for remote executors, per-thread executor-plugin MCP servers, and richer app-server thread and rate-limit controls. |
| `v0.142.0` | Usage-reset credits, categorized plugin discovery and recommendations, rollout token budgets, indexed web search, and configurable multi-agent delegation. |
| `v0.143.0` | `codex remote-control pair`, remote plugins enabled by default, system proxy support, MCP tool search, and GPT-5.6 Bedrock models. |
| `v0.144.0–v0.144.6` | Reliability and security fixes for Windows sandboxing, Code Mode, WebSockets, authentication, and review workflows. |
| `v0.145.0` | Experimental paginated history, expanded `/import` support for Cursor and Claude Code, Bedrock login options, audio and realtime V3, and stronger multi-agent V2 support. |
| `v0.146.0` | Named `/new` and `/clear` chats, side-chat and paginated-fork improvements, Agent Plugins, remote Code Mode hosts, and custom-provider web search. |
| `v0.148.0` | `/export`, `codex exec fork`, resume-picker archive and restore, cost estimates in `/status`, built-in Bedrock Runtime, and asynchronous command and MCP hooks. |

## How to Read This Cheat Sheet

Codex has several command surfaces. Use the section that matches where the item is typed.

| Type | Where to use it |
|---|---|
| CLI command | Type in your shell, such as `codex exec "Fix failing tests"`. |
| Slash command | Type inside the interactive Codex TUI, such as `/status`. |
| TUI shortcut | Type inside the TUI composer, such as `@`. |
| CLI flag | Add after `codex` or a subcommand, such as `--model`. |
| Config key | Save in `~/.codex/config.toml`. |
| Environment variable | Set in the shell before running Codex. |
| Integration API | Use through app-server or another integration client. |

## CLI Commands

Use these from a terminal prompt before or instead of opening the interactive TUI.

| Command | Use |
|---|---|
| `npm install -g @openai/codex` | Install Codex CLI with npm. |
| `brew install --cask codex` | Install Codex CLI with Homebrew on macOS. |
| `codex` | Open the interactive TUI in the current project. |
| `codex "Explain this codebase"` | Open the TUI with an initial prompt. |
| `codex login` | Start the default ChatGPT sign-in flow. |
| `codex login --device-auth` | Use device-code auth for headless or remote environments. |
| `printenv OPENAI_API_KEY \| codex login --with-api-key` | Sign in with an API key from stdin. |
| `codex login status` | Check the active authentication mode. |
| `codex logout` | Remove stored Codex credentials. |
| `codex update` | Apply an update when the installed build supports self-update. |
| `codex resume` | Open a picker for saved sessions. |
| `codex resume --last` | Resume the most recent saved session. |
| `codex resume <SESSION_ID>` | Resume a specific saved session. |
| `codex fork` | Fork a previous session into a new thread. |
| `codex archive` | Archive a saved session. |
| `codex remote-control pair` | Print a short-lived manual pairing code for a running remote-control daemon. |
| `codex unarchive` | Restore an archived session. |
| `codex delete` | Permanently delete a saved session after confirmation. |
| `codex exec "Task"` | Run a non-interactive task. |
| `codex exec -` | Read the prompt from stdin. |
| `codex exec resume --last "Task"` | Continue the last non-interactive run. |
| `codex exec fork` | Fork a saved non-interactive session into a new session while preserving the original. |
| `codex review` | Run a non-interactive code review. |
| `codex apply <TASK_ID>` | Apply the latest diff from a Codex Cloud task. |
| `codex doctor` | Create a local diagnostic report. |
| `codex app` | Open Codex Desktop from the terminal. |
| `codex app [PATH]` | Launch Codex Desktop. macOS can open a workspace path; Windows prints the path to open. |
| `codex app --download-url` | Print the Codex Desktop download URL. |
| `codex completion <shell>` | Print shell completions for Bash, Zsh, Fish, PowerShell, or Elvish. |

## Cloud, MCP, and Plugin CLI Commands

| Command | Use |
|---|---|
| `codex cloud` or `codex cloud-tasks` | Open a Codex Cloud task picker. |
| `codex cloud exec --env <ENV_ID> [--attempts <1-4>] "Task"` | Submit a cloud task directly with an optional retry limit. |
| `codex cloud list [--limit <N>] [--cursor <CURSOR>] [--env <ENV_ID>] --json` | List recent cloud tasks with JSON output and optional pagination or environment filtering. |
| `codex cloud status <TASK_ID>` | Legacy syntax. Use the cloud task picker or current cloud task commands. |
| `codex cloud diff <TASK_ID>` | Legacy syntax. Use the current cloud task workflow to inspect results. |
| `codex cloud apply <TASK_ID>` | Legacy syntax. Use `codex apply <TASK_ID>` for a task diff when supported. |
| `codex mcp list` | List configured MCP servers. |
| `codex mcp get <name>` | Show one MCP server configuration. |
| `codex mcp add <name> -- <command...>` | Add a stdio MCP server. |
| `codex mcp add <name> --url https://...` | Add a streamable HTTP MCP server. |
| `codex mcp login <name>` | Start OAuth login for an MCP server. |
| `codex mcp logout <name>` | Remove stored MCP OAuth credentials. |
| `codex mcp remove <name>` | Delete an MCP server definition. |
| `codex mcp-server` | Run Codex itself as an MCP server. |
| `codex plugin list [--available] --json` | List installed plugins or include available plugin metadata as JSON. |
| `codex plugin add <PLUGIN[@MARKETPLACE]> --json` | Add a plugin with structured output. |
| `codex plugin remove --json` | Remove a plugin with structured output. |
| `codex plugin marketplace list --json` | List marketplace sources as JSON. |
| `codex plugin marketplace add <source> [--ref <REF>] [--sparse] --json` | Add a plugin marketplace and optionally return JSON. |
| `codex plugin marketplace remove <name> --json` | Remove a plugin marketplace. |
| `codex plugin marketplace upgrade [name] --json` | Refresh Git-backed plugin marketplaces. |

## TUI Shortcuts

These are typed inside the TUI composer but are not slash commands.

| Shortcut | Use |
|---|---|
| `@` | Open the unified mentions menu for files, plugins, and skills. |
| `Ctrl+R` | Search prompt history. |
| `Ctrl+O` | Copy the latest completed output. |

## Slash Commands

Use these only after `codex` opens the interactive TUI.

| Slash command | Use |
|---|---|
| `/status` | Check model, approval mode, roots, token use, cost estimate, and session details. |
| `/usage` | View token activity, rate limits, and available usage-reset credits. |
| `/new [NAME]` | Start a new conversation; an optional name helps find it later. |
| `/clear [NAME]` | Clear the terminal and start a fresh, optionally named chat. |
| `/resume` | Open the saved-session picker. |
| `/fork` | Fork the current conversation. |
| `/archive` | Archive the current session. |
| `/delete` | Permanently delete the current session after confirmation. |
| `/side` | Start a temporary side chat and return to the parent without closing it. |
| `/compact` | Summarize earlier turns to reduce context use. |
| `/copy` | Copy the latest completed response. |
| `/export` | Export the complete TUI conversation to Markdown, copy it, or save it to a file. |
| `/import` | Import supported setup, project files, recent chats, and other supported artifacts from Claude Code or Cursor. |
| `/diff` | Show current working tree changes. |
| `/review` | Review current changes for bugs and regressions. |
| `/permissions` | Change approval behavior. |
| `/sandbox-add-read-dir C:\absolute\path` | Grant sandbox read access to another Windows directory. |
| `/mention src/file.ts` | Attach a file or folder to the conversation. |
| `/init` | Create an `AGENTS.md` scaffold. |
| `/model` | Choose model and reasoning effort. |
| `/fast on`, `/fast off`, `/fast status` | Control Fast mode when supported. |
| `/plan` | Switch to plan mode. |
| `/goal` | Set or view a long-running goal. |
| `/personality` | Change response style when supported. |
| `/experimental` | Toggle experimental features. |
| `/mcp` | List MCP tools in the current session. |
| `/apps` | Browse apps and connectors. |
| `/plugins` | Browse categorized plugins, recommendations, and installed plugin state. |
| `/app` | Hand off the current CLI thread to Codex Desktop. |
| `/statusline` | Configure TUI footer fields. |
| `/title` | Configure terminal title fields. |
| `/keymap` | Remap TUI keyboard shortcuts. |
| `/agent` | Manage delegated agents when available. |
| `/ps` | List background terminals or running tasks. |
| `/stop` | Stop a running task. |
| `/exit` or `/quit` | Exit the TUI. |
| `/logout` | Sign out from inside the TUI. |
| `/feedback` | Open the feedback flow. |
| `/approvals` | Legacy alias. Prefer `/permissions`. |
| `/clean` | Alias for `/stop`. |

## CLI Flags

Add these to `codex`, `codex exec`, or another subcommand when the command supports them.

| Flag | Use |
|---|---|
| `--add-dir <path>` | Grant access to another directory. |
| `--ask-for-approval, -a <policy>` | Set approval behavior for one run. |
| `--cd, -C <path>` | Set the working directory. |
| `--config, -c key=value` | Override config for one invocation. |
| `--dangerously-bypass-approvals-and-sandbox` | Disable approvals and sandboxing. |
| `--yolo` | Alias for bypassing approvals and sandboxing. |
| `--disable <feature>` | Disable a feature flag for one run. |
| `--enable <feature>` | Enable a feature flag for one run. |
| `--image, -i <path>` | Attach an image to the first prompt. |
| `--help, -h` | Print help. |
| `--version, -V` | Print the installed CLI version. |
| `--model, -m <model>` | Override the configured model. |
| `--no-alt-screen` | Disable alternate-screen TUI mode. |
| `--oss` | Use a local open source provider. |
| `--local-provider <provider>` | Select a local OSS provider. |
| `--profile, -p <name>` | Load a config profile. |
| `--remote <ws://host:port>` | Connect the TUI to a remote app server. |
| `--remote-auth-token-env <ENV_VAR>` | Send a bearer token for remote TUI auth. |
| `--sandbox, -s <mode>` | Set sandbox policy. |
| `--search` | Enable live web search. |
| `--json` | Print newline-delimited JSON events for `codex exec`. |
| `--ephemeral` | Avoid saving session rollout files. |
| `--skip-git-repo-check` | Run outside a Git repository. |
| `--ignore-user-config` | Ignore user config for one run. |
| `--ignore-rules` | Skip user and project execpolicy rules. |
| `--color never` | Control ANSI color output. |
| `--output-last-message out.md` | Save the final response to a file. |
| `--output-schema schema.json` | Validate the final response against a JSON Schema. |
| `--full-auto` | Deprecated shortcut. Prefer explicit sandbox and approval flags. |
| `--experimental-json` | Legacy alias for `--json`. |

## Config Keys

Put these in `~/.codex/config.toml` when you want persistent defaults.

| Config key | Use |
|---|---|
| `model = "gpt-5.5"` | Set the default model. |
| `model_reasoning_effort = "medium"` | Set default reasoning effort. |
| `model_reasoning_summary = "auto"` | Control reasoning summary detail. |
| `model_verbosity = "medium"` | Set GPT-5 Responses API verbosity. |
| `model_provider = "openai"` | Select the model provider. |
| `oss_provider = "ollama"` | Set the provider used by `--oss`. |
| `review_model = "<model>"` | Set the model used by `/review`. |
| `sandbox_mode = "workspace-write"` | Persist the sandbox policy. |
| `sandbox_workspace_write.writable_roots = ["/path"]` | Add writable roots in workspace-write mode. |
| `sandbox_workspace_write.network_access = true` | Allow outbound network in workspace-write mode. |
| `approval_policy = "on-request"` | Persist approval behavior. |
| `history.persistence = "none"` | Stop transcript persistence. |
| `hide_agent_reasoning = true` | Hide reasoning events in TUI and exec output. |
| `mcp_servers.<id>.command = "node"` | Define a stdio MCP launcher command. |
| `mcp_servers.<id>.args = ["server.js"]` | Define stdio MCP launcher arguments. |
| `mcp_servers.<id>.env = { KEY = "VALUE" }` | Set environment variables for a stdio MCP server. |
| `mcp_servers.<id>.bearer_token_env_var = "TOKEN_ENV"` | Read an HTTP bearer token from an environment variable. |
| `mcp_servers.<id>.enabled = false` | Disable a server without deleting it. |
| `mcp_servers.<id>.enabled_tools = ["tool_name"]` | Allow only selected MCP tools. |
| `mcp_servers.<id>.disabled_tools = ["tool_name"]` | Block selected MCP tools. |
| Hooks in `config.toml` | Run lifecycle hooks. |

## Environment Variables

| Variable | Use |
|---|---|
| `OPENAI_API_KEY` | Store an OpenAI API key for API-key auth flows. |
| `CODEX_API_KEY` | Register remote execution setup for approved OpenAI hosts. |
| `CODEX_HOME` | Change where Codex stores config, auth, logs, and sessions. |
| `CODEX_NON_INTERACTIVE=1` | Run supported install scripts without prompts. |
| `CODEX_CA_CERTIFICATE` | Point Codex at a custom CA bundle. |
| `SSL_CERT_FILE` | Fallback custom CA bundle path. |
| `RUST_LOG` | Control Rust logging verbosity. |
| `CODEX_REMOTE_AUTH_TOKEN` | Example bearer-token variable for remote TUI auth. |
| Custom provider `env_key` | Supply a provider-specific API key. |

## Experimental CLI Commands

| Command | Use |
|---|---|
| `codex app-server [--stdio|--listen <URL>]` | Start the experimental app-server. Stdio JSONL is the default; use `--listen ws://...` or `--listen unix://...` for other transports. |
| `codex remote-control pair` | Print a short-lived manual pairing code; add `--json` for machine-readable output. |
| `codex remote-control` | Start or manage a remotely controllable app-server. Use `codex remote-control pair` for manual pairing. |
| `codex execpolicy` | Test execpolicy rules. |
| `codex sandbox macos -- <COMMAND>` | Run a command under the macOS sandbox helper. |
| `codex sandbox linux -- <COMMAND>` | Run a command under the Linux sandbox helper. |
| `codex sandbox windows -- <COMMAND>` | Run a command under the Windows sandbox helper. |
| `codex sandbox ... --permission-profile <NAME>` | Run sandbox commands with a named permissions profile on supported platforms. |
| `codex sandbox setup --elevated` | Provision supported Windows sandbox requirements. |
| `codex --upgrade` | Older update command. Prefer `codex update`. |
| `codex debug seatbelt` | Legacy alias. Prefer `codex sandbox macos`. |
| `codex debug landlock` | Legacy alias. Prefer `codex sandbox linux`. |

## App Server Integration APIs

| Integration API | Use |
|---|---|
| `app-server thread/delete` | Delete a thread through app-server integration. |

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

## Related Resources

- [Codex CLI reference](https://developers.openai.com/codex/cli/reference)
- [Codex CLI releases](https://github.com/openai/codex/releases)
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
OpenAI Codex CLI commands cheat sheet for developers: CLI commands, slash commands, flags, config, environment variables, MCP, and workflows.
```

Recommended topics:

```text
codex, openai, codex-cli, cli, developer-tools, cheat-sheet, mcp, ai-coding
```

## Update Policy

- Keep CLI commands, slash commands, TUI shortcuts, CLI flags, config keys, environment variables, and integration APIs in separate sections.
- Verify unstable commands against the official Codex CLI reference.
- Treat official OpenAI Codex CLI docs as the primary source.
- Use `codex --help` and subcommand help only as local installed-version checks.
- Mark experimental, deprecated, legacy, community, or unverified items clearly.
