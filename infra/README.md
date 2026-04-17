# Infrastructure

This project currently keeps infrastructure simple and local-first for fast delivery:

- `docker/`: local development containers and service orchestration.
- `monitoring/`: MVP observability (basic health, latency, and errors only).

`k8s/` is intentionally removed for now to reduce operational overhead during early development.

## Delivery-first principle

Until core features are stable, infra decisions must optimize iteration speed:

1. Keep setup light and reproducible.
2. Track only metrics needed to catch production-breaking issues.
3. Defer advanced dashboards, tracing, and cost analytics to later phases.

## Why monitoring still exists

Monitoring helps the team answer four practical questions quickly:

1. Is the system healthy right now?
2. Is inference fast enough for users?
3. Are recommendation rules producing usable outputs?
4. Are costs and failures trending in the wrong direction?

See `monitoring/README.md` for MVP-now vs later-phase scope.
