# MCPHub Codebase Structure

## Root Level
- `src/` - Backend source code
- `frontend/` - React frontend application
- `tests/` - Test files
- `docs/` - Documentation
- `scripts/` - Build and utility scripts
- `mcp_settings.json` - MCP server configuration
- `package.json` - Node.js dependencies and scripts
- `tsconfig.json` - TypeScript configuration
- `jest.config.cjs` - Jest testing configuration
- `Dockerfile` - Docker build configuration

## Backend Structure (`src/`)
- `index.ts` - Application entry point
- `server.ts` - Express server setup and middleware
- `routes/` - API route definitions
- `services/` - Business logic services
  - `mcpService.ts` - MCP server management and client connections
  - `sseService.ts` - Server-Sent Events handling
  - `userService.ts` - User management
  - `dataService.ts` - Data filtering and processing
- `config/` - Configuration management
- `db/` - Database entities and connections
- `models/` - Data models
- `middlewares/` - Express middleware
- `utils/` - Utility functions
- `types/` - TypeScript type definitions
- `clients/` - External API clients

## Frontend Structure (`frontend/`)
- `src/`
  - `main.tsx` - React application entry point
  - `components/` - Reusable React components
  - `pages/` - Page components
  - `contexts/` - React contexts for state management
  - `hooks/` - Custom React hooks
  - `constants/` - Application constants
  - `types/` - Frontend type definitions

## Key Architectural Patterns
- **Service Layer**: Business logic separated into service classes
- **Repository Pattern**: Data access through repository classes
- **Middleware Pattern**: Request processing through Express middleware
- **Observer Pattern**: Event-driven communication between services
- **Factory Pattern**: Dynamic service instantiation

## Data Flow
1. HTTP requests → Routes → Services → Database/Models
2. MCP server connections → MCP Service → SSE/WebSocket clients
3. Frontend → REST API → Backend services → Database

## Configuration Flow
1. `mcp_settings.json` loaded via `config/index.ts`
2. Settings validated and processed
3. MCP servers initialized in `mcpService.ts`
4. Services registered with Express app in `server.ts`