# Manual MCP Test Plan: Playwright & BrowserOS

This document outlines the steps to manually verify the integration of Playwright and BrowserOS MCP servers within the `claw` CLI.

## Prerequisites

1.  **Environment Variables**: Load the local proxy environment variables:
    ```bash
    source ~/export_lm_proxy_env.sh
    ```
2.  **BrowserOS**: Ensure the BrowserOS application is running and the MCP server is enabled at `http://127.0.0.1:9100/mcp`.
3.  **Playwright**: Ensure `npx @playwright/mcp` is accessible (the runtime will launch it automatically if configured).

## Test Case 1: MCP Server Discovery

Verify that both servers are correctly registered and their tools are discovered.

1.  **Launch `claw`**:
    ```bash
    cargo run -p rusty-claude-cli -- --model anthropic/oswe-vscode-prime --dangerously-skip-permissions
    ```
2.  **Check MCP List**:
    Inside the REPL, run:
    ```text
    /mcp list
    ```
    *   **Expected Result**: Both `playwright` and `browseros` should appear in the list with `status: ok` (or similar).
3.  **Inspect BrowserOS Tools**:
    ```text
    /mcp show browseros
    ```
    *   **Expected Result**: A list of tools like `browser_list_tabs`, `browser_open_url`, etc., should be displayed.
4.  **Inspect Playwright Tools**:
    ```text
    /mcp show playwright
    ```
    *   **Expected Result**: A list of tools like `playwright_navigate`, `playwright_screenshot`, etc., should be displayed.

## Test Case 2: BrowserOS Functional Test

Verify that BrowserOS can interact with the actual browser.

1.  **List Tabs**:
    Ask Claude: "List my open browser tabs using BrowserOS."
    *   **Expected Result**: Claude should call `mcp__browseros__browser_list_tabs` and report the titles of your currently open tabs.
2.  **Open URL**:
    Ask Claude: "Open https://example.com in a new tab using BrowserOS."
    *   **Expected Result**: A new tab should open in your browser, and Claude should confirm the action.

## Test Case 3: Playwright Functional Test

Verify that Playwright can perform headless/headed automation.

1.  **Navigate and Screenshot**:
    Ask Claude: "Use Playwright to navigate to https://github.com and tell me the page title."
    *   **Expected Result**: Claude should call `mcp__playwright__playwright_navigate`, retrieve the page, and report the title.
2.  **Search/Interaction**:
    Ask Claude: "Search for 'rust' on the GitHub page using Playwright."
    *   **Expected Result**: Claude should use Playwright interaction tools to perform the search.

## Test Case 4: Permission Enforcement (Optional)

Verify that permissions are respected if NOT using `--dangerously-skip-permissions`.

1.  **Restart without skip-permissions**:
    ```bash
    cargo run -p rusty-claude-cli -- --model anthropic/oswe-vscode-prime
    ```
2.  **Attempt Tool Use**:
    Ask Claude to open a URL.
    *   **Expected Result**: The CLI should prompt you for permission before executing the MCP tool.

## Troubleshooting

-   **MCP Error**: If a server shows an error in `/mcp list`, check if the URL (for BrowserOS) is reachable via `curl http://127.0.0.1:9100/mcp`.
-   **Proxy Issues**: Ensure `ANTHROPIC_BASE_URL` is correctly pointing to your local proxy if the model fails to respond.
-   **Logs**: Check for any error messages in the terminal where `claw` is running.
