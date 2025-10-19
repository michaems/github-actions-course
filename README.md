# GitHub Actions Course 🚀

A comprehensive, hands-on course for mastering GitHub Actions. This repository contains practical examples demonstrating various GitHub Actions concepts, from basic CI/CD to advanced patterns.

## 📚 Course Content

This course covers the following topics through working examples:

### 1. Basic CI Workflow (`01-basic-ci.yml`)
- Setting up a basic continuous integration pipeline
- Triggering on push and pull requests
- Running linters, tests, and builds
- Using checkout and setup actions

### 2. Multiple Jobs with Dependencies (`02-multiple-jobs.yml`)
- Creating multiple jobs in a workflow
- Defining job dependencies with `needs`
- Building a pipeline with sequential stages
- Understanding job execution order

### 3. Matrix Strategy (`03-matrix-strategy.yml`)
- Testing across multiple Node.js versions
- Running jobs on different operating systems
- Using matrix to reduce code duplication
- Excluding specific matrix combinations
- Controlling failure behavior with `fail-fast`

### 4. Environment Variables and Secrets (`04-environment-variables.yml`)
- Setting workflow-level, job-level, and step-level variables
- Using GitHub context variables
- Working with secrets securely
- Environment-specific deployments
- Best practices for sensitive data

### 5. Caching Dependencies (`05-caching.yml`)
- Speeding up workflows with caching
- Using built-in cache support in setup actions
- Custom caching strategies
- Cache key patterns and restore keys
- Conditional dependency installation

### 6. Artifacts (`06-artifacts.yml`)
- Uploading and downloading artifacts
- Sharing data between jobs
- Setting artifact retention policies
- Working with coverage reports and build outputs

### 7. Manual Triggers (`07-manual-trigger.yml`)
- Using `workflow_dispatch` for manual execution
- Defining workflow inputs with different types
- Creating interactive workflows
- Choice inputs, boolean flags, and custom strings

### 8. Scheduled Workflows (`08-scheduled.yml`)
- Running workflows on a schedule with cron syntax
- Implementing maintenance tasks
- Periodic health checks and reports
- Understanding cron expressions

### 9. Conditional Execution (`09-conditional-execution.yml`)
- Conditional steps and jobs
- Using GitHub context for conditions
- Event-based conditionals
- Success, failure, and always conditions
- Matrix-based conditionals

### 10. Advanced Patterns (`10-advanced-patterns.yml`)
- Job outputs and data passing
- Service containers (Redis example)
- Concurrency control and cancellation
- Composite actions
- Reusable workflow patterns

## 🚀 Getting Started

### Prerequisites
- Node.js 16+ installed
- Basic understanding of Git and GitHub
- A GitHub account

### Installation

1. Clone this repository:
```bash
git clone https://github.com/michaems/github-actions-course.git
cd github-actions-course
```

2. Install dependencies:
```bash
npm install
```

### Running Locally

Run the tests:
```bash
npm test
```

Run the linter:
```bash
npm run lint
```

Build the project:
```bash
npm run build
```

Run the application:
```bash
npm start
```

## 📖 Learning Path

We recommend following this learning path:

1. **Start with Basic CI** - Understand the fundamentals
2. **Explore Multiple Jobs** - Learn about job orchestration
3. **Try Matrix Strategy** - Scale your testing
4. **Master Environment Variables** - Handle configuration
5. **Implement Caching** - Optimize workflow performance
6. **Work with Artifacts** - Share data between jobs
7. **Use Manual Triggers** - Create on-demand workflows
8. **Set up Schedules** - Automate recurring tasks
9. **Apply Conditionals** - Add logic to your workflows
10. **Study Advanced Patterns** - Tackle complex scenarios

## 🔍 Workflow Triggers

Each workflow can be triggered in different ways:

- **Automatic**: Push or PR to main/master branch
- **Manual**: Use the "Actions" tab → Select workflow → "Run workflow"
- **Scheduled**: Automatically runs based on cron schedule

## 📝 Best Practices

1. **Keep workflows focused** - One workflow per purpose
2. **Use caching** - Speed up your builds
3. **Fail fast when appropriate** - Save compute time
4. **Use matrix strategically** - Test what matters
5. **Protect secrets** - Never log or expose secrets
6. **Name things clearly** - Use descriptive names
7. **Add comments** - Document complex logic
8. **Version your actions** - Use specific versions like @v3

## 🛠️ Project Structure

```
.
├── .github/
│   └── workflows/          # GitHub Actions workflows
│       ├── 01-basic-ci.yml
│       ├── 02-multiple-jobs.yml
│       ├── 03-matrix-strategy.yml
│       ├── 04-environment-variables.yml
│       ├── 05-caching.yml
│       ├── 06-artifacts.yml
│       ├── 07-manual-trigger.yml
│       ├── 08-scheduled.yml
│       ├── 09-conditional-execution.yml
│       └── 10-advanced-patterns.yml
├── src/                    # Source code
│   ├── calculator.js       # Sample module
│   └── index.js           # Main entry point
├── tests/                  # Test files
│   └── calculator.test.js # Unit tests
├── .eslintrc.js           # ESLint configuration
├── jest.config.js         # Jest configuration
├── package.json           # Project dependencies
└── README.md              # This file
```

## 🎯 Sample Application

This repository includes a simple calculator application to demonstrate the workflows in action. The application provides basic arithmetic operations and is fully tested.

## 🤝 Contributing

Feel free to fork this repository and experiment with the workflows. Try:
- Modifying workflow triggers
- Adding new jobs or steps
- Creating custom workflows
- Experimenting with different actions

## 📚 Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax Reference](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)
- [GitHub Actions Marketplace](https://github.com/marketplace?type=actions)
- [Events that trigger workflows](https://docs.github.com/en/actions/reference/events-that-trigger-workflows)

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🌟 Support

If you find this course helpful, please ⭐️ star this repository!

---

Happy Learning! 🎓