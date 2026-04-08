---
title: MCP
hidden: true
---
The intrud3r-docs Model Context Protocol (MCP) server enables AI-powered code editors like Cursor and Windsurf, plus general-purpose tools like Claude Desktop, to interact directly with your intrud3r-docs API and documentation.

## What is MCP?

Model Context Protocol (MCP) is an open standard that allows AI applications to securely access external data sources and tools. The intrud3r-docs MCP server provides AI agents with:

* **Direct API access** to intrud3r-docs functionality
* **Documentation search** capabilities
* **Real-time data** from your intrud3r-docs account
* **Code generation** assistance for intrud3r-docs integrations

## intrud3r-docs MCP Server Setup

intrud3r-docs hosts a remote MCP server at `https://intrud3r-docs.readme.io/mcp`. Configure your AI development tools to connect to this server. If your APIs require authentication, you can pass in headers via query parameters or however headers are configured in your MCP client.

<Tabs>
  <Tab title="Cursor">
    **Add to `~/.cursor/mcp.json`:**

    ```json
    {
      "mcpServers": {
        "intrud3r-docs": {
          "url": "https://intrud3r-docs.readme.io/mcp"
        }
      }
    }
    ```

    </Tab>
  <Tab title="Windsurf">
    **Add to `~/.codeium/windsurf/mcp_config.json`:**

    ```json
    {
      "mcpServers": {
        "intrud3r-docs": {
          "url": "https://intrud3r-docs.readme.io/mcp"
        }
      }
    }
    ```

  </Tab>
  <Tab title="Claude Desktop">
    **Add to `claude_desktop_config.json`:**

    ```json
    {
      "mcpServers": {
        "intrud3r-docs": {
          "url": "https://intrud3r-docs.readme.io/mcp"
        }
      }
    }
    ```

  </Tab>
</Tabs>

## Testing Your MCP Setup

Once configured, you can test your MCP server connection:

1. **Open your AI editor** (Cursor, Windsurf, etc.)
2. **Start a new chat** with the AI assistant
3. **Ask about intrud3r-docs** - try questions like:
   * "How do I [common use case]?"
   * "Show me an example of [API functionality]"
   * "Create a [integration type] using intrud3r-docs"

The AI should now have access to your intrud3r-docs account data and documentation through the MCP server.