# Monitoring Scope

Monitoring in SmartNutrition is used to ensure the detection + nutrition pipeline is reliable, fast, and affordable.

## Current priority: ship fast

Monitoring is not a primary concern for this phase. We keep only minimum observability that protects user-facing reliability.

## MVP monitoring (do now)

- API health: request count, error rate, and p95 latency.
- Inference health: model timeout count and low-confidence rate.
- Worker health: queue depth and job failure count.
- Basic alerting: only for severe reliability regressions.

## Deferred monitoring (do later)

- Detailed product analytics (swap acceptance, correction funnels).
- Full tracing across API -> queue -> inference -> recommendation.
- Cost dashboards and advanced SLO burn-rate alerts.
- Environment-level capacity forecasting.

## Suggested immediate thresholds

- `HighAPIErrorRate`: error rate > 5% for 5 minutes.
- `SlowInferenceP95`: inference p95 > 1.5s for 10 minutes.
- `WorkerFailureSpike`: worker failures above baseline for 10 minutes.

## Implementation rule for this phase

If a metric does not help detect an outage, severe latency, or failed inference, defer it.

## Privacy baseline (always on)

- Never emit raw medical profile data in logs/metrics.
- Use redacted or hashed identifiers.
- Keep short retention for raw logs.

## Minimal starter stack

- Metrics: Prometheus-compatible endpoint.
- Dashboard: one Grafana board for API + inference + worker health.
- Logs: structured JSON (`request_id`, `endpoint`, `latency_ms`, `status`, `model_version`).

This folder can later hold concrete configs, dashboards, and alert rules when core product delivery is stable.
