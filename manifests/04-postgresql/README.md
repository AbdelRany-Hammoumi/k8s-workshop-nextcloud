# 04 — PostgreSQL (CloudNativePG)

Deploy a managed PostgreSQL cluster (1 primary + 1 replica) using the CloudNativePG
operator. CNPG handles provisioning, replication, failover, and the application
credentials Secret.

## Files in this directory

- [`cluster.yaml.skeleton`](cluster.yaml.skeleton) — the `Cluster` CRD to fill in.

## Where to find the instructions

This step is covered by **Day 2, Steps 1 to 3** in [docs/DAY2.md](../../docs/DAY2.md):

1. Install the CNPG operator (Helm or `kubectl apply`)
2. Create the `Cluster` resource from `cluster.yaml.skeleton`
3. Inspect the auto-generated `<cluster-name>-app` Secret — note which keys hold the
   username, password, host, and database name. You will need them in `06-nextcloud/`.

## Quick check

```bash
kubectl get cluster -n nextcloud
kubectl get pods   -n nextcloud
kubectl get secret -n nextcloud
```

Expected: cluster `Healthy`, 2 pods `Running` (suffixes `-1` and `-2`),
and a Secret named `<cluster-name>-app`.
