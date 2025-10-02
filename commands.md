# Development Commands for MCPHub

## Testing Commands
- `pnpm test` - Run all unit tests
- `pnpm test:watch` - Run tests in watch mode
- `pnpm test:coverage` - Run tests with coverage report
- `pnpm test:ci` - Run tests for CI environment
- `pnpm test:verbose` - Run tests with verbose output

## Code Quality Commands
- `pnpm lint` - Run ESLint for code linting
- `pnpm format` - Format code with Prettier

## Build Commands
- `pnpm build` - Build both backend and frontend
- `pnpm backend:build` - Build backend only
- `pnpm frontend:build` - Build frontend only

## Development Commands
- `pnpm dev` - Start both backend and frontend in development mode
- `pnpm backend:dev` - Start backend in development mode
- `pnpm frontend:dev` - Start frontend in development mode
- `pnpm debug` - Start in debug mode

## Utility Commands
- `pnpm start` - Start production server
- `pnpm prepublishOnly` - Prepare for npm publish (build and verify)

## Docker Commands
- Build: `docker build -t mcphub .`
- Run: `docker run -p 3000:3000 mcphub`
- Run with config: `docker run -p 3000:3000 -v ./mcp_settings.json:/app/mcp_settings.json mcphub`

## Git Commands
- `git add . && git commit -m "message"` - Commit changes
- `git push origin main` - Push to main branch
- `git checkout -b feature/name` - Create feature branch

## Environment Setup
- Install dependencies: `pnpm install`
- Install Playwright browsers: `npx playwright install`
- Set environment variables in `.env` file