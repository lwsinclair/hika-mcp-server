[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/atlas-comstock-hika-mcp-server-badge.png)](https://mseep.ai/app/atlas-comstock-hika-mcp-server)

# Hika MCP Server

## Overview

Hika MCP Server is a Server-Sent Events (SSE) implementation for the Hika AI knowledge search tool. This server component facilitates real-time streaming of AI-generated perspectives and knowledge extensions to enhance the search experience.

Hika AI is a knowledge search tool that uses artificial intelligence to provide multiple cognitive perspectives (MCP) on search queries, helping users to broaden their understanding by exploring related knowledge domains or diving deeper into specific aspects of their questions.

## Features

- Real-time streaming of AI-generated content via SSE protocol
- Multiple cognitive perspectives to enhance knowledge exploration
- Authentication via JWT Bearer tokens
- Easy integration with the Hika AI ecosystem

## Prerequisites

- Node.js (v14.0.0 or higher)
- Understanding of Server-Sent Events (SSE)
- Valid Hika AI authentication credentials

## Installation

```bash
# Clone the repository
git clone https://github.com/hikaai/mcp-server.git
cd mcp-server

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env
# Edit .env with your credentials
```

## Configuration

Set the following environment variables:

```
PORT=3000
HIKA_API_URL=https://api.hika.fyi
JWT_SECRET=your_jwt_secret
```

Or use the provided configuration in your application:

```javascript
{
  "command": "",
  "url": "https://hika.fyi/api/mcp/sse",
  "env": {
    "HIKA_AUTH": "Bearer your_jwt_token_here"
  }
}
```

## Usage

### Starting the Server

```bash
npm start
```

### Client Connection Example

```javascript
const eventSource = new EventSource('https://hika.fyi/api/mcp/sse', {
  headers: {
    'Authorization': 'Bearer your_jwt_token_here'
  }
});

eventSource.onmessage = (event) => {
  const data = JSON.parse(event.data);
  console.log('Received perspective:', data);
};

eventSource.onerror = (error) => {
  console.error('SSE connection error:', error);
  eventSource.close();
};
```

## API Reference

### SSE Endpoint

```
GET /api/mcp/sse
```

#### Headers

- `Authorization`: Bearer token for authentication

#### Events

The server emits the following event types:

1. `perspective` - New AI-generated perspective on the search query
2. `knowledge_extension` - Additional related knowledge domain
3. `deep_dive` - In-depth analysis of a specific aspect
4. `completion` - Signals the end of the stream

#### Event Data Format

```json
{
  "type": "perspective",
  "content": {
    "title": "Historical Context",
    "description": "Looking at the question from a historical perspective...",
    "relevance_score": 0.85
  }
}
```

## Authentication

The server uses JWT Bearer tokens for authentication. Tokens should be included in the `Authorization` header as follows:

```
Authorization: Bearer eyJhbGciOiJFUzM4NCIsInR5cCI6ImF0K2p3dCIs...
```

## Error Handling

The server may return the following error codes:

- `401` - Unauthorized (invalid or expired token)
- `429` - Too Many Requests (rate limit exceeded)
- `500` - Internal Server Error

## Development

### Running in Development Mode

```bash
npm run dev
```

### Running Tests

```bash
npm test
```

## Integration with Hika AI

This server component is designed to work seamlessly with the Hika AI knowledge search tool. When integrated, it provides real-time AI-generated perspectives that help users:

- Quickly extend their knowledge into related domains
- Explore different viewpoints on the same question
- Dive deeper into specific aspects of complex topics

## License

[MIT License](LICENSE)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Support

For support, please contact support@hika.fyi or open an issue on GitHub.
