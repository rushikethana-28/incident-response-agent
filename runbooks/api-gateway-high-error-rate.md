# Runbook: High Error Rate — API Gateway

## Applies To
- Service: `api-gateway`
- Pattern: Cascade failure, Bad deploy
- Trigger: Error rate > 5% for > 2 minutes

## Prerequisites
- kubectl access to production cluster
- Read access to deployment history

## Steps

### 1. Check recent deployments
```bash
kubectl rollout history deployment/api-gateway -n production
```

### 2. Compare error rate to deployment time
If error spike correlates within 10 minutes of a deploy, proceed to rollback.

### 3. [CONFIRM] Roll back deployment
```bash
kubectl rollout undo deployment/api-gateway -n production
```
**⚠️ Requires confirmation — this will revert the last deployment.**

### 4. [MONITOR 5min] Verify error rate recovery
Watch error rate in Grafana dashboard: `api-gateway/errors`.
Expected: error rate returns to < 1% within 5 minutes.

### 5. If rollback does not resolve
Escalate to on-call engineer and open SEV1 incident channel.

## Prevention
- Add canary deployment stage before full rollout
- Set automatic rollback trigger on error rate threshold