# 05 — Redis

Deploy Redis as a session lock and cache backend for Nextcloud. Without it,
concurrent Nextcloud requests can corrupt the database.

## Files in this directory

- [`redis.yaml.skeleton`](redis.yaml.skeleton) — `Deployment` + `Service` to fill in.

## Where to find the instructions

This step is covered by **Day 2, Step 4** in [docs/DAY2.md](../../docs/DAY2.md).

Key points to figure out from the skeleton:
- Image and tag (avoid `:latest`)
- `maxmemory` and `maxmemory-policy` flags passed via `command`
- Readiness / liveness probes using a built-in Redis command
- Resource `requests` and `limits` — memory `limit` must be ≥ `maxmemory`

## Quick check

```bash
kubectl get pods -n nextcloud -l app=redis
kubectl exec -n nextcloud <redis-pod> -- redis-cli ping
# Expected: PONG
```
