# k8s-gitops-monitoring

A local GitOps Kubernetes environment bootstrapped with a single command.
Argo CD (app-of-apps pattern) deploys a VictoriaMetrics + Grafana monitoring
stack and a demo application (**spam2000**) that exposes Prometheus metrics,
scraped automatically and visualized on a Grafana dashboard defined as code.

---

## Repository structure

```text
clusters/minikube/       # Argo CD root app, namespaces and child applications
  apps/                  # monitoring, dashboards, spam2000 apps
helm/spam2000/           # Helm chart for the demo app (Deployment, Service, VMServiceScrape)
dashboards/              # Grafana dashboard provisioned as code
hack/                    # helper scripts: up/down, port-forwards for Argo CD and Grafana
Makefile                 # make up / make down entry points
```

## How it works

- **GitOps:** everything after the Argo CD install is driven from Git — the root
  application points Argo CD at `clusters/minikube/apps`, which defines all other apps.
- **Monitoring:** the VictoriaMetrics stack collects metrics; the app's Helm chart
  ships a `VMServiceScrape`, so new deployments are scraped without manual config.
- **Dashboards as code:** the Grafana dashboard is stored in the repo and
  provisioned automatically.
- **Reproducible environment:** `make up` / `make down` create and fully tear down
  the cluster, so the whole setup can be rebuilt from scratch.
