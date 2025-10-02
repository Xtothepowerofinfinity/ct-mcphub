# Suggested Commands for MCPHub Development

## Building and Running
- `pnpm build` - Build the project (backend and frontend)
- `pnpm start` - Start the production server
- `pnpm dev` - Start development mode (backend and frontend concurrently)
- `pnpm backend:dev` - Start backend in development mode
- `pnpm frontend:dev` - Start frontend in development mode

## Testing
- `pnpm test` - Run all tests
- `pnpm test:watch` - Run tests in watch mode
- `pnpm test:coverage` - Run tests with coverage
- `pnpm test:ci` - Run tests for CI

## Code Quality
- `pnpm lint` - Lint the code with ESLint
- `pnpm format` - Format code with Prettier

## Docker
- `docker run -p 3000:3000 -v ./mcp_settings.json:/app/mcp_settings.json samanhappy/mcphub` - Run with custom config
- `docker run -p 3000:3000 samanhappy/mcphub` - Run with default settings

## Development Setup
- `git clone https://github.com/samanhappy/mcphub.git`
- `cd mcphub`
- `pnpm install`
- `pnpm dev`

## Configuration
- MCP settings are loaded from `mcp_settings.json` in the project root or via `MCPHUB_SETTING_PATH` env var
- Default port: 3000 (configurable via `PORT` env var)
- Base path: configurable via `BASE_PATH` env var

## Key Files
- `src/index.ts` - Application entry point
- `src/server.ts` - Main server setup
- `src/services/mcpService.ts` - MCP server management
- `src/config/index.ts` - Configuration loading
- `frontend/` - React frontend
- `mcp_settings.json` - MCP server configuration