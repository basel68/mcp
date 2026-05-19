# Xray Cloud MCP Server

An MCP (Model Context Protocol) server that exposes Xray Cloud test management operations as tools — enabling AI assistants to create/update test cases and test sets via natural language.

## Features

| Tool | Description |
|------|-------------|
| `create_test_case` | Create a Manual, Cucumber, or Generic test case with steps |
| `update_test_case` | Update summary, steps, type, labels, priority, folder |
| `get_test_case` | Retrieve full test case details |
| `create_test_set` | Create a test set, optionally pre-populated with tests |
| `update_test_set` | Add/remove tests, update fields and folder |
| `get_test_set` | Retrieve test set details and member tests |

## Prerequisites

- Node.js 18+
- Xray Cloud API credentials (Client ID + Client Secret)
  - Generate at: **Xray Cloud → Settings → API Keys**

## Setup

```bash
# Install dependencies
npm install

# Copy env file and add your credentials
cp .env.example .env
# Edit .env with your XRAY_CLIENT_ID and XRAY_CLIENT_SECRET

# Build
npm run build

# Start the server
npm start
```

The server starts on `http://localhost:3000` by default.

## Endpoints

| Endpoint | Description |
|----------|-------------|
| `GET /sse` | SSE connection for MCP clients |
| `POST /messages?sessionId=<id>` | Message endpoint for MCP communication |
| `GET /health` | Health check |

## VS Code / Copilot Integration

Add to your `.vscode/mcp.json` or user MCP settings:

```json
{
  "servers": {
    "xray": {
      "type": "sse",
      "url": "http://localhost:3000/sse"
    }
  }
}
```

## Development

```bash
# Run in development mode (requires ts-node)
npm run dev
```

## Architecture

```
src/
├── index.ts           # Express + MCP server setup (SSE transport)
├── auth.ts            # Xray Cloud authentication (token caching)
├── xray-client.ts     # GraphQL client for Xray Cloud API
└── tools/
    ├── index.ts       # Re-exports all tools
    ├── test-cases.ts  # Create/update/get test cases
    └── test-sets.ts   # Create/update/get test sets
```

## API Reference

This server uses the [Xray Cloud GraphQL API](https://docs.getxray.app/display/XRAYCLOUD/GraphQL+API) for all operations. Authentication is handled via the `/api/v2/authenticate` endpoint with client credentials.
