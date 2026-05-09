---
name: deployment
description: Deployment and release skill. Activate when user says "deploy", "release", "launch", "CI/CD", "pipeline", or "rollback".
license: "MIT"
metadata:
  version: "1.0"
  author: "slc-team"
  tags: "deployment, CI/CD, release, rollback"
---

# Deployment

Deployment and release management, planning deployment processes and executing releases.

## Use Cases

- Planning deployment process and rollback strategy
- Writing or reviewing CI/CD configuration
- Executing production releases
- Handling release failures or rollbacks

## Not Suitable For

- One-time test deployments in development environments
- Pure code development (no deployment needs)
- Infrastructure configuration (belongs to operations)

## Core Capabilities

### Deployment Planning
Design deployment sequence, rollback strategy, and verification steps.

**Method:**
1. Determine deployment order (dependencies)
2. Define rollback trigger conditions
3. Define verification checkpoints

### CI/CD Configuration
Write or review CI/CD pipeline configuration.

**Method:**
1. Analyze deployment environment
2. Design pipeline stages
3. Add quality gates

### Release Execution
Execute deployment and verify release results.

**Method:**
1. Pre-flight checklist
2. Execute deployment
3. Verify health checks

### Rollback Handling
Execute rollback when release fails.

**Method:**
1. Assess rollback necessity
2. Select rollback version
3. Execute rollback
4. Verify service recovery

## Usage

### Trigger
```
/deployment --action plan
/deployment --action deploy
/deployment --action rollback
```

### Input
- Target deployment environment
- Release content list

## Output Format Example

```markdown
## Deployment Overview
- Environment: Production
- Time Window: 2024-01-15 02:00-04:00 UTC
- Version: v2.1.0

## Release Checklist

| Service | Version | Order | Dependencies |
|---------|---------|-------|--------------|
| api-gateway | v2.1.0 | 1 | - |
| auth-service | v1.9.0 | 2 | api-gateway |
| user-service | v3.0.0 | 3 | auth-service |

## Deployment Steps

### Phase 1: Pre-Deployment Check
- [ ] All tests passed
- [ ] Backup completed
- [ ] Monitoring alerts confirmed

### Phase 2: Deploy api-gateway
```bash
kubectl rollout restart deployment/api-gateway
```

### Phase 3: Verification
- [ ] Health check passed
- [ ] Error rate < 0.1%
- [ ] Latency P99 < 200ms

## Rollback Plan

### Trigger Conditions
- Error rate > 1%
- Latency P99 > 500ms

### Rollback Command
```bash
kubectl rollout undo deployment/user-service
```

### Rollback Verification
- [ ] Service recovered
- [ ] Error rate decreased
```
