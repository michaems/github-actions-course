# GitHub Actions Cheat Sheet

Quick reference for common GitHub Actions patterns and syntax.

## Basic Workflow Structure

```yaml
name: Workflow Name

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  job-name:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    - name: Step name
      run: echo "Hello World"
```

## Common Triggers (on)

```yaml
# Push to specific branches
on:
  push:
    branches: [ main, develop ]

# Pull requests
on:
  pull_request:
    branches: [ main ]

# Multiple events
on: [push, pull_request]

# Manual trigger
on:
  workflow_dispatch:

# Scheduled (cron)
on:
  schedule:
    - cron: '0 2 * * *'  # Daily at 2 AM

# Release
on:
  release:
    types: [published]

# Path-based triggers
on:
  push:
    paths:
      - 'src/**'
      - '!docs/**'
```

## Runners

```yaml
# GitHub-hosted runners
runs-on: ubuntu-latest
runs-on: windows-latest
runs-on: macos-latest

# Specific versions
runs-on: ubuntu-20.04
runs-on: windows-2022
runs-on: macos-12

# Self-hosted
runs-on: self-hosted

# Matrix
runs-on: ${{ matrix.os }}
```

## Steps

```yaml
steps:
  # Use an action
  - uses: actions/checkout@v3
  
  # Run a command
  - name: Run command
    run: echo "Hello"
  
  # Multi-line command
  - name: Multi-line
    run: |
      echo "Line 1"
      echo "Line 2"
  
  # Working directory
  - name: In directory
    run: npm install
    working-directory: ./frontend
  
  # Continue on error
  - name: Might fail
    run: npm test
    continue-on-error: true
  
  # Timeout
  - name: With timeout
    run: npm install
    timeout-minutes: 10
```

## Job Dependencies

```yaml
jobs:
  job1:
    runs-on: ubuntu-latest
    steps:
      - run: echo "First"
  
  job2:
    needs: job1  # Waits for job1
    runs-on: ubuntu-latest
    steps:
      - run: echo "Second"
  
  job3:
    needs: [job1, job2]  # Waits for both
    runs-on: ubuntu-latest
    steps:
      - run: echo "Third"
```

## Matrix Strategy

```yaml
jobs:
  test:
    strategy:
      matrix:
        node: [16, 18, 20]
        os: [ubuntu-latest, windows-latest]
      fail-fast: false
    runs-on: ${{ matrix.os }}
    steps:
      - uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node }}
```

## Environment Variables

```yaml
# Workflow level
env:
  GLOBAL_VAR: value

jobs:
  job:
    # Job level
    env:
      JOB_VAR: value
    
    steps:
      # Step level
      - name: Step
        env:
          STEP_VAR: value
        run: echo $GLOBAL_VAR $JOB_VAR $STEP_VAR
      
      # Using secrets
      - name: Use secret
        env:
          API_KEY: ${{ secrets.API_KEY }}
        run: curl -H "Authorization: $API_KEY" api.example.com
```

## Conditionals

```yaml
# If expressions
- name: Conditional step
  if: github.event_name == 'push'
  run: echo "On push"

# Common conditions
if: success()                          # Previous steps succeeded
if: failure()                          # Previous step failed
if: always()                           # Always run
if: cancelled()                        # Workflow was cancelled

# Multiple conditions
if: github.ref == 'refs/heads/main' && success()

# Check event type
if: github.event_name == 'pull_request'

# Check actor
if: github.actor == 'dependabot[bot]'

# Check step outcome
if: steps.tests.outcome == 'success'
```

## Caching

```yaml
# Built-in npm cache
- uses: actions/setup-node@v3
  with:
    node-version: '18'
    cache: 'npm'

# Custom cache
- uses: actions/cache@v3
  with:
    path: node_modules
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-
```

## Artifacts

```yaml
# Upload
- uses: actions/upload-artifact@v3
  with:
    name: my-artifact
    path: dist/
    retention-days: 30

# Download
- uses: actions/download-artifact@v3
  with:
    name: my-artifact
    path: ./downloaded
```

## Outputs

```yaml
jobs:
  job1:
    outputs:
      output1: ${{ steps.step1.outputs.value }}
    steps:
      - id: step1
        run: echo "value=hello" >> $GITHUB_OUTPUT
  
  job2:
    needs: job1
    steps:
      - run: echo ${{ needs.job1.outputs.output1 }}
```

## Contexts

