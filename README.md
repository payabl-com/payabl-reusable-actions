# Payabl Reusable GitHub Actions

Collection of reusable GitHub Actions workflows for Payabl Open Banking projects.

## Available Workflows

### 1. Node.js CI Pipeline (`nodejs-ci.yml`)

For simple Node.js/TypeScript projects like `ob-cip`.

```yaml
name: CI

on:
  push:
  pull_request:
    branches: [main]

jobs:
  ci:
    uses: payabl-com/payabl-reusable-actions/.github/workflows/nodejs-ci.yml@main
    with:
      node-version: '22.x'
      test-env: 'dev'
      working-directory: '.'
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```

**Parameters:**
- `node-version`: Node.js version (default: '22.x')
- `test-env`: Test environment - local/dev/prod (default: 'dev')
- `skip-build`: Skip build step (default: false)
- `skip-tests`: Skip tests (default: false)
- `working-directory`: Working directory (default: '.')

### 2. NestJS CI Pipeline (`nestjs-ci.yml`)

For NestJS projects like `ob-backend`.

```yaml
name: CI

on:
  push:
  pull_request:
    branches: [main]

jobs:
  ci:
    uses: payabl-com/payabl-reusable-actions/.github/workflows/nestjs-ci.yml@main
    with:
      node-version: '22.x'
      mongodb-version: '7.0'
      coverage-threshold: 80
      working-directory: './ob-backend'
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```

**Parameters:**
- `node-version`: Node.js version (default: '22.x')
- `mongodb-version`: MongoDB version for tests (default: '7.0')
- `skip-build`: Skip build step (default: false)
- `skip-tests`: Skip unit tests (default: false)
- `skip-e2e`: Skip e2e tests (default: false)
- `coverage-threshold`: Coverage threshold % (default: 80)
- `working-directory`: Working directory (default: '.')

### 3. Next.js CI Pipeline (`nextjs-ci.yml`)

For Next.js projects like `ob-banksearch-frontend`.

```yaml
name: CI

on:
  push:
  pull_request:
    branches: [main]

jobs:
  ci:
    uses: payabl-com/payabl-reusable-actions/.github/workflows/nextjs-ci.yml@main
    with:
      node-version: '22.x'
      skip-e2e: false
      working-directory: './ob-banksearch-frontend'
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```

**Parameters:**
- `node-version`: Node.js version (default: '22.x')
- `skip-build`: Skip build step (default: false)
- `skip-tests`: Skip unit tests (default: false)
- `skip-e2e`: Skip e2e tests (default: false)
- `skip-i18n-check`: Skip i18n validation (default: false)
- `working-directory`: Working directory (default: '.')

### 4. Renovate Configuration (`renovate-config.yml`)

Sets up Renovate for dependency management.

```yaml
name: Setup Renovate

on:
  workflow_dispatch:
    inputs:
      project-type:
        description: 'Project type'
        required: true
        type: choice
        options:
          - nodejs
          - nestjs
          - nextjs

jobs:
  setup:
    uses: payabl-com/payabl-reusable-actions/.github/workflows/renovate-config.yml@main
    with:
      project-type: ${{ github.event.inputs.project-type }}
      enable-auto-merge: true
      timezone: 'Europe/Berlin'
      schedule: 'weekly'
```

## Project Setup Examples

### ob-cip
```yaml
# .github/workflows/main.yml
name: CI

on:
  push:
  pull_request:
    branches: [main]

jobs:
  ci:
    uses: payabl-com/payabl-reusable-actions/.github/workflows/nodejs-ci.yml@main
    with:
      test-env: 'dev'
```

### ob-backend
```yaml
# .github/workflows/main.yml
name: CI

on:
  push:
  pull_request:
    branches: [main]

jobs:
  ci:
    uses: payabl-com/payabl-reusable-actions/.github/workflows/nestjs-ci.yml@main
    with:
      working-directory: './ob-backend'
      coverage-threshold: 80
```

### ob-banksearch-frontend
```yaml
# .github/workflows/main.yml
name: CI

on:
  push:
  pull_request:
    branches: [main]

jobs:
  ci:
    uses: payabl-com/payabl-reusable-actions/.github/workflows/nextjs-ci.yml@main
```

## Features

- ✅ **Type checking** with TypeScript
- ✅ **Linting** with ESLint (NestJS/Next.js)
- ✅ **Unit testing** with Jest/Vitest
- ✅ **E2E testing** with Playwright (Next.js) / Jest (NestJS)
- ✅ **Coverage reporting** with Codecov integration
- ✅ **MongoDB services** for NestJS tests
- ✅ **i18n validation** for Next.js projects
- ✅ **Renovate configuration** for dependency management
- ✅ **Configurable parameters** for different environments
- ✅ **Working directory support** for monorepos

## Usage Notes

1. Make sure your project has the required scripts in `package.json`
2. Add secrets like `NPM_TOKEN` to your repository if needed
3. Configure Renovate once per project using the renovate workflow
4. All workflows support monorepo structures via `working-directory`