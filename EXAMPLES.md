# Examples and Use Cases

This document provides real-world examples and use cases for each workflow pattern.

## 01. Basic CI - Real-World Examples

**Use Case 1: Web Application CI**
```yaml
- Run ESLint for code quality
- Run Prettier for code formatting
- Run unit tests with Jest
- Run integration tests
- Build production bundle
- Check bundle size
```

**Use Case 2: API Service CI**
```yaml
- Lint API code
- Run unit tests
- Run API integration tests
- Check API documentation
- Validate OpenAPI spec
```

## 02. Multiple Jobs - Real-World Examples

**Use Case 1: Full Stack Deployment**
```yaml
Jobs:
1. Frontend: lint → test → build
2. Backend: lint → test → build
3. Database: migrations check
4. E2E: integration tests (needs: frontend, backend)
5. Deploy: staging deployment (needs: e2e)
```

**Use Case 2: Mobile App Release**
```yaml
Jobs:
1. Code Quality: lint, format check
2. Unit Tests: parallel test suites
3. Build: iOS and Android builds
4. Screenshot Tests: visual regression
5. Deploy: App Store & Play Store (needs: all above)
```

## 03. Matrix Strategy - Real-World Examples

**Use Case 1: Cross-Platform Testing**
```yaml
matrix:
  os: [ubuntu-latest, macos-latest, windows-latest]
  node: [16, 18, 20]
  # Test 9 combinations
```

**Use Case 2: Database Compatibility**
```yaml
matrix:
  database: [postgres, mysql, mongodb]
  version: ['14', '15', '16']  # for postgres
  # Test across multiple DB versions
```

**Use Case 3: Browser Testing**
```yaml
matrix:
  browser: [chrome, firefox, safari, edge]
  viewport: [mobile, tablet, desktop]
```

## 04. Environment Variables - Real-World Examples

**Use Case 1: Multi-Environment Deployment**
```yaml
env:
  NODE_ENV: ${{ github.ref == 'refs/heads/main' && 'production' || 'development' }}
  API_URL: ${{ secrets.PROD_API_URL }}
  
environment: production
```

**Use Case 2: Feature Flags**
```yaml
env:
  ENABLE_NEW_FEATURE: true
  DEBUG_MODE: false
  LOG_LEVEL: info
```

## 05. Caching - Real-World Examples

**Use Case 1: Large Dependencies**
```yaml
# Cache node_modules (hundreds of MB)
# Cache build output
# Cache test coverage
# Reduce 5-minute install to 30 seconds
```

**Use Case 2: Docker Layers**
```yaml
# Cache Docker layers
# Cache build artifacts
# Speed up multi-stage builds
```

**Use Case 3: Gradle/Maven Dependencies**
```yaml
# Cache .gradle or .m2
# Cache downloaded dependencies
# Reduce build time by 70%
```

## 06. Artifacts - Real-World Examples

**Use Case 1: Test Reports**
```yaml
# Upload coverage reports
# Upload test results XML
# Upload screenshots of failures
# Access from GitHub UI
```

**Use Case 2: Build Distributions**
```yaml
# Upload compiled binaries
# Upload Docker images info
# Upload deployment packages
# Download for manual testing
```

**Use Case 3: Documentation**
```yaml
# Upload generated docs
# Upload API specifications
# Upload changelog
# Make available for download
```

## 07. Manual Trigger - Real-World Examples

**Use Case 1: Production Deployment**
```yaml
inputs:
  version: '1.2.3'
  environment: 'production'
  run-smoke-tests: true
  notify-team: true
```

**Use Case 2: Database Operations**
```yaml
inputs:
  operation: ['backup', 'restore', 'migrate']
  database: ['prod-db', 'staging-db']
  dry-run: true
```

**Use Case 3: Release Management**
```yaml
inputs:
  release-type: ['major', 'minor', 'patch']
  changelog-message: 'Release notes'
  create-github-release: true
```

## 08. Scheduled - Real-World Examples

**Use Case 1: Nightly Builds**
```yaml
# cron: '0 2 * * *'  # 2 AM daily
# Run comprehensive test suite
# Generate nightly builds
# Send email with results
```

**Use Case 2: Dependency Updates**
```yaml
# cron: '0 9 * * 1'  # Monday 9 AM
# Check for npm updates
# Create PR with updates
# Run security audit
```

