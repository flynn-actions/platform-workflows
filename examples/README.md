# Platform Workflow Usage Examples

This directory contains example workflows showing how consumer organizations should use the platform engineering workflows and actions.

## 🚀 Quick Start for Consumer Teams

### 1. Copy Example Workflow
Choose the appropriate example for your team:
- **Frontend Teams**: Copy `frontend-ci-cd.yml` to `.github/workflows/ci-cd.yml` in your repository
- **Backend Teams**: Copy `backend-ci-cd.yml` to `.github/workflows/ci-cd.yml` in your repository  
- **Container-Only**: Copy `container-only.yml` for simple container builds

### 2. Configure Secrets
Add these secrets to your repository settings:
- `DEPLOY_TOKEN` - Your deployment token
- `SONAR_TOKEN` - SonarQube token (optional)
- `TEAMS_WEBHOOK_URL` - Teams notification webhook (optional)

### 3. Customize Configuration
Modify the workflow inputs to match your application:
- `environment` - Target environment (dev, staging, prod)
- `node_version` - Node.js version for your app
- `run_tests` - Whether to run tests
- `deploy` - Whether to deploy

## 📋 Available Platform Workflows

### Standard CI/CD Pipeline
```yaml
uses: flynn-actions/platform-workflows/.github/workflows/standard-ci-cd.yml@main
```
**Features**: Security scanning, testing, code analysis, deployment, notifications

### Container Build Pipeline  
```yaml
uses: flynn-actions/platform-workflows/.github/workflows/container-build.yml@main
```
**Features**: Multi-platform builds, security scanning, registry push, signing

## 🧩 Available Platform Actions

### Security Scanner
```yaml
uses: flynn-actions/platform-actions/security-scan@v1
```
**Features**: SAST, dependency scanning, secret detection

### Enterprise Deploy
```yaml
uses: flynn-actions/platform-actions/deploy@v1
```
**Features**: Multi-environment deployment, backups, rollback

### Health Check
```yaml
uses: flynn-actions/platform-actions/health-check@v1
```
**Features**: Application validation, database checks, dependency verification

### Notification System
```yaml
uses: flynn-actions/platform-actions/notify@v1
```
**Features**: Teams/email notifications, rich context, status-based formatting

## 🔒 Security Compliance

All examples use only approved platform actions. Consumer organizations are automatically restricted to:
- ✅ `flynn-actions/platform-workflows/*`
- ✅ `flynn-actions/platform-actions/*`
- ❌ GitHub Marketplace actions (blocked)
- ❌ Third-party actions (blocked)

## 📞 Support

For questions or issues:
- **Platform Team**: Create issue in this repository
- **Documentation**: See main README.md
- **Examples**: Review the workflow files in this directory

## 🔄 Updates

The platform team maintains these examples. When new features are added to platform workflows, examples will be updated automatically.
