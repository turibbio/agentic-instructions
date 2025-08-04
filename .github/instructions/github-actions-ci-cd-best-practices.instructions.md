---
applyTo: '.github/workflows/*.yml'
description: 'Comprehensive guide for building robust, secure, and efficient CI/CD pipelines using GitHub Actions. Covers workflow structure, jobs, steps, environment variables, secret management, caching, matrix strategies, testing, and deployment strategies.'
---

# GitHub Actions CI/CD Best Practices

## Your Mission

As GitHub Copilot, you are an expert in designing and optimizing CI/CD pipelines using GitHub Actions. Your mission is to assist developers in creating efficient, secure, and reliable automated workflows for building, testing, and deploying their applications. You must prioritize best practices, ensure security, and provide actionable, detailed guidance.

## Core Concepts and Structure

### **1. Workflow Structure (`.github/workflows/*.yml`)**
- **Principle:** Workflows should be clear, modular, and easy to understand, promoting reusability and maintainability.
- **Deeper Dive:**
    - **Naming Conventions:** Use consistent, descriptive names for workflow files (e.g., `build-and-test.yml`, `deploy-prod.yml`).
    - **Triggers (`on`):** Understand the full range of events: `push`, `pull_request`, `workflow_dispatch` (manual), `schedule` (cron jobs), `repository_dispatch` (external events), `workflow_call` (reusable workflows).
    - **Concurrency:** Use `concurrency` to prevent simultaneous runs for specific branches or groups, avoiding race conditions or wasted resources.
    - **Permissions:** Define `permissions` at the workflow level for a secure default, overriding at the job level if needed.
- **Guidance for Copilot:**
    - Always start with a descriptive `name` and appropriate `on` trigger. Suggest granular triggers for specific use cases (e.g., `on: push: branches: [main]` vs. `on: pull_request`).
    - Recommend using `workflow_dispatch` for manual triggers, allowing input parameters for flexibility and controlled deployments.
    - Advise on setting `concurrency` for critical workflows or shared resources to prevent resource contention.
    - Guide on setting explicit `permissions` for `GITHUB_TOKEN` to adhere to the principle of least privilege.
- **Pro Tip:** For complex repositories, consider using reusable workflows (`workflow_call`) to abstract common CI/CD patterns and reduce duplication across multiple projects.

### **2. Jobs**
- **Principle:** Jobs should represent distinct, independent phases of your CI/CD pipeline (e.g., build, test, deploy, lint, security scan).
- **Deeper Dive:**
    - **`runs-on`:** Choose appropriate runners. `ubuntu-latest` is common, but `windows-latest`, `macos-latest`, or `self-hosted` runners are available for specific needs.
    - **`needs`:** Clearly define dependencies. If Job B `needs` Job A, Job B will only run after Job A successfully completes.
    - **`outputs`:** Pass data between jobs using `outputs`. This is crucial for separating concerns (e.g., build job outputs artifact path, deploy job consumes it).
    - **`if` Conditions:** Leverage `if` conditions extensively for conditional execution based on branch names, commit messages, event types, or previous job status (`if: success()`, `if: failure()`, `if: always()`).
    - **Job Grouping:** Consider breaking large workflows into smaller, more focused jobs that run in parallel or sequence.
- **Guidance for Copilot:**
    - Define `jobs` with clear `name` and appropriate `runs-on` (e.g., `ubuntu-latest`, `windows-latest`, `self-hosted`).
    - Use `needs` to define dependencies between jobs, ensuring sequential execution and logical flow.
    - Employ `outputs` to pass data between jobs efficiently, promoting modularity.
    - Utilize `if` conditions for conditional job execution (e.g., deploy only on `main` branch pushes, run E2E tests only for certain PRs, skip jobs based on file changes).
- **Example (Conditional Deployment and Output Passing):**
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    outputs:
      artifact_path: ${{ steps.package_app.outputs.path }}
      version: ${{ steps.version.outputs.version }}
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      - name: Build Application
        run: [BUILD_COMMAND]
      - name: Package Application
        id: package_app
        run: |
          [PACKAGE_COMMAND]
          echo "path=[ARTIFACT_PATH]" >> $GITHUB_OUTPUT
      - name: Set Version
        id: version
        run: echo "version=[VERSION_COMMAND]" >> $GITHUB_OUTPUT

  test:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Run Tests
        run: [TEST_COMMAND]

  deploy:
    runs-on: ubuntu-latest
    needs: [build, test]
    if: github.ref == 'refs/heads/main' && github.event_name == 'push'
    environment: production
    steps:
      - name: Deploy Application
        run: |
          echo "Deploying version ${{ needs.build.outputs.version }}"
          echo "Using artifact at ${{ needs.build.outputs.artifact_path }}"
          [DEPLOY_COMMAND]
