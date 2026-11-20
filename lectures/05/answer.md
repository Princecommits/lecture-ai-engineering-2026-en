# Lecture 05 Answers

## Exercises

- Tool names shown by `--list-tools` and differences from Lecture 03 (without `--mcp`): During the stdio baseline, the MCP client exposed `glossary_search_glossary`. Function-tool mode uses tools directly registered on `agent`, such as `search_glossary`, while MCP mode adds the server-name prefix and receives the tool description through MCP.
- Tool calls shown by `--mcp --trace` (tool names and arguments) and number of `requests`: The live LLM trace could not be run because `GOOGLE_API_KEY` is not configured in this environment. The HTTP/MCP wiring was verified with the API-key-free test model and `--list-tools`.
- Tool exposed on the MCP server and its description: Added `get_lecture_summary` to `app/mcp_server.py`. It is exposed through MCP as `glossary_get_lecture_summary` and describes retrieving the complete key-point summary for one numbered AIE2026 lecture.
- Tool calls after adding the fetch server (tool names and arguments): Not applicable; the Basic level does not require adding the external fetch server.

## Homework

- Level attempted: **Basic**
- What you did: Started `app/mcp_server.py` with Streamable HTTP, replaced the `glossary` stdio entry in `app/mcp_servers.json` with `{"url": "http://127.0.0.1:8000/mcp"}`, rewrote the `get_lecture_summary` docstring, and compared MCP-visible descriptions.
- Results: The HTTP configuration was accepted by the MCP client and exposed `glossary_search_glossary` and `glossary_get_lecture_summary` on the running server. The rewritten docstring appeared in `--list-tools` with the updated description. A TestModel MCP call reached `glossary_get_lecture_summary` over HTTP and received a result from the server. The live Google-model trace was not available because `GOOGLE_API_KEY` is not configured.
- Observations and questions: Stdio starts a server subprocess for each stdio MCP toolset and communicates over stdin/stdout. Streamable HTTP expects a separately running server at a TCP port (port 8000 here), so the client connects to an existing process and no stdio child process is started by the agent. HTTP mode makes server lifecycle, startup, and port availability explicit; the server must be running before tool listing or use.
