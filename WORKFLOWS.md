# GitHub Actions Workflows Summary

## Quick Reference Guide

### Workflow Triggers

| Workflow | Trigger Type | When It Runs |
|----------|-------------|--------------|
| 01-basic-ci.yml | Automatic | Push/PR to main/master |
| 02-multiple-jobs.yml | Automatic | Push/PR to main/master |
| 03-matrix-strategy.yml | Automatic | Push/PR to main/master |
| 04-environment-variables.yml | Automatic | Push/PR to main/master |
| 05-caching.yml | Automatic | Push/PR to main/master |
| 06-artifacts.yml | Automatic | Push/PR to main/master |
| 07-manual-trigger.yml | Manual | workflow_dispatch |
| 08-scheduled.yml | Scheduled | Daily at 2am UTC, Mondays at 9am UTC |
| 09-conditional-execution.yml | Automatic | Push/PR to main/master |
| 10-advanced-patterns.yml | Automatic + Manual | Push/PR to main/master + workflow_dispatch |

### Workflow Complexity

**Beginner Level:**
- `01-basic-ci.yml` - Start here to understand the basics
- `07-manual-trigger.yml` - Learn about manual triggers

**Intermediate Level:**
- `02-multiple-jobs.yml` - Job dependencies
- `03-matrix-strategy.yml` - Parallel testing
- `04-environment-variables.yml` - Configuration management
- `05-caching.yml` - Performance optimization
- `06-artifacts.yml` - Data sharing

**Advanced Level:**
- `08-scheduled.yml` - Automation with cron
- `09-conditional-execution.yml` - Logic and conditions
- `10-advanced-patterns.yml` - Complex patterns

### Key Concepts by Workflow

#### 01. Basic CI
- Single job with multiple steps
- Checkout, setup, install, lint, test, build
- Foundation for all other workflows

#### 02. Multiple Jobs
- Job orchestration with `needs`
- Sequential pipeline: lint → test → build → deploy
- Demonstrates workflow stages

#### 03. Matrix Strategy
- Test across Node.js 16, 18, 20
- Test on Ubuntu, Windows, macOS
- Efficient parallel testing

#### 04. Environment Variables
- Workflow, job, and step-level variables
- GitHub context usage
- Secrets best practices
- Environment-specific deployments

#### 05. Caching
- Built-in npm caching
- Custom cache with cache@v3
- Cache keys and restore keys
- Conditional installation

#### 06. Artifacts
- Upload coverage and build artifacts
- Download artifacts in dependent jobs
- Retention policies (7-30 days)
- Job-to-job data sharing

#### 07. Manual Trigger
- workflow_dispatch inputs
- Choice, boolean, and string inputs
- Interactive workflow execution
- User-initiated deployments

#### 08. Scheduled Workflows
- Cron-based scheduling
- Daily and weekly tasks
- Maintenance automation
- Health checks

#### 09. Conditional Execution
- Event-based conditions
- Step outcome handling
- Always/failure conditions
- Matrix-specific logic

#### 10. Advanced Patterns
- Job outputs
- Service containers
- Concurrency control
- Composite action patterns

### Common Actions Used

| Action | Purpose | Workflows |
|--------|---------|-----------|
| `actions/checkout@v3` | Clone repository | All |
| `actions/setup-node@v3` | Setup Node.js | All |
| `actions/cache@v3` | Cache dependencies | 05, 10 |
| `actions/upload-artifact@v3` | Upload artifacts | 06 |
| `actions/download-artifact@v3` | Download artifacts | 06 |

### Testing the Workflows

1. **Automatic workflows**: Push to main/master branch
2. **Manual workflow**: Go to Actions tab → Select workflow → Run workflow
3. **Scheduled workflow**: Wait for schedule or trigger manually

### Expected Results

All workflows should:
✅ Pass linting (ESLint)
✅ Pass all tests (6 tests in Jest)
✅ Complete build successfully
✅ Execute without errors

### Troubleshooting

**Issue**: Workflow doesn't trigger
- Check branch name matches trigger (main vs master)
- Verify workflow YAML syntax is valid
- Ensure workflow file is in `.github/workflows/`

**Issue**: Tests fail
- Verify Node.js version compatibility
- Check all dependencies are installed
- Review error messages in workflow logs

**Issue**: Manual trigger not available
- Ensure workflow has `workflow_dispatch` trigger
- Check you have write access to repository
- Refresh the Actions page

### Next Steps

1. Fork this repository
2. Make changes to trigger workflows
3. Experiment with workflow modifications
4. Create your own custom workflows
5. Add real deployment targets

### Learning Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)
- [GitHub Actions Marketplace](https://github.com/marketplace?type=actions)
- [Actions Toolkit](https://github.com/actions/toolkit)

---

📝 **Note**: The scheduled workflow (08-scheduled.yml) won't run in forked repositories unless enabled. To test it, use the workflow_dispatch trigger.