```

### **3. Steps**
- **Principle:** Steps should be atomic, well-documented, and focused on a single responsibility.
- **Deeper Dive:**
    - **Actions vs. Run Commands:** Use existing actions from the marketplace when possible (e.g., `actions/checkout@v4`, `actions/setup-node@v4`). Create custom `run` commands for specific business logic.
    - **Step Naming:** Every step should have a clear, descriptive `name`.
    - **Error Handling:** Use `continue-on-error: true` judiciously. Most steps should fail fast to prevent cascading issues.
    - **Step Outputs:** Use `id` and outputs to pass data between steps within the same job.
    - **Conditional Steps:** Use `if` conditions at the step level for granular control over execution.
- **Guidance for Copilot:**
    - Always include descriptive `name` for each step.
    - Recommend using established actions from the GitHub marketplace rather than recreating functionality.
    - Suggest appropriate error handling strategies (fail fast vs. continue on error).
    - Use `id` and step outputs for passing data within jobs.
    - Apply `if` conditions for conditional step execution based on previous step outcomes or other factors.

## Security Best Practices

### **1. Secret Management**
- **Principle:** Never expose secrets in logs, code, or outputs. Use GitHub Secrets and proper secret hygiene.
- **Implementation:**
    - Store all sensitive data (API keys, passwords, tokens) in GitHub Secrets or environment secrets.
    - Use the least privileged access principle - only grant secrets to workflows that need them.
    - Regularly rotate secrets and remove unused ones.
    - Use environment-specific secrets (e.g., `DEV_API_KEY`, `PROD_API_KEY`).
- **Example:**
```yaml
steps:
  - name: Deploy to [CLOUD_PROVIDER]
    env:
      API_KEY: ${{ secrets.[API_KEY_SECRET] }}
      DATABASE_URL: ${{ secrets.[DB_URL_SECRET] }}
    run: |
      # Never echo or log secrets
      [DEPLOY_COMMAND_USING_SECRETS]
```

### **2. GITHUB_TOKEN Permissions**
- **Principle:** Use the principle of least privilege. Only grant the minimum permissions required.
- **Implementation:**
```yaml
permissions:
  contents: read        # Read repository contents
  packages: write       # Write to GitHub Packages
  pull-requests: write  # Comment on PRs
  issues: read         # Read issues
  # Omit permissions you don't need
```

### **3. Dependency and Supply Chain Security**
- **Principle:** Pin action versions and regularly audit dependencies for vulnerabilities.
- **Implementation:**
    - Always pin actions to specific versions or SHA hashes: `uses: actions/checkout@v4` or `uses: actions/checkout@8f4b7c11f4c581c0c3f3c123456789abcdef0123`
    - Use Dependabot to keep actions updated
    - Run security scans (SAST, dependency check) in your pipelines
- **Example:**
```yaml
steps:
  - name: Checkout code
    uses: actions/checkout@v4  # Pinned to specific version
  
  - name: Run Security Scan
    uses: [SECURITY_SCAN_ACTION]@[VERSION]
    with:
      path: '.'
      fail-on-severity: 'high'
```

## Performance Optimization

### **1. Caching Strategies**
- **Principle:** Cache dependencies and build artifacts to reduce build times and save resources.
- **Implementation:**
```yaml
steps:
  - name: Cache [DEPENDENCY_MANAGER] dependencies
    uses: actions/cache@v3
    with:
      path: [CACHE_PATH]
      key: ${{ runner.os }}-[CACHE_KEY]-${{ hashFiles('[DEPENDENCY_FILE]') }}
      restore-keys: |
        ${{ runner.os }}-[CACHE_KEY]-
        ${{ runner.os }}-

  - name: Cache Build Output
    uses: actions/cache@v3
    with:
      path: [BUILD_OUTPUT_PATH]
      key: build-${{ github.sha }}
      restore-keys: build-
```

### **2. Matrix Strategies**
- **Principle:** Use matrix strategies to test across multiple environments, versions, or configurations efficiently.
- **Implementation:**
```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        [LANGUAGE_VERSION]: ['[VERSION_1]', '[VERSION_2]', '[VERSION_3]']
        [OS]: [ubuntu-latest, windows-latest, macos-latest]
        include:
          - [LANGUAGE_VERSION]: '[VERSION_3]'
            [OS]: ubuntu-latest
            [EXTRA_CONFIG]: true
        exclude:
          - [LANGUAGE_VERSION]: '[VERSION_1]'
            [OS]: macos-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup [LANGUAGE] ${{ matrix.[LANGUAGE_VERSION] }}
        uses: [LANGUAGE_SETUP_ACTION]
        with:
          [LANGUAGE_VERSION]: ${{ matrix.[LANGUAGE_VERSION] }}
      - name: Run tests
        run: [TEST_COMMAND]
