# Slopographic for Claude Code (Slopographic MCP)

[Slopographic](https://slopographic.com) is a set of free apps that run in your browser: PhotoSlop (photo editing),
Slop PDF, SlopCut (video), File convert, Slop Workflows and more. This plugin lets Claude Code use the app you have
open in your browser, with the app's own commands, and bring your files in and the results back out as files on your
computer.

## Install (once)

In Claude Code:

```
/plugin marketplace add slopographic/claude-plugin
/plugin install slopographic@slopographic
```

You need Node.js 20 or newer (Claude Code already has it) and Chrome, Edge or another Chromium browser.

## Use it

```
/slopographic:slop-connect photoslop
```

1. Claude Code opens Slopographic in your browser and shows a short code, like `K7Q-4MZ`.
2. Chrome asks once whether the site may use apps and services on this device. Choose **Allow**. That is how the
   page reaches Claude Code on your computer. It talks to your own computer only (127.0.0.1), not to your network.
3. Type the code in the page. The page shows **Claude Code is connected · Disconnect** in the corner.

Then just ask, for example:

```
/slopographic:slop-edit "remove the background of ./photos/cat.jpg and save it as ./cat-cutout.png"
```

or in your own words: "resize every picture in ./shots to 1200 px wide with Slopographic", "turn report.docx into a PDF".

Any Slopographic page can connect too: press Ctrl+K (⌘K on a Mac) and choose **Connect an AI app…**.

## Disconnect

Press **Disconnect** in the page, close the tab, or ask Claude Code to disconnect. Ending the Claude Code session ends
it as well. The next time, you pair again with a new code.

## What it can do

- `connect`, `status`, `use_tab`, `disconnect`: pairing, and choosing a tab when several are connected.
- `slop_…`: the tools of the app in the active tab, generated from the app's own commands (the same ones as the
  Slopographic AI panel): for example `slop_app_state`, `slop_list_commands`, `slop_run_command`, `slop_list_files`,
  `slop_run_operation`, `slop_run_workflow`, `slop_open_file`, and one tool per app command (PhotoSlop has more than 30).
  They change when you switch apps.
- `send_file_to_app`: a file on your computer into the app (or into Slop Drive, the site's storage in your browser).
- `save_result`: a file from the app or Slop Drive to your computer. It never replaces a file unless you ask.

Paths are relative to the folder Claude Code runs in. A path outside it is used only when you name it.

## Privacy and safety

- Everything stays on your computer. The server listens on 127.0.0.1 only and accepts only the Slopographic pages
  (slopographic.com and slopographic.github.io). A page can use it only after you type the one-time code; each tab is
  paired on its own, and the code expires after 10 minutes or 5 wrong tries.
- Your files stay on your computer and in your browser. What Claude Code sends to Anthropic is what it always sends:
  your conversation, including the tool calls and their results.
- Actions that delete, clear or replace your work ask you in the page first, following your AI permission settings.
- Claude Code asks you before each tool it has not been allowed to use.

## Without the plugin

The same server runs in other AI apps (below) or by hand: `npx -y github:slopographic/claude-plugin`.
Settings (environment variables): `SLOPOGRAPHIC_PORT` (first port; default 47821, then the next 9),
`SLOPOGRAPHIC_SITE`, `SLOPOGRAPHIC_ROOT` (the folder for relative paths), `SLOPOGRAPHIC_NO_OPEN=1` (never open a browser).

Source: [slopographic/apps](https://github.com/slopographic/apps) (`tools/claude-plugin/`, `shared/agent/`). MIT licence.

## Other AI apps

The same server works in any AI app that runs local MCP servers. Each app needs the lines below once; then ask it to connect to Slopographic.

### Claude Desktop

Download slopographic.mcpb and open it: Claude Desktop installs it as an extension (it brings its own Node).

Or add the server to claude_desktop_config.json (Settings ▸ Developer ▸ Edit Config), then quit and reopen Claude.

Extension:

```
https://github.com/slopographic/claude-plugin/releases/latest/download/slopographic.mcpb
```

claude_desktop_config.json (~/Library/Application Support/Claude/claude_desktop_config.json · %APPDATA%\Claude\claude_desktop_config.json):

```json
{
  "mcpServers": {
    "slopographic": {
      "command": "npx",
      "args": [
        "-y",
        "github:slopographic/claude-plugin"
      ]
    }
  }
}
```

Format from https://modelcontextprotocol.io/docs/develop/connect-local-servers

### OpenAI Codex (CLI, IDE and the ChatGPT desktop app)

Run the command once, or add the lines to ~/.codex/config.toml. The first start downloads the server, so give it a minute.

Command:

```sh
codex mcp add slopographic -- npx -y github:slopographic/claude-plugin
```

~/.codex/config.toml:

```toml
[mcp_servers.slopographic]
command = "npx"
args = ["-y", "github:slopographic/claude-plugin"]
startup_timeout_sec = 60
tool_timeout_sec = 600
```

Format from https://learn.chatgpt.com/docs/extend/mcp?surface=cli

### Gemini CLI

Add the server to ~/.gemini/settings.json. Antigravity CLI takes the same entry in ~/.gemini/config/mcp_config.json.

~/.gemini/settings.json:

```json
{
  "mcpServers": {
    "slopographic": {
      "command": "npx",
      "args": [
        "-y",
        "github:slopographic/claude-plugin"
      ],
      "timeout": 600000
    }
  }
}
```

Format from https://geminicli.com/docs/tools/mcp-server/

### Cursor

Add the server to ~/.cursor/mcp.json (every project) or .cursor/mcp.json (one project).

~/.cursor/mcp.json:

```json
{
  "mcpServers": {
    "slopographic": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "github:slopographic/claude-plugin"
      ]
    }
  }
}
```

Format from https://cursor.com/docs/context/mcp

### VS Code (GitHub Copilot)

Run the command once (it adds the server to your profile), or add it to .vscode/mcp.json in a project. Note the key is "servers".

Command:

```sh
code --add-mcp '{"name":"slopographic","type":"stdio","command":"npx","args":["-y","github:slopographic/claude-plugin"]}'
```

.vscode/mcp.json:

```json
{
  "servers": {
    "slopographic": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "github:slopographic/claude-plugin"
      ]
    }
  }
}
```

Format from https://code.visualstudio.com/docs/agents/reference/mcp-configuration

### Windsurf / Devin Desktop

Add the server to mcp_config.json: ~/.config/devin/mcp_config.json (Devin Desktop; %APPDATA%\devin on Windows) or ~/.codeium/windsurf/mcp_config.json (Windsurf).

mcp_config.json:

```json
{
  "mcpServers": {
    "slopographic": {
      "command": "npx",
      "args": [
        "-y",
        "github:slopographic/claude-plugin"
      ]
    }
  }
}
```

Format from https://docs.devin.ai/desktop/cascade/mcp

### ChatGPT (chat)

ChatGPT chats connect only to servers on the internet (an https address), not to a program on your computer, so this server does not work there yet. Codex in the ChatGPT desktop app does (see OpenAI Codex).

Format from https://developers.openai.com/apps-sdk/deploy/connect-chatgpt

