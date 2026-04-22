# 10. MCP Servers

_New in v2 · Section verified: April 2026_

Model Context Protocol servers expose tool surfaces — filesystem read, shell, database, internal APIs — for consumption by LLM clients. When operators expose MCP over HTTP/SSE rather than stdio, the tools become available to anyone who connects.

| Shodan Query | Notes |
|---|---|
| `"modelcontextprotocol" port:3000` | MCP SSE transport, no auth default |
| `"mcp" "/sse" "message" port:3000` | MCP server over SSE |
| `"mcp" "tools/list" port:8080` | JSON-RPC MCP transport |
| `"jsonrpc" "2.0" "tools/list"` | Transport-agnostic MCP handshake — catches servers that stripped the "mcp" literal |
| `"server-filesystem" "mcp"` | Filesystem MCP exposed — host file access |
| `"mcp-server-" "sse" port:8000` | |
| `"FastMCP" "uvicorn" port:8000` | Python MCP server framework |
| `"mcp-proxy" port:8080` | stdio-to-HTTP bridge, extends exposure surface |

**MCP over HTTP is the 2026 wave.** The protocol was designed for stdio (in-process transport) but the ecosystem pushed toward HTTP/SSE for remote access. Operators wiring internal tools — shell, database, filesystem — into an MCP server and exposing it without auth is the same failure pattern as unauthenticated RPC from the 1990s, with a new label.