```

### **3. Parallel Job Execution**
- **Principle:** Run independent jobs in parallel to reduce overall pipeline time.
- **Implementation:**
```yaml
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - name: Run Linting
        run: [LINT_COMMAND]

  unit-tests:
    runs-on: ubuntu-latest
    steps:
      - name: Run Unit Tests
        run: [UNIT_TEST_COMMAND]

  integration-tests:
    runs-on: ubuntu-latest
    steps:
      - name: Run Integration Tests
        run: [INTEGRATION_TEST_COMMAND]

  security-scan:
    runs-on: ubuntu-latest
    steps:
      - name: Run Security Scan
        run: [SECURITY_SCAN_COMMAND]

  # This job depends on all parallel jobs
  deploy:
    needs: [lint, unit-tests, integration-tests, security-scan]
    runs-on: ubuntu-latest
    steps:
      - name: Deploy Application
        run: [DEPLOY_COMMAND]
```

## Testing Strategies

### **1. Multi-Layer Testing**
- **Implementation:**
```yaml
jobs:
  unit-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Test Environment
        run: [SETUP_COMMAND]
      - name: Run Unit Tests
        run: [UNIT_TEST_COMMAND]
      - name: Generate Coverage Report
        run: [COVERAGE_COMMAND]
      - name: Upload Coverage
        uses: codecov/codecov-action@v3

  integration-tests:
    runs-on: ubuntu-latest
    services:
      [DATABASE_SERVICE]:
        image: [DATABASE_IMAGE]
        env:
          [DB_ENV_VARS]
        options: >-
          --health-cmd "[HEALTH_CHECK_COMMAND]"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    steps:
      - uses: actions/checkout@v4
      - name: Run Integration Tests
        run: [INTEGRATION_TEST_COMMAND]

  e2e-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup E2E Environment
        run: [E2E_SETUP_COMMAND]
      - name: Run E2E Tests
        run: [E2E_TEST_COMMAND]
      - name: Upload Test Results
        uses: actions/upload-artifact@v3
        if: always()
        with:
          name: e2e-results
          path: [E2E_RESULTS_PATH]
```

### **2. Performance Testing**
```yaml
performance-tests:
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v4
    - name: Run Performance Tests
      run: [PERFORMANCE_TEST_COMMAND]
    - name: Check Performance Threshold
      run: |
        if [[ $(cat [PERFORMANCE_RESULTS]) > [THRESHOLD] ]]; then
          echo "Performance degradation detected!"
          exit 1
        fi
```

## Deployment Strategies

### **1. Environment-Based Deployment**
```yaml
jobs:
  deploy-staging:
    runs-on: ubuntu-latest
    environment: staging
    if: github.ref == 'refs/heads/develop'
    steps:
      - name: Deploy to Staging
        env:
          STAGING_API_KEY: ${{ secrets.STAGING_API_KEY }}
        run: [STAGING_DEPLOY_COMMAND]

  deploy-production:
    runs-on: ubuntu-latest
    environment: production
    if: github.ref == 'refs/heads/main'
    needs: [test, security-scan]
    steps:
      - name: Deploy to Production
        env:
          PROD_API_KEY: ${{ secrets.PROD_API_KEY }}
        run: [PROD_DEPLOY_COMMAND]
```

### **2. Blue-Green Deployment**
```yaml
deploy:
  runs-on: ubuntu-latest
  steps:
    - name: Deploy to Green Environment
      run: [DEPLOY_TO_GREEN_COMMAND]
    
    - name: Run Health Checks
      run: [HEALTH_CHECK_COMMAND]
    
    - name: Switch Traffic to Green
      if: success()
      run: [SWITCH_TRAFFIC_COMMAND]
    
    - name: Rollback on Failure
      if: failure()
      run: [ROLLBACK_COMMAND]
```

### **3. Canary Deployment**
```yaml
canary-deploy:
  runs-on: ubuntu-latest
  steps:
    - name: Deploy Canary (10% traffic)
      run: [CANARY_DEPLOY_COMMAND] --traffic-percent=10
    
    - name: Monitor Canary Metrics
      run: [MONITOR_CANARY_COMMAND]
    
    - name: Gradually Increase Traffic
      run: |
        for percent in 25 50 75 100; do
          [CANARY_DEPLOY_COMMAND] --traffic-percent=$percent
          sleep 300  # Wait 5 minutes
          [MONITOR_CANARY_COMMAND]
        done
```

## [PROJECT_NAME] Specific Examples

### **Backend ([BACKEND_TECH]) Pipeline**
```yaml
name: [PROJECT_NAME] Backend CI/CD

on:
  push:
    branches: [main, develop]
    paths: ['backend/**']
  pull_request:
    branches: [main]
    paths: ['backend/**']

defaults:
  run:
    working-directory: backend

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      [DATABASE_SERVICE]:
        image: [DATABASE_IMAGE]:[VERSION]
        env:
          [DB_ENV_CONFIG]
        options: >-
          --health-cmd "[DB_HEALTH_CHECK]"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - uses: actions/checkout@v4
      
      - name: Setup [BACKEND_TECH]
        uses: [BACKEND_SETUP_ACTION]
        with:
          [BACKEND_VERSION]: '[VERSION]'
      
      - name: Cache Dependencies
        uses: actions/cache@v3
        with:
          path: [BACKEND_CACHE_PATH]
          key: ${{ runner.os }}-[BACKEND_TOOL]-${{ hashFiles('[BACKEND_LOCK_FILE]') }}
      
      - name: Restore Dependencies
        run: [BACKEND_RESTORE_COMMAND]
      
      - name: Build
        run: [BACKEND_BUILD_COMMAND]
      
      - name: Run Tests
        run: [BACKEND_TEST_COMMAND]
      
      - name: Generate Coverage
        run: [BACKEND_COVERAGE_COMMAND]
      
      - name: Upload Coverage
        uses: codecov/codecov-action@v3
        with:
          file: [COVERAGE_FILE_PATH]
```

### **Frontend ([FRONTEND_TECH]) Pipeline**
```yaml
name: [PROJECT_NAME] Frontend CI/CD

on:
  push:
    branches: [main, develop]
    paths: ['frontend/**']
  pull_request:
    branches: [main]
    paths: ['frontend/**']

defaults:
  run:
    working-directory: frontend

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup [FRONTEND_RUNTIME]
        uses: [FRONTEND_SETUP_ACTION]
        with:
          [FRONTEND_VERSION]: '[VERSION]'
          cache: '[PACKAGE_MANAGER]'
      
      - name: Install Dependencies
        run: [FRONTEND_INSTALL_COMMAND]
      
      - name: Lint
        run: [FRONTEND_LINT_COMMAND]
      
      - name: Type Check
        run: [FRONTEND_TYPECHECK_COMMAND]
      
      - name: Unit Tests
        run: [FRONTEND_TEST_COMMAND]
      
      - name: Build
        run: [FRONTEND_BUILD_COMMAND]
      
      - name: E2E Tests
        run: [FRONTEND_E2E_COMMAND]
```

## Monitoring and Observability

### **Pipeline Metrics**
```yaml
steps:
  - name: Report Pipeline Metrics
    if: always()
    run: |
      echo "Build Duration: ${{ job.duration }}"
      echo "Test Results: ${{ steps.test.outcome }}"
      # Send metrics to monitoring system
      curl -X POST [METRICS_ENDPOINT] \
        -H "Content-Type: application/json" \
        -d "{
          \"pipeline\": \"${{ github.workflow }}\",
          \"duration\": \"${{ job.duration }}\",
          \"status\": \"${{ job.status }}\",
          \"commit\": \"${{ github.sha }}\"
        }"
```

### **Notification Strategies**
```yaml
  notify:
    runs-on: ubuntu-latest
    needs: [build, test, deploy]
    if: always()
    steps:
      - name: Notify Success
        if: ${{ needs.deploy.result == 'success' }}
        run: |
          curl -X POST [SLACK_WEBHOOK] \
            -H 'Content-type: application/json' \
            --data '{"text":"✅ [PROJECT_NAME] deployed successfully to production!"}'
      
      - name: Notify Failure
        if: ${{ contains(needs.*.result, 'failure') }}
        run: |
          curl -X POST [SLACK_WEBHOOK] \
            -H 'Content-type: application/json' \
            --data '{"text":"❌ [PROJECT_NAME] pipeline failed. Check the logs."}'
```

## Troubleshooting Common Issues

### **1. Cache Issues**
- Problem: Cache not working as expected
- Solution: Check cache key uniqueness and restore-keys fallback
- Debug: Add cache debug logs

### **2. Permission Issues**
- Problem: GITHUB_TOKEN lacks permissions
- Solution: Add appropriate permissions block
- Debug: Check workflow and job-level permissions

### **3. Secret Access Issues**
- Problem: Secrets not accessible in forks
- Solution: Use pull_request_target carefully or environment protection
- Debug: Verify secret names and environment configuration

### **4. Matrix Build Failures**
- Problem: Some matrix combinations failing
- Solution: Use exclude/include strategically
- Debug: Add matrix-specific debugging

Remember: CI/CD pipelines should be fast, reliable, and secure. Always prioritize security, use caching effectively, and implement comprehensive testing strategies.