# dify Development Patterns

> Auto-generated skill from repository analysis

## Overview

This skill teaches the development patterns for the dify repository, a TypeScript/Python codebase that follows conventional commit patterns and comprehensive testing practices. The codebase emphasizes strong test coverage, dependency management, and structured workflows for both frontend (React/TypeScript) and backend (Python) components.

## Coding Conventions

### File Naming
- **TypeScript/React**: Use camelCase for file names
- **Test files**: Follow `*.spec.tsx` pattern for component tests
- **Python tests**: Mirror module structure in test directories

### Import/Export Style
```typescript
// Use relative imports
import { ComponentName } from '../components/ComponentName'
import { utilityFunction } from './utils'

// Use named exports
export { ComponentName }
export const utilityFunction = () => {}
```

### Commit Messages
- Follow conventional commit format
- Use prefixes: `feat:`, `fix:`, `chore:`, `refactor:`, `test:`
- Keep messages around 65 characters
- Examples:
  ```
  feat: add user authentication component
  fix: resolve API timeout issues
  test: add unit tests for data service
  ```

## Workflows

### Dependency Update
**Trigger:** When dependencies need to be updated or security patches are required
**Command:** `/update-deps`

1. Update `web/package.json` with new package versions
2. Run `pnpm install` to update `web/pnpm-lock.yaml`
3. Update `api/pyproject.toml` for Python dependencies
4. Update `api/uv.lock` with `uv lock`
5. If needed, update `.github/dependabot.yml` configuration
6. Commit with message like `chore: update dependencies`

### Unit Test Module
**Trigger:** When adding test coverage for a new module or improving existing coverage
**Command:** `/add-tests`

1. Create test files in `api/tests/unit_tests/` following the module structure
2. Mirror the directory structure of the code being tested
3. Test all major components and edge cases
4. Example structure:
   ```
   api/tests/unit_tests/controllers/test_user_controller.py
   api/tests/unit_tests/core/test_data_processor.py
   ```
5. Use descriptive test names and group related tests in classes
6. Commit with message like `test: add comprehensive unit tests for user module`

### Bug Fix with Test
**Trigger:** When fixing a bug in core functionality
**Command:** `/fix-bug`

1. Identify and modify the implementation file in `api/dify_graph/nodes/` or `api/core/`
2. Add or update corresponding unit tests in `api/tests/unit_tests/`
3. Ensure tests cover both the bug scenario and expected behavior
4. Run test suite to verify fix doesn't break existing functionality
5. Sometimes add integration tests for complex fixes
6. Commit with message like `fix: resolve data processing edge case with tests`

### Web Component Test Coverage
**Trigger:** When improving test coverage for React components
**Command:** `/add-component-tests`

1. Create `.spec.tsx` files in `web/app/components/` directory
2. Test component rendering, props handling, and user interactions
3. Example test structure:
   ```typescript
   import { render, screen } from '@testing-library/react'
   import { ComponentName } from './ComponentName'

   describe('ComponentName', () => {
     it('should render correctly', () => {
       render(<ComponentName />)
       // assertions
     })
   })
   ```
4. Update `web/eslint-suppressions.json` if needed for test-specific ESLint rules
5. Commit with message like `test: add comprehensive tests for user dashboard components`

### Migrate Tests to Testcontainers
**Trigger:** When converting unit tests to integration tests that require database/external services
**Command:** `/migrate-test`

1. Move test files from `api/tests/unit_tests/services/`
2. Create corresponding files in `api/tests/test_containers_integration_tests/services/`
3. Update test setup to use testcontainers for database/service dependencies
4. Modify test assertions to work with real database interactions
5. Update CI configuration if needed to support integration tests
6. Commit with message like `test: migrate user service tests to testcontainers`

### Feature with Service and Command
**Trigger:** When adding new CLI-accessible functionality
**Command:** `/add-feature-command`

1. Create service class in `api/services/` directory
2. Implement business logic in the service layer
3. Add command interface in `api/commands.py`
4. Create comprehensive unit tests in `api/tests/unit_tests/services/`
5. Example structure:
   ```python
   # api/services/export_service.py
   class ExportService:
       def export_data(self, format: str):
           # implementation
   
   # api/commands.py
   @click.command()
   def export_data():
       service = ExportService()
       service.export_data('json')
   ```
6. Commit with message like `feat: add data export service with CLI command`

## Testing Patterns

### Frontend Testing
- Use **vitest** as the testing framework
- Test files follow `*.spec.tsx` pattern
- Focus on component behavior, props, and user interactions
- Mock external dependencies and API calls

### Backend Testing
- Separate unit tests and integration tests
- Unit tests in `api/tests/unit_tests/` mirror source structure
- Integration tests use testcontainers for database interactions
- Test both happy path and error scenarios

### Test Organization
```
api/
├── tests/
│   ├── unit_tests/
│   │   ├── controllers/
│   │   ├── core/
│   │   └── services/
│   └── test_containers_integration_tests/
│       └── services/
web/
└── app/
    └── components/
        ├── ComponentName.tsx
        └── ComponentName.spec.tsx
```

## Commands

| Command | Purpose |
|---------|---------|
| `/update-deps` | Update package dependencies and lock files |
| `/add-tests` | Add comprehensive unit tests for a module |
| `/fix-bug` | Fix a bug and add corresponding test coverage |
| `/add-component-tests` | Add test coverage for React components |
| `/migrate-test` | Migrate unit tests to testcontainers integration tests |
| `/add-feature-command` | Add new feature with service layer and CLI command |