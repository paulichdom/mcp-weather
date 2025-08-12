# AI Assistant Project Instructions

Purpose: Enable AI coding agents to be immediately productive in this repo.

## Project Snapshot

- Small prototype MCP (Model Context Protocol) weather server.
- Tech: Python 3.11+, `httpx` for async HTTP, `mcp` library (FastMCP helper).
- Key files:
  - `weather.py`: Core logic (MCP server instance + helper functions).
  - `main.py`: Simple entry point placeholder (currently prints a greeting).
  - `pyproject.toml`: Declares dependencies.
  - `README.md`: High-level description (user-focused wording).

## Architecture & Data Flow

1. Client (e.g., Claude Desktop MCP host) invokes exposed MCP tools (not yet implemented in file, only scaffold present).
2. Server code will call National Weather Service API (base: `https://api.weather.gov`).
3. JSON responses are parsed and formatted into human-readable strings (`format_alert`).
4. Returned strings or structured data are sent back through MCP tool responses.

Currently missing: actual `@mcp.tool` (or similar) decorated functions to expose `get_alerts` / `get_forecast`. Add them in `weather.py` beside helper functions.

## Conventions

- Constants: ALL_CAPS (`NWS_API_BASE`, `USER_AGENT`).
- Async I/O for network operations (`httpx.AsyncClient`).
- Graceful failure: network helper returns `None` on any exception; callers must handle fallback messaging.
- Formatting: Multiline f-strings for user-facing output.
- Typing: Uses modern Python 3.11+ syntax (`dict[str, Any] | None`).

## Adding MCP Tools (Pattern)

Example pattern to follow inside `weather.py`:

```python
from mcp.server.fastmcp import tool

@tool()  # or mcp.tool if API differs
async def get_alerts(area: str) -> str:
    data = await make_nws_request(f"{NWS_API_BASE}/alerts/active?area={area}")
    if not data or not data.get("features"):
        return "No active alerts."
    return "\n".join(format_alert(f) for f in data["features"][:5])
```

Check actual decorator name from `fastmcp` (may be `@mcp.tool`).

## Error Handling Expectations

- Do NOT raise raw HTTP exceptions to the user; convert to neutral messages (e.g., "Service unavailable").
- If `make_nws_request` returns `None`, respond with a safe fallback string.

## Dependency & Environment

- Install deps via: `pip install -e .` (if project is a package) or `pip install httpx mcp[cli]`.
- Python version: enforced `>=3.11`.

## Typical Dev Loop

1. Implement / adjust a tool in `weather.py`.
2. Run an MCP host using CLI (e.g., `mcp run weather:...`) – determine exact command once tools are added.
3. Test via Claude Desktop by configuring the server.

## Extensibility Notes

- Consider adding a caching layer (dict with timestamps) if rate limits appear.
- Could introduce structured return objects later (`TypedDict` / Pydantic) but keep initial outputs simple.

## Style & Quality

- Keep functions short and async where IO-bound.
- Prefer explicit type hints for tool parameters & returns.
- Limit external dependencies; current minimal set is intentional.

## Gaps / TODOs (Concrete)

- Expose `get_alerts` tool.
- Add `get_forecast` tool (likely needs point lookup: `/points/{lat},{lon}` then follow `forecast` URL in response).
- Add minimal tests (e.g., simulate alert formatting with sample JSON).
- Expand README with usage instructions & sample output.

## When Unsure

- Search `fastmcp` docs for correct decorator names.
- Keep outputs concise; trim large JSON payloads.
- Ask for clarification before introducing new dependencies.

---

Provide deltas only when updating this file; retain concise actionable focus.
