---
name: create-mcp-with-builder
description: Guides a user through building a fully functional MCP server using the official mcp-server-dev builder plugin, following the four-phase MCP builder methodology (Research & Planning → Implementation → Testing → Evaluation). This skill should be used whenever someone wants to create or scaffold an MCP server and prefers to use Claude's built-in MCP builder tool rather than writing code from scratch. Trigger when users say "build an MCP server", "create an MCP using the builder", "use mcpbuilder", "use mcp-server-dev", "scaffold an MCP server for me", or "help me make an MCP integration with the builder tool".
---

# Create an MCP Server with the MCP Builder

You are guiding the user through building a fully functional MCP server using the official **`mcp-server-dev`** plugin, following the four-phase MCP builder methodology: Research & Planning → Implementation → Testing → Evaluation.

---

## Step 1 — Check whether the builder is installed

Before anything else, check whether `mcp-server-dev` is installed:

```bash
cat ~/.claude/plugins/installed_plugins.json
```

Look for a key starting with `"mcp-server-dev"` in the `plugins` object.

**If found** → skip to Step 3.  
**If not found** → proceed to Step 2.

---

## Step 2 — Prompt the user to install the plugin

Tell the user:

> The `mcp-server-dev` builder plugin is not installed. Install it by running this command in your terminal (or prefix it with `!` to run it here):
>
> ```
> claude plugin install mcp-server-dev
> ```
>
> This installs three skills from the official Claude plugins registry:
> - **`build-mcp-server`** — entry point; determines deployment model and scaffolds the server
> - **`build-mcp-app`** — adds interactive UI widgets (forms, pickers, confirm dialogs) rendered in chat
> - **`build-mcpb`** — packages a local server with its runtime so users don't need Node or Python installed
>
> Once installation completes, type **"continue"** and I'll pick up from here.

Wait for the user to confirm, then re-check `installed_plugins.json` to verify. If it still doesn't appear, suggest running `claude plugin update` to refresh the marketplace index and retrying.

---

## Step 3 — Run the four-phase MCP builder workflow

With the plugin confirmed installed, guide the user through all four phases. Announce each phase before starting it.

---

### Phase 1: Research & Planning

Invoke `/mcp-server-dev:build-mcp-server` to begin the discovery conversation. The builder will ask:

- **What does it connect to?** (cloud API, local filesystem, desktop app, hardware)
- **Who will use it?** (just you / anyone who installs it)
- **How many distinct actions does it expose?** (determines tool-design pattern)
- **Does any tool need mid-call user input or rich display?** (elicitation vs. MCP app widgets)
- **What auth does the upstream service use?** (none/API key, OAuth 2.0)

From those answers the builder recommends a deployment model:
- **Remote streamable-HTTP** — default for cloud API wrappers; zero install friction
- **MCP app** — remote HTTP + interactive UI widgets in chat
- **MCPB** — bundled local server; no Node/Python required for end users
- **Local stdio** — simplest prototype; personal tools only

**Recommended stack:**
- TypeScript with `@modelcontextprotocol/sdk` (primary)
- Python with `fastmcp` 3.x (if user prefers Python or is wrapping a Python library)

---

### Phase 2: Implementation

The builder scaffolds the project. Key outputs:

- `package.json` (or `pyproject.toml`) with the MCP SDK dependency and a `bin` entry
- `tsconfig.json` / project config
- `src/index.ts` (or `server.py`) with:
  - Server initialization and transport setup
  - Tool definitions with precise input/output schemas
  - Authentication handling (API key injection, OAuth token exchange)
  - Error handling with MCP-standard error codes

**Tool design patterns:**
- **One tool per action** — for servers with fewer than ~15 operations; gives Claude a tight, readable tool list
- **Search + execute** — for large API surfaces (dozens of endpoints); exposes two tools (`search_actions`, `execute_action`) and keeps context lean

After scaffolding, tell the user to:
```bash
npm install && npm run build
# or: pip install fastmcp && python server.py
```

---

### Phase 3: Testing

Guide the user to test the server before shipping:

1. **Code quality review** — check for duplicated logic, consistent error handling, no hardcoded secrets
2. **Build and compile** — confirm `npm run build` (or `python -m py_compile`) succeeds with no errors
3. **MCP Inspector** — connect the running server to the MCP Inspector tool for interactive tool testing:
   ```bash
   npx @modelcontextprotocol/inspector node dist/index.js
   ```
4. **Live test in Claude Code** — add the server as a local MCP server:
   ```bash
   claude mcp add <server-name> -- node $(pwd)/dist/index.js
   ```
   Then invoke each tool from a Claude Code session and verify outputs match expectations.

---

### Phase 4: Evaluation

Before marking the server ready for distribution, create an evaluation suite of **10 realistic, complex test questions** that exercise the server's tools. Each question should:

- Reflect real user intent (not a toy example)
- Require at least one tool call to answer
- Have a verifiable expected answer

Output the evaluation suite in this XML format:

```xml
<evaluation>
  <question id="1">
    <prompt>...</prompt>
    <tools_required>tool_name_1, tool_name_2</tools_required>
    <expected_answer>...</expected_answer>
  </question>
  <!-- repeat for all 10 questions -->
</evaluation>
```

Verify each expected answer independently (by running the tool or checking the upstream API docs). Save the file as `eval/questions.xml` in the project root.

---

## Step 4 — Next steps after all four phases

Print this summary:

```
Your MCP server is ready. Next steps:

1. Publish the package:
   npm login && npm publish --access public
   (or: pip install build && python -m build && twine upload dist/*)

2. Submit to the Anthropic connector directory:
   https://claude.com/docs/connectors/building/submission

3. Wrap it as a Claude plugin for easy distribution:
   /plugin-to-mcp  (converts the server into a plugin with skills)

4. Submit to the official MCP registry:
   /mcp-submit-mcp-registry  (generates a pre-filled submission form)
```

---

## Handling edge cases

- **User knows which sub-skill they need directly:**
  - Interactive UI (forms, pickers, live widgets): `/mcp-server-dev:build-mcp-app`
  - Bundled local server: `/mcp-server-dev:build-mcpb`
  - Everything else: `/mcp-server-dev:build-mcp-server`

- **Plugin install fails:** Check internet connection, confirm they are on Claude Code CLI (not the web app), run `claude plugin update`, then retry.

- **User asks about transport types:**
  > - **Remote streamable-HTTP** — hosted, zero install, one deployment serves all users. Best for cloud APIs.
  > - **MCPB** — local server bundled with runtime. No Node/Python needed. Best for local file access or driving desktop apps.
  > - **Local stdio** — easiest to prototype, painful to distribute. Personal tools only.
