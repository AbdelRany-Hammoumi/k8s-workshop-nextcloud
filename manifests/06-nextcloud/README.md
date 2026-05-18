# 06 — Nextcloud

Deploy Nextcloud as a multi-container pod (PHP-FPM + nginx sidecar) with persistent
storage, environment configuration, admin credentials, and a Service.

## Files in this directory

- [`pvc.yaml.skeleton`](pvc.yaml.skeleton) — PVC for `/var/www/html`
- [`secrets.yaml.skeleton`](secrets.yaml.skeleton) — admin password (do NOT commit real values)
- [`configmap.yaml.skeleton`](configmap.yaml.skeleton) — two ConfigMaps: Nextcloud env vars + nginx.conf
- [`deployment.yaml.skeleton`](deployment.yaml.skeleton) — init container + PHP-FPM + nginx sidecar
- [`service.yaml.skeleton`](service.yaml.skeleton) — ClusterIP targeting nginx port 80

## Where to find the instructions

This step is covered by **Day 2, Steps 5 to 9** in [docs/DAY2.md](../../docs/DAY2.md).

The Nextcloud deployment is the most demanding part of Day 2. Watch out for:
- The init container ownership fix (`chown -R 33:33 /var/www/html`) — without it,
  PHP-FPM cannot write and Nextcloud fails on first run
- The CNPG Secret key names do **not** map 1:1 to the env vars Nextcloud expects —
  bridge them explicitly with `secretKeyRef`
- Both `fastcgi_pass` and `SCRIPT_FILENAME` in the nginx ConfigMap contain deliberate
  errors — they need to be fixed before nginx can talk to PHP-FPM
- Nextcloud is slow on first start (1–3 min) — set generous probe `initialDelaySeconds`
  and `failureThreshold`

## Quick check

```bash
kubectl get pods -n nextcloud           # nextcloud-* should be 2/2 Running
kubectl get pvc  -n nextcloud           # all Bound
kubectl get svc  -n nextcloud           # ClusterIP service for nginx port 80
```
