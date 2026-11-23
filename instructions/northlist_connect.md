# Connect NorthList

Read this file only when the user pastes a setup prompt from NorthList ("Connect NorthList Apply to my NorthList account", with a NorthList server address and a setup code; or the older "Connect this NorthList Apply folder to my NorthList account", with a token), or asks to connect or reconnect NorthList. It may come before setup is finished. Never apply to jobs during these steps.

The values in the prompt:
- `<server>`: the NorthList server address, for example `https://career-companion-api-i0lc.onrender.com`.
- `<code>`: the setup code, like `NL-7Q2X-K9PD-4MWT`. It works once, for 10 minutes. The older prompt has a token instead (it starts with `ccmcp_`): then skip step 0.

## 0. Swap the setup code for a token

Do this first, before anything else: the code works only for 10 minutes.

Send the code to NorthList with the app's command tool:
- Mac and Linux: `curl -s -X POST <server>/api/setup-codes/redeem -H "Content-Type: application/json" -d '{"code":"<code>"}'`
- Windows: `Invoke-RestMethod -Method Post -Uri <server>/api/setup-codes/redeem -ContentType "application/json" -Body '{"code":"<code>"}'`

The answer is JSON:
- With `plaintextToken`: that is `<token>`. Its `serverAddress` is `<server>` from now on. Go on with step 1.
- With `SETUP_CODE_REFUSED`: stop and say "This setup code is used or too old. In NorthList, open Automation › Setup, make a new one and paste the new prompt here." Never try the same code again.
- With `RATE_LIMITED`, or no answer: wait one minute and try once more. If that fails too, say "NorthList did not answer. Make a new setup code in NorthList (Automation › Setup) later and paste it here." and stop.
- Anything else (for example `TOKEN_LIMIT_REACHED`): say its `message` in one line and stop.

`<mcp>` is `<server>` with `/mcp` at the end.

## The token

- The token goes only into the AI app's own MCP settings for this user, which live outside this folder.
- Never write it into a file in this folder: not `personal_data/`, not the daily record, not `.mcp.json` or `.agents/mcp_config.json`.
- Never repeat it in your messages. Call it "your token". The setup code may be shown; it is used up after step 0.
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
- **This chat is not in the NorthList Apply folder** (you found it in Documents or downloaded it): say first where the folder is and how to open it there: Claude Code: in a terminal, `cd "<folder>"`, then `claude`; Codex: open the folder as a project; Antigravity: File › Open Folder. `start` is typed there.
- NorthList was added to this app's settings for this user, and the profile line is set.
- **What to do now:**
  - Claude Code: open a new chat in this folder, because new servers load at the start of a chat.
  - Codex: restart the app. In the desktop app, Restart; in the IDE, Restart extension; in the CLI, start a new session.
  - Antigravity: Refresh the MCP servers.
- Then type `start`. At `start`, the NorthList check (`instructions/northlist_sync.md`) confirms that the two tools are there. If setup is not finished, `start` does setup first.
- The first time each NorthList tool runs (after the first application, and at the end of the first session), the app asks. Allow it for good: "Yes, and don't ask again" in Claude Code, "Allow and don't ask me again" in Codex, "Always allow" in Antigravity.

**Reconnect.** When a token expires or is revoked, the user creates a new setup prompt in NorthList (Automation › Setup) and pastes it here. The same steps replace the old token.
