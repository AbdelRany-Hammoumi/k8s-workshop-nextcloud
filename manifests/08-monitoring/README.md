# 08 — Monitoring (kube-prometheus-stack)

Deploy Prometheus + Grafana + Alertmanager via the kube-prometheus-stack Helm chart,
expose Grafana via Ingress, and add a `PodMonitor` so Prometheus scrapes the
CloudNativePG instance metrics.

## Files in this directory

- [`values.yaml.skeleton`](values.yaml.skeleton) — Helm values override for the chart

## Where to find the instructions

This step is covered by **Day 3, Steps 4 to 7** in [docs/DAY3.md](../../docs/DAY3.md):

- Step 4 — install kube-prometheus-stack with the values from this directory
- Step 5 — access Grafana and explore the default dashboards
- Step 6 — enable TLS on the Grafana Ingress (uses cert-manager from
  [../07-ingress/cert-manager/](../07-ingress/cert-manager/))
- Step 7 — create a `PodMonitor` so Prometheus scrapes CloudNativePG metrics

Watch out for:
- CRDs: pass `--set crds.enabled=true` to the Helm install, otherwise the
  `Prometheus` / `ServiceMonitor` / `PodMonitor` CRDs are missing
- Resource requests: the full stack is heavy. On 8 GB machines, reduce Prometheus
  retention and Grafana persistence sizes in `values.yaml`
- `PodMonitor` must target the correct pod labels exposed by CNPG —
  see CNPG's documentation for the metrics endpoint structure

## Quick check

```bash
kubectl get pods -n monitoring
kubectl get ingress -n monitoring
kubectl get podmonitor -n nextcloud
# Open https://grafana.local in a browser (accept the self-signed cert warning)
```
