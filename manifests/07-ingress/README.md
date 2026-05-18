# 07 — Ingress

Expose Nextcloud externally via Traefik with the hostname `nextcloud.local`.
TLS comes on Day 3 (see [cert-manager/README.md](cert-manager/README.md)).

## Files in this directory

- [`ingress.yaml.skeleton`](ingress.yaml.skeleton) — the `Ingress` resource to fill in
- [`cert-manager/`](cert-manager/) — TLS setup for Day 3

## Where to find the instructions

- **HTTP exposure** (Day 2): covered by **Day 2, Step 10** in [docs/DAY2.md](../../docs/DAY2.md).
- **TLS** (Day 3): covered by **Day 3, Steps 1 to 3** in [docs/DAY3.md](../../docs/DAY3.md)
  and [cert-manager/README.md](cert-manager/README.md) in this directory.

Watch out for:
- `ingressClassName` must match the class Traefik registered (see
  `kubectl get ingressclass`)
- Body size: Nextcloud uploads files — the default 1 MB limit will break uploads.
  Use a Traefik annotation or `Middleware` CRD
- Proxy headers (`X-Forwarded-Proto`, `X-Forwarded-Host`) — Nextcloud uses these to
  build correct redirect URLs
- Add `nextcloud.local` to your hosts file pointing to `127.0.0.1`
  (kind `extraPortMappings` bind 80/443 to the host on Linux, macOS, and WSL2)

## Quick check

```bash
kubectl get ingress -n nextcloud
curl -kv http://nextcloud.local
# Expected: HTTP 302 redirect to /index.php/login, or the login page itself
```
