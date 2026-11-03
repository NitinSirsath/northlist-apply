# Connect NorthList

Read this file only when the user pastes a setup prompt from NorthList ("Connect this NorthList Apply folder to my NorthList account", with a NorthList server address and a token), or asks to connect or reconnect NorthList. It may come before setup is finished. Never apply to jobs during these steps.

The values in the prompt:
- `<server>`: the NorthList server address, for example `https://career-companion-api-i0lc.onrender.com`.
- `<mcp>`: the same address with `/mcp` at the end.
- `<token>`: the token. It starts with `ccmcp_`.

## The token

- The token goes only into the AI app's own MCP settings for this user, which live outside this folder.
- Never write it into a file in this folder: not `personal_data/`, not the daily record, not `.mcp.json` or `.agents/mcp_config.json`.
- Never repeat it in your messages. Call it "your token".
- If a step would put it anywhere else, stop and ask the user.
- Never change the app's permission or approval settings. The user allows the NorthList tools in the app itself, the first time each one runs.

## 1. Which app is this?

Say in one line which app you are running in: Claude Code (also the Code tab of the Claude desktop app), Codex (the CLI, the IDE extension or the ChatGPT desktop app), or Antigravity. If you cannot tell, ask the user once.

## 2. Add NorthList to the app

The app asks the user to allow these changes, because they are outside this folder. If a server named `northlist` already exists (an older token), replace it. Leave every other server as it is.

**Claude Code**
1. If `claude mcp list` shows `northlist`, run `claude mcp remove northlist --scope user`.
2. Run:
   ```
   claude mcp add --transport http --scope user northlist <mcp> --header "Authorization: Bearer <token>"
   ```
3. Check with `claude mcp list`: `northlist` must be listed with `<mcp>`. Never run `claude mcp get northlist`, which can show the token.

**Codex**
1. Open `~/.codex/config.toml`. Create it if it is missing.
2. Remove an existing `[mcp_servers.northlist]` table, then add:
   ```toml
   [mcp_servers.northlist]
   url = "<mcp>"
   http_headers = { "Authorization" = "Bearer <token>" }
   ```
3. Keep everything else in the file as it is.

**Antigravity**
1. Open `~/.gemini/config/mcp_config.json`. Create it as `{ "mcpServers": {} }` if it is missing.
2. Under `mcpServers`, set:
   ```json
   "northlist": { "serverUrl": "<mcp>", "headers": { "Authorization": "Bearer <token>" } }
   ```
3. Keep everything else in the file as it is, and keep it valid JSON. Never use the folder's own `.agents/mcp_config.json`.

**Another app**

Do not guess where it keeps its settings. Tell the user to add an MCP server named `northlist` in that app's settings: the address `<mcp>`, and the header `Authorization` with the value `Bearer` followed by the token from the setup prompt. Then go on with step 3.

## 3. The profile line

1. If `personal_data/profile.md` is missing, create it from `personal_data/profile_template.md`.
2. Set its NorthList server line to `<server>`, without `/mcp`, replacing `None` or an older address. An older `Career Companion server:` line becomes the NorthList line.
3. Change nothing else in the file.

## 4. Tell the user

In a few short lines, without the token:
- NorthList was added to this app's settings for this user, and the profile line is set.
- **What to do now:**
  - Claude Code: open a new chat in this folder, because new servers load at the start of a chat.
  - Codex: restart the app. In the desktop app, Restart; in the IDE, Restart extension; in the CLI, start a new session.
  - Antigravity: Refresh the MCP servers.
- Then type `start`. At `start`, the NorthList check (`instructions/northlist_sync.md`) confirms that the two tools are there. If setup is not finished, `start` does setup first.
- The first time each NorthList tool runs (after the first application, and at the end of the first session), the app asks. Allow it for good: "Yes, and don't ask again" in Claude Code, "Allow and don't ask me again" in Codex, "Always allow" in Antigravity.

**Reconnect.** When a token expires or is revoked, the user creates a new setup prompt in NorthList (Automation › Setup) and pastes it here.