**Use Case 3: Data Cleanup**
```yaml
# cron: '0 0 * * 0'  # Sunday midnight
# Clean old logs
# Archive old data
# Generate weekly reports
```

**Use Case 4: Health Monitoring**
```yaml
# cron: '*/15 * * * *'  # Every 15 minutes
# Ping production endpoints
# Check service health
# Alert if down
```

## 09. Conditional Execution - Real-World Examples

**Use Case 1: Deploy Only on Main**
```yaml
- name: Deploy to production
  if: github.ref == 'refs/heads/main' && github.event_name == 'push'
  run: ./deploy.sh production
```

**Use Case 2: Skip Steps on Draft PRs**
```yaml
- name: Run expensive tests
  if: github.event.pull_request.draft == false
  run: npm run test:e2e
```

**Use Case 3: Different Actions per Branch**
```yaml
- name: Deploy to staging
  if: startsWith(github.ref, 'refs/heads/release/')
  run: ./deploy.sh staging
  
- name: Deploy to production
  if: github.ref == 'refs/heads/main'
  run: ./deploy.sh production
```

## 10. Advanced Patterns - Real-World Examples

**Use Case 1: Microservices Deployment**
```yaml
# Detect changed services
# Build only changed services
# Run tests for affected services
# Deploy with zero downtime
```

**Use Case 2: Monorepo Management**
```yaml
# Path-based triggering
# Selective CI for changed packages
# Cross-package dependency testing
# Coordinated releases
```

**Use Case 3: Blue-Green Deployment**
```yaml
# Deploy to blue environment
# Run smoke tests
# Switch traffic gradually
# Rollback on failure
```

## Cron Schedule Examples

```yaml
# Every day at 2 AM
- cron: '0 2 * * *'

# Every Monday at 9 AM
- cron: '0 9 * * 1'

# Every weekday at 6 AM
- cron: '0 6 * * 1-5'

# Every 4 hours
- cron: '0 */4 * * *'

# First day of every month at midnight
- cron: '0 0 1 * *'

# Every 15 minutes
- cron: '*/15 * * * *'
```

## Best Practices from Real Projects

### 1. Fast Feedback
```yaml
# Run fast jobs first (lint, type check)
# Fail fast on simple errors
# Run expensive tests last
# Use concurrency limits
```

### 2. Security
```yaml
# Never log secrets
# Use environment secrets
# Rotate tokens regularly
# Limit workflow permissions
```

### 3. Cost Optimization
```yaml
# Use caching aggressively
# Cancel redundant runs
# Use self-hosted runners for heavy loads
# Monitor Actions minutes usage
```

### 4. Reliability
```yaml
# Set appropriate timeouts
# Handle flaky tests
# Use retry mechanisms
# Monitor workflow success rates
```

### 5. Maintainability
```yaml
# Use reusable workflows
# Keep workflows DRY
# Document complex logic
# Version actions explicitly
```

## Common Patterns

### Pattern 1: Auto-merge Dependabot PRs
```yaml
if: github.actor == 'dependabot[bot]'
```

### Pattern 2: Skip CI
```yaml
if: "!contains(github.event.head_commit.message, '[skip ci]')"
```

### Pattern 3: PR Labels
```yaml
if: contains(github.event.pull_request.labels.*.name, 'deploy')
```

### Pattern 4: Changed Files
```yaml
- uses: dorny/paths-filter@v2
  id: changes
  with:
    filters: |
      frontend:
        - 'frontend/**'
```

### Pattern 5: Semantic Versioning
```yaml
- name: Determine version
  run: |
    if [[ ${{ github.ref }} == refs/tags/v* ]]; then
      VERSION=${GITHUB_REF#refs/tags/v}
    fi
```

## Anti-Patterns to Avoid

❌ **Running all tests on every commit to any branch**
✅ Run quick tests on feature branches, full suite on main

❌ **No caching, installing from scratch every time**
✅ Use caching to speed up workflows by 5-10x

❌ **One massive workflow file with everything**
✅ Split into focused workflows by purpose

❌ **Hardcoded values everywhere**
✅ Use variables, secrets, and configuration files

❌ **No error handling or retries**
✅ Handle failures gracefully, retry flaky operations

---

These examples demonstrate how the workflows in this course can be applied to real-world projects!
