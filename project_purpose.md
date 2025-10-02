# MCPHub Project Purpose

MCPHub is a unified hub server for managing and scaling multiple Model Context Protocol (MCP) servers. It provides a centralized platform for:

## Core Functionality
- **Server Management**: Add, remove, and configure MCP servers through a web interface
- **Protocol Support**: Full compatibility with stdio and SSE MCP protocols
- **Unified API**: Single HTTP endpoint for accessing all configured MCP servers
- **Group Management**: Organize servers into logical groups for different use cases
- **Authentication**: JWT-based user management and role-based access control
- **Monitoring**: Real-time status monitoring and performance metrics
- **Smart Routing**: AI-powered tool discovery using vector search (experimental)

## Target Users
- Developers integrating multiple MCP servers
- Organizations managing complex AI tool ecosystems
- Users wanting a unified interface for various MCP-compatible tools

## Architecture
- **Backend**: Node.js/Express with TypeScript
- **Frontend**: React with Vite and Tailwind CSS
- **Database**: PostgreSQL with pgvector for smart routing
- **Deployment**: Docker-ready with configurable settings

## Key Features
- Hot-swappable configuration
- Docker containerization
- RESTful API for server management
- Web dashboard for administration
- Support for various MCP server types (stdio, SSE, streamable HTTP)
- OpenAPI schema support for API-based servers

The project aims to simplify MCP server management and provide a scalable solution for organizations deploying multiple AI tools.