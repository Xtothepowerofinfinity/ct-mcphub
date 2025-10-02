# When a Task is Completed

After completing any coding task, run the following commands in sequence:

1. **Linting**: `pnpm lint` - Check for code style issues
2. **Formatting**: `pnpm format` - Format code according to Prettier rules
3. **Testing**: `pnpm test` - Run unit tests to ensure functionality
4. **Build**: `pnpm build` - Ensure the project builds successfully
5. **Integration Tests**: If applicable, run integration tests

## Quality Gates
- All linting errors must be fixed
- Code must be properly formatted
- All tests must pass
- Build must succeed
- No console errors or warnings in production build

## Documentation Updates
- Update README.md if new features are added
- Update API documentation if endpoints change
- Update configuration docs if settings change

## Deployment
- For Docker deployment: Build and test the Docker image
- Ensure environment variables are properly documented
- Test with production-like configuration

## Git
- Commit changes with descriptive message
- Push to appropriate branch
- Create pull request if working on a feature branch
- Ensure CI/CD pipelines pass