---
name: operations
description: Operations and monitoring skill. Activate when user says "operations", "monitoring", "log analysis", "alert handling", "troubleshooting", or "SRE".
license: "MIT"
metadata:
  version: "1.0"
  author: "slc-team"
  tags: "operations, monitoring, incident, SRE"
---

# Operations

Operations and monitoring analysis, ensuring system stability and reliability.

## Use Cases

- System health status analysis
- Log analysis and troubleshooting
- Performance analysis and optimization suggestions
- Security audits

## Not Suitable For

- New feature development
- Code refactoring
- Architecture design

## Core Capabilities

### Monitoring Analysis
Analyze system monitoring data, identify anomalies.

**Method:**
1. Check key metrics (SLO/SLI)
2. Compare with baseline data
3. Identify anomalous patterns

### Incident Troubleshooting
Diagnose root causes and develop remediation plans.

**Method:**
1. Collect symptom information
2. Narrow down problem scope
3. Identify root cause
4. Develop remediation plan

### Performance Optimization
Identify performance bottlenecks and provide optimization suggestions.

**Method:**
1. Analyze performance metrics
2. Identify bottleneck points
3. Evaluate optimization options
4. Implement and verify

### Security Auditing
Check system security status.

**Method:**
1. Check access logs
2. Verify security configurations
3. Scan for vulnerabilities

## Usage

### Trigger
```
/operations --action analyze
/operations --action troubleshoot
/operations --action audit
```

### Input
- System or service name
- Related logs or monitoring data
- Problem description (if any)

## Output Format Example

```markdown
## System Status

### Key Metrics
| Metric | Current | SLO | Status |
|--------|---------|-----|--------|
| Availability | 99.95% | 99.9% | ✅ |
| Latency P99 | 180ms | 200ms | ✅ |
| Error Rate | 0.05% | 0.1% | ✅ |

### Resource Usage
| Service | CPU | Memory | Status |
|---------|-----|-------|--------|
| api-gateway | 45% | 60% | ✅ |
| auth-service | 30% | 40% | ✅ |

## Issues Found

| Severity | Issue | Impact | Recommendation |
|----------|-------|--------|----------------|
| Medium | DB connection pool full | Slow response | Increase pool size |
| Low | Logs too verbose | Storage cost | Adjust log level |

## Incident Analysis (if applicable)

**Problem**: Service response timeout

**Timeline**:
- 14:32 - Monitoring alert triggered
- 14:33 - Check found slow DB query
- 14:35 - Identified missing index
- 14:40 - Added index, recovered

**Root Cause**: Missing composite index

## Recommended Actions

| Action | Priority | Owner | Due Date |
|--------|----------|-------|----------|
| Increase DB pool | P1 | @dev | 2024-01-20 |
| Add composite index | P2 | @dev | 2024-01-22 |
```
