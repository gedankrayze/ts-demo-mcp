# TypeScript MCP Demo Server

A basic Model Context Protocol (MCP) server implementation using TypeScript SDK and Bun runtime.

## Overview

This project demonstrates how to build an MCP server with TypeScript, featuring:
- Server initialization with stdio transport
- Tool registration and handling
- TypeScript type safety
- Bun runtime for fast execution

## Prerequisites

- [Bun](https://bun.sh/) (latest version)
- [Task](https://taskfile.dev/) (optional, for task runner)

## Installation

```bash
# Clone the repository
git clone https://github.com/gedankrayze/ts-demo-mcp.git
cd ts-demo-mcp

# Install dependencies
bun install
```

## Project Structure

```
ts-demo-mcp/
├── src/
│   └── index.ts        # Main MCP server implementation
├── package.json        # Project dependencies and scripts
├── tsconfig.json       # TypeScript configuration
├── Taskfile.yaml       # Task runner configuration
└── README.md          # This file
```

## Available Tools

The server implements two example tools:

### 1. `greet`
Greets a user with their name.

**Input Schema:**
```json
{
  "name": "string"  // Required: The name of the person to greet
}
```

**Example Response:**
```
Hello, John! Welcome to the TypeScript MCP demo server.
```

### 2. `add_numbers`
Adds two numbers together.

**Input Schema:**
```json
{
  "a": "number",  // Required: The first number
  "b": "number"   // Required: The second number
}
```

**Example Response:**
```
The sum of 5 and 3 is 8.
```

## Usage

### Using npm/bun scripts

```bash
# Start the MCP server
bun start

# Run TypeScript type checking
bun typecheck
```

### Using Task runner

```bash
# Start the MCP server
task start

# Inspect server capabilities with MCP inspector
task inspect-server

# Send a raw initialization request
task inspect
```

## Integration with Claude Desktop

To use this server with Claude Desktop, add the following to your Claude Desktop configuration:

```json
{
  "mcpServers": {
    "ts-demo-mcp": {
      "command": "bun",
      "args": ["run", "/path/to/ts-demo-mcp/src/index.ts"]
    }
  }
}
```

## Development

### Adding New Tools

To add new tools to the server:

1. Update the tool list in the `ListToolsRequestSchema` handler
2. Add the tool implementation in the `CallToolRequestSchema` handler

Example:
```typescript
// In ListToolsRequestSchema handler
{
  name: "my_tool",
  description: "Description of what the tool does",
  inputSchema: {
    type: "object",
    properties: {
      param1: { type: "string", description: "Parameter description" }
    },
    required: ["param1"]
  }
}

// In CallToolRequestSchema handler
case "my_tool": {
  const param1 = request.params.arguments?.param1 as string;
  return {
    content: [{
      type: "text",
      text: `Tool output for ${param1}`
    }]
  };
}
```

### Type Checking

The project uses TypeScript with strict mode enabled. Run type checking with:

```bash
bun typecheck
```

## MCP Inspector

The project includes the MCP inspector for debugging. Use it to explore server capabilities:

```bash
task inspect-server
```

This will launch an interactive web interface to test your MCP server.

## License

This project is public and available at [https://github.com/gedankrayze/ts-demo-mcp](https://github.com/gedankrayze/ts-demo-mcp).

## Resources

- [Model Context Protocol Documentation](https://modelcontextprotocol.io/)
- [MCP TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk)
- [Bun Documentation](https://bun.sh/docs)