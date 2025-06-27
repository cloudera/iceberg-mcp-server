# Cloudera Iceberg MCP Server (via Impala)

This is a A Model Context Protocol server that provides read-only access to Iceberg tables via Apache Impala. This server enables LLMs to inspect database schemas and execute read-only queries.

- `execute_query(query: str)`: Run any SQL query on Impala and return the results as JSON.
- `get_schema()`: List all tables available in the current database.

## Usage with Claude Desktop

To use this server with the Claude Desktop app, add the following configuration to the "mcpServers" section of your `claude_desktop_config.json`:

### Option 1: Direct installation from GitHub (Recommended)
```json
{
  "mcpServers": {
    "iceberg-mcp-server": {
      "command": "uvx",
      "args": [
        "--from",
        "git+https://github.com/cloudera/iceberg-mcp-server@main",
        "run-server"
      ],
      "env": {
        "IMPALA_HOST": "coordinator-default-impala.example.com",
        "IMPALA_PORT": "443",
        "IMPALA_USER": "username",
        "IMPALA_PASSWORD": "password",
        "IMPALA_DATABASE": "default"
      }
    }
  }
}
```

### Option 2: Local installation (after cloning the repository)
```json
{
  "mcpServers": {
    "iceberg-mcp-server": {
      "command": "uv",
      "args": [
        "--directory",
        "/path/to/iceberg-mcp-server",
        "run",
        "src/iceberg_mcp_server/server.py"
      ],
      "env": {
        "IMPALA_HOST": "coordinator-default-impala.example.com",
        "IMPALA_PORT": "443",
        "IMPALA_USER": "username",
        "IMPALA_PASSWORD": "password",
        "IMPALA_DATABASE": "default"
      }
    }
  }
}
```

For Option 2, replace `/path/to` with your path to this repository. Set the environment variables according to your Impala configuration.

## Usage with AI frameworks

The `./examples` folder contains several examples how to integrate this MCP Server with common AI Frameworks like LangChain/LangGraph, OpenAI SDK.

### Transport

The MCP server's transport protocol is configurable via the `MCP_TRANSPORT` environment variable. Supported values:
- `stdio` **(default)** — communicate over standard input/output. Useful for local tools, command-line scripts, and integrations with clients like Claude Desktop.
- `http` - expose an HTTP server. Useful for web-based deployments, microservices, exposing MCP over a network.
- `sse` — use Server-Sent Events (SSE) transport. Useful for existing web-based deployments that rely on SSE.


*Copyright (c) 2025 - Cloudera, Inc. All rights reserved.*
