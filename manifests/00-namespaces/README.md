# 00 — Namespaces

Create the four namespaces the workshop uses to isolate components:
`nextcloud`, `monitoring`, `traefik`, `metallb-system`.

## Files in this directory

- [`namespaces.yaml`](namespaces.yaml) — partially-filled manifest with `# TODO`
  comments. Fill in the namespace names and labels following the hints.

  > Note: unlike the other steps, this file does not use the `.skeleton` suffix.
  > Edit it in place. The TODO comments tell you what to fill in.

## Where to find the instructions

This step is the first part of **Day 1** in [docs/DAY1.md](../../docs/DAY1.md)
(under the cluster bootstrap section).

## Why four namespaces

| Namespace        | What lives here                                                 |
|------------------|-----------------------------------------------------------------|
| `nextcloud`      | The application: Nextcloud pod, PostgreSQL cluster, Redis, PVCs |
| `monitoring`     | kube-prometheus-stack: Prometheus, Grafana, Alertmanager (Day 3) |
| `traefik`        | The Traefik Ingress controller                                  |
| `metallb-system` | The MetalLB controller and speaker DaemonSet                    |

Some tools (Helm charts especially) will create their namespace automatically with
`--create-namespace`. Creating them up-front is still good practice — it makes the
deployment topology explicit and lets you apply RBAC, network policies, or quotas
before any workload lands.

## Apply

```bash
kubectl apply -f namespaces.yaml
```

## Quick check

```bash
kubectl get ns nextcloud monitoring traefik metallb-system
# Expected: all four Active
```
