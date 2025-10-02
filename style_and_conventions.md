# Code Style and Conventions for MCPHub

## TypeScript/JavaScript
- **TypeScript**: Strictly typed, no `any` types except where necessary
- **Imports**: Use ES6 imports, group by external libraries, then internal modules
- **Naming**: camelCase for variables/functions, PascalCase for classes/interfaces
- **Async/Await**: Preferred over Promises for asynchronous code
- **Error Handling**: Use try/catch, throw descriptive errors

## Code Structure
- **Modularity**: Each file has a single responsibility
- **Services**: Business logic in `src/services/`
- **Routes**: API routes in `src/routes/`
- **Models**: Data models in `src/models/`
- **Config**: Configuration in `src/config/`
- **Utils**: Utility functions in `src/utils/`

## Frontend (React)
- **Components**: Functional components with hooks
- **State Management**: React hooks (useState, useEffect, etc.)
- **Styling**: Tailwind CSS
- **Routing**: React Router

## Testing
- **Framework**: Jest
- **Coverage**: Aim for high coverage, especially critical paths
- **Mocks**: Use jest-mock-extended for complex mocks
- **Integration Tests**: Test API endpoints and service interactions

## Documentation
- **JSDoc**: Use for function/class documentation
- **README**: Comprehensive setup and usage instructions
- **API Docs**: OpenAPI/Swagger for API endpoints

## Best Practices
- **Security**: Input validation, authentication checks
- **Performance**: Efficient database queries, caching where appropriate
- **Maintainability**: Clear code structure, meaningful variable names
- **Error Handling**: Graceful error handling, logging
- **Configuration**: Environment-based config, no hard-coded values

## Git Workflow
- **Commits**: Meaningful commit messages
- **Branches**: Feature branches for new work
- **PRs**: Code review required
- **Releases**: Semantic versioning

## Docker
- **Multi-stage builds**: Optimize image size
- **Security**: Non-root user, minimal base image
- **Configuration**: Environment variables for flexibility