# BrowserOS Integration

This project is integrated with BrowserOS via MCP (Model Context Protocol).

## Configuration

The BrowserOS MCP server is configured in `.claw.json`:

```json
{
  "mcpServers": {
    "browseros": {
      "type": "http",
      "url": "http://127.0.0.1:9100/mcp"
    }
  }
}
```

## Environment Variables

To use BrowserOS with a local proxy, ensure the following environment variables are set:

```bash
export ANTHROPIC_BASE_URL="http://localhost:4000/anthropic/claude"
export ANTHROPIC_AUTH_TOKEN="lmproxy"
```

These can be loaded from `~/export_lm_proxy_env.sh`.

## Features

- **Agentic Web Testing**: Use BrowserOS tools for browser navigation and interaction.
- **Data Extraction**: Extract data from web pages using BrowserOS tools.
- **External Apps**: Access 40+ external applications supported by BrowserOS.
- **HTTP Transport**: Communicates with BrowserOS via HTTP POST with `X-BrowserOS-Source: gemini-agent` header.

## Verification

Run the mock parity tests to verify BrowserOS integration:

```bash
cd rust/
./scripts/run_mock_parity_harness.sh
```

The `browseros_list_tabs` scenario specifically tests this integration.
