# Grafana dashboards

This repository contains codified Grafana dashboards. Argo CD renders the Helm
chart at the repository root, and the chart creates one `GrafanaDashboard`
resource for every JSON file in `dashboards/`.

## Adding a dashboard

1. Export the dashboard JSON from Grafana and add it to `dashboards/` using a
   descriptive filename.
2. Keep the dashboard `uid` stable and unique. Do not include credentials,
   hard-coded environment, or account identifiers.
3. Run the local validation commands:

   ```bash
   find dashboards -name '*.json' -print0 | xargs -0 -n1 jq empty
   helm lint .
   helm template grafana-dashboard-resources . --namespace grafana-system
   ```

The chart defaults to the `grafana-system` namespace and selects Grafana
instances labelled `dashboards: aurora`. Both settings can be overridden by
the consuming Argo CD application.