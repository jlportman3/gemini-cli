# Repository Overview

The gemini-cli project is a command-line AI workflow tool.
It is organized around two main packages:
- `packages/cli` – the user-facing CLI which handles input, history and UI.
- `packages/core` – the backend that communicates with Gemini API and manages tools.

According to the architecture documentation:
The architecture overview describes how the CLI forwards user input to the core, which builds a prompt, interacts with Gemini, optionally executes tools and then returns the answer back to the CLI for display. Key principles include modularity and extensibility.

The core documentation details responsibilities such as API interaction, prompt engineering, tool management, session state and configuration. It also covers security considerations, chat history compression, model fallback, file discovery and hierarchical memory loading.

Tools are defined via `BaseTool` with a schema, optional confirmation, and an `execute` method. Built-in tools include file system helpers (`ls`, `read_file`, `write_file`, etc.), execution tools (`shell`), web fetch/search, and memory storage. The core exposes a registry so the model can call tools, validates parameters, prompts for user approval if needed and then executes the tool.

Telemetry is available through OpenTelemetry with configuration via CLI flags, environment variables or `.gemini/settings.json`. Defaults disable telemetry but you can enable it to send traces locally or to Google Cloud.

Checkpointing automatically snapshots your project before a tool modifies files, storing a git snapshot, conversation history and the pending tool call. You can enable it with `--checkpointing` or in `settings.json`, and use `/restore` to revert.

Extensions are directories under `.gemini/extensions` with a `gemini-extension.json` that can provide MCP servers and context files. Workspace extensions take precedence over user ones.

Sandboxing supports Seatbelt on macOS or container-based isolation. Enable with `-s`, the `GEMINI_SANDBOX` env var or a setting.

The CLI includes numerous commands like `/bug`, `/chat save`, `/restore`, and `@` file inclusion. Shell commands start with `!`. These tools combine to provide interactive AI assistance in your terminal.
