# Security Enforcement Policy for Consumer Organizations

## 🔒 MANDATORY Security Requirements

All consumer organizations MUST implement the following security measures:

### 1. Organization-Level Enforcement

#### Required Status Checks (Enforced via Rulesets):
```yaml
required_status_checks:
  - security-scan          # MANDATORY - Cannot merge without this
  - vulnerability-check    # MANDATORY - Cannot merge without this  
  - secrets-detection      # MANDATORY - Cannot merge without this
  - compliance-check       # MANDATORY - Cannot merge without this
```

#### Branch Protection Rules:
- ✅ Require pull request reviews (minimum 2)
- ✅ Require signed commits
- ✅ Require linear history
- ✅ Require status checks to pass
- ❌ Allow force pushes (DISABLED)
- ❌ Allow deletions (DISABLED)

### 2. Workflow-Level Enforcement

#### MANDATORY Platform Workflow Usage:
All consumer teams MUST use one of these security-enforced workflows:

```yaml
# CORRECT - Uses mandatory security workflow
jobs:
  secure-pipeline:
    uses: flynn-actions/platform-workflows/.github/workflows/mandatory-security-ci-cd.yml@main
    with:
      environment: production
      node_version: '18'
      # Note: Security cannot be disabled
```

```yaml
# INCORRECT - Custom workflow without mandatory security
jobs:
  custom-pipeline:  # ❌ NOT ALLOWED
    runs-on: ubuntu-latest
    steps:
      - name: Deploy without security  # ❌ BLOCKED by rulesets
        run: echo "This will fail"
```

### 3. Multi-Layer Security Enforcement

#### Layer 1: GitHub Organization Rulesets
- **Purpose**: Prevent ANY code from merging without security checks
- **Scope**: ALL repositories in consumer organization
- **Enforcement**: Cannot be bypassed, even by admins

#### Layer 2: Platform Workflow Design
- **Purpose**: Ensure security steps always run in workflows
- **Scope**: All teams using platform workflows
- **Enforcement**: Security steps have no conditional logic

#### Layer 3: Status Check Requirements
- **Purpose**: Block deployments if security fails
- **Scope**: All branches and pull requests
- **Enforcement**: GitHub prevents merge if checks fail

#### Layer 4: Environment Protection Rules
- **Purpose**: Additional security gates for production
- **Scope**: Production deployments
- **Enforcement**: Manual approval + security validation

### 4. Implementation Guide for Consumer Organizations

#### Step 1: Apply Organization Rulesets
```terraform
resource "github_organization_ruleset" "security_enforcement" {
  name        = "mandatory-security-checks"
  enforcement = "active"
  
  rules {
    required_status_checks {
      required_check { context = "security-scan" }
      required_check { context = "vulnerability-check" }
      required_check { context = "secrets-detection" }
      required_check { context = "compliance-check" }
    }
  }
}
```

#### Step 2: Use Mandatory Security Workflows
```yaml
# In consumer repo .github/workflows/ci-cd.yml
name: Enforced Security Pipeline
on: [push, pull_request]

jobs:
  security-enforced-pipeline:
    uses: flynn-actions/platform-workflows/.github/workflows/mandatory-security-ci-cd.yml@main
    with:
      environment: ${{ github.ref == 'refs/heads/main' && 'production' || 'staging' }}
```

#### Step 3: Configure Environment Protection
```yaml
# Production environment requires:
# - Manual approval from security team
# - All status checks must pass
# - Signed commits required
```

### 5. Monitoring and Compliance

#### Security Metrics Dashboard:
- 📊 Security scan coverage: 100% (enforced)
- 📊 Vulnerability remediation time: < 24 hours
- 📊 Secrets detection rate: 100% (enforced)
- 📊 Compliance violations: 0 (blocked)

#### Audit Trail:
- All security checks logged and auditable
- Failed security checks prevent deployment
- Security approvals tracked and reported

### 6. Enforcement Mechanisms Summary

| Method | Scope | Bypass Possible | Effectiveness |
|--------|-------|----------------|---------------|
| Organization Rulesets | All repos | No (even admins) | 🟢 Maximum |
| Platform Workflows | Teams using platform | No (built-in) | 🟢 Maximum |
| Status Checks | All PRs/merges | No (GitHub enforced) | 🟢 Maximum |
| Environment Protection | Production | Admin override only | 🟡 High |

### 7. What Happens If Teams Try to Bypass?

#### Scenario 1: Team creates custom workflow without security
- **Result**: Organization ruleset blocks merge
- **Message**: "Required status check 'security-scan' is missing"
- **Action**: Must add security checks or use platform workflow

#### Scenario 2: Team tries to disable security in platform workflow
- **Result**: No option to disable (removed from workflow inputs)
- **Message**: Security steps always run
- **Action**: Security cannot be bypassed

#### Scenario 3: Team tries to force push to bypass checks
- **Result**: Ruleset prevents force pushes
- **Message**: "Force pushes are not allowed"
- **Action**: Must go through proper PR process with security checks

### 🎯 Result: 100% Security Enforcement

With this multi-layer approach:
- ✅ **No code can merge** without security validation
- ✅ **No deployments possible** without security approval
- ✅ **No bypass mechanisms** available to development teams
- ✅ **Centralized security management** via platform team
- ✅ **Audit trail** for all security decisions
- ✅ **Consistent enforcement** across all consumer organizations

This ensures that security is not optional but mandatory for all teams.