```yaml
# GitHub context
${{ github.repository }}      # owner/repo
${{ github.ref }}             # refs/heads/main
${{ github.sha }}             # commit SHA
${{ github.actor }}           # user who triggered
${{ github.event_name }}      # push, pull_request, etc.
${{ github.run_number }}      # workflow run number
${{ github.run_id }}          # unique run ID

# Env context
${{ env.MY_VAR }}

# Secrets context
${{ secrets.MY_SECRET }}

# Matrix context
${{ matrix.node }}
${{ matrix.os }}

# Runner context
${{ runner.os }}              # Linux, Windows, macOS
${{ runner.arch }}            # X64, ARM, ARM64
${{ runner.temp }}            # temp directory path

# Steps context
${{ steps.step-id.outputs.value }}
${{ steps.step-id.outcome }}  # success, failure
${{ steps.step-id.conclusion }}

# Needs context
${{ needs.job-id.outputs.output }}
${{ needs.job-id.result }}    # success, failure, cancelled
```

## Functions

```yaml
# contains
if: contains(github.event.head_commit.message, '[skip ci]')

# startsWith / endsWith
if: startsWith(github.ref, 'refs/tags/')

# format
run: echo ${{ format('Hello {0}', github.actor) }}

# join
${{ join(matrix.*, ', ') }}

# toJSON / fromJSON
run: echo '${{ toJSON(github) }}'

# hashFiles
key: ${{ runner.os }}-${{ hashFiles('**/package-lock.json') }}
```

## Service Containers

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    
    services:
      postgres:
        image: postgres:14
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432
    
    steps:
      - run: npm test
```

## Concurrency

```yaml
# Cancel previous runs
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
```

## Permissions

```yaml
permissions:
  contents: read
  pull-requests: write
  issues: write

# Or minimal
permissions: read-all

# Or none
permissions: {}
```

## Workflow Commands

```yaml
# Set output
- run: echo "name=value" >> $GITHUB_OUTPUT

# Set environment variable
- run: echo "NAME=value" >> $GITHUB_ENV

# Add to PATH
- run: echo "/path/to/bin" >> $GITHUB_PATH

# Debug message
- run: echo "::debug::Debug message"

# Notice
- run: echo "::notice::Important message"

# Warning
- run: echo "::warning::Warning message"

# Error
- run: echo "::error::Error message"

# Group logs
- run: |
    echo "::group::My Group"
    echo "Inside group"
    echo "::endgroup::"
```

## Common Actions

```yaml
# Checkout code
- uses: actions/checkout@v3
  with:
    fetch-depth: 0  # Full history

# Setup Node.js
- uses: actions/setup-node@v3
  with:
    node-version: '18'
    cache: 'npm'

# Setup Python
- uses: actions/setup-python@v4
  with:
    python-version: '3.11'

# Cache
- uses: actions/cache@v3
  with:
    path: ~/.cache/pip
    key: ${{ runner.os }}-pip-${{ hashFiles('**/requirements.txt') }}

# Upload artifact
- uses: actions/upload-artifact@v3
  with:
    name: build
    path: dist/

# Download artifact
- uses: actions/download-artifact@v3
  with:
    name: build
```

## Best Practices

✅ **Pin action versions**: `actions/checkout@v3` not `@main`
✅ **Use secrets for sensitive data**: `${{ secrets.TOKEN }}`
✅ **Enable caching**: Speed up workflows
✅ **Use matrix for multiple versions**: Test broadly
✅ **Fail fast when appropriate**: Save time
✅ **Set timeouts**: Prevent hanging jobs
✅ **Use concurrency control**: Avoid duplicate runs
✅ **Name everything clearly**: Jobs, steps, workflows
✅ **Keep workflows focused**: One purpose per workflow
✅ **Use conditionals wisely**: Skip unnecessary work

❌ **Don't log secrets**: Never `echo ${{ secrets.KEY }}`
❌ **Don't use wildcards for action versions**: `@*` is dangerous
❌ **Don't ignore failures**: Handle errors properly
❌ **Don't hardcode values**: Use variables
❌ **Don't make workflows too complex**: Split if needed

## Debugging Tips

```yaml
# Enable debug logging
# Set repository secret: ACTIONS_STEP_DEBUG = true
# Set repository secret: ACTIONS_RUNNER_DEBUG = true

# Print context
- name: Dump GitHub context
  run: echo '${{ toJSON(github) }}'

# Print environment
- name: Print env
  run: env | sort

# Use tmate for SSH debugging
- uses: mxschmitt/action-tmate@v3
  if: failure()
```

## Security

```yaml
# Least privilege permissions
permissions:
  contents: read

# Audit actions before use
# Review action source code
# Use verified actions

# Protect secrets
# Never log secrets
# Use environment secrets
# Rotate regularly

# Pin dependencies
# Use specific versions
# Review dependency updates
```

## Performance Tips

- Use caching aggressively
- Run fast jobs first
- Use matrix for parallel execution
- Cancel redundant workflows
- Minimize checkout depth
- Use artifacts efficiently
- Set appropriate timeouts

---

Keep this cheat sheet handy while building your workflows! 🚀
