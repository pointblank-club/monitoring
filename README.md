# pointblank-club/monitoring

Monitoring tooling, dashboards, and alerting configurations for PointBlank Club infrastructure. This repository collects exporters, Prometheus rules, Grafana dashboards, alertmanager configs, and automation that support observability for our services and infrastructure.

- Status: Draft — please review and adapt to your environment before deploying to production.
- Maintainers: pointblank-club observability team

## Contents
- /prometheus — Prometheus scrape configs, recording and alerting rules
- /grafana — Grafana dashboards (JSON)
- /exporters — Custom or hosted-exporter deployment manifests
- /alerts — Alertmanager configuration and templates
- /playbooks — Operational runbooks and troubleshooting steps
- /terraform or /k8s — (optional) infrastructure-as-code for deploying monitoring components

(Adjust paths above to match repository layout if different.)

## Goals
- Provide a single source of truth for our monitoring configuration
- Make dashboards and alerts consistent across environments
- Enable easy iteration on alerting rules and dashboards with code review and testing
- Document runbooks and expected on-call behaviour

## Features
- Prometheus rules for SLOs, service health, and infrastructure metrics
- Grafana dashboards for service performance, resource utilization, and errors
- Alertmanager config for routing alerts to on-call channels
- Exporter and scrape config examples for common services (node_exporter, blackbox, cAdvisor, etc.)

## Quickstart

Prerequisites
- Prometheus (v2.XX+)
- Grafana (v8+ recommended)
- Alertmanager (v0.XX+)
- kubectl / helm / or Terraform depending on deployment method
- Access to the repository and appropriate secrets for production deployment

Local testing (example using Docker Compose)
1. Clone the repo:
   git clone https://github.com/pointblank-club/monitoring.git
2. Review and update config files in `/prometheus` and `/alertmanager` for local paths and targets.
3. Start a local Prometheus + Grafana stack (example tooling is not included by default; pick your preferred compose file).
4. Load dashboards into Grafana (Import JSON from `/grafana`).

Deploying to Kubernetes (example)
- Use Helm or kustomize to apply Prometheus and Alertmanager manifests.
- Ensure scrape configs point to the correct service discovery endpoints.
- Apply Grafana dashboards as ConfigMaps or use a dashboard provisioning mechanism.

Example (pseudo):
kubectl apply -k ./k8s/prometheus
kubectl apply -k ./k8s/grafana
kubectl apply -k ./k8s/alertmanager

Note: Replace the example paths above with the repository's actual k8s/helm artifacts if present.

## Configuration

Prometheus
- Update `prometheus.yml` (or the equivalent Helm values) with proper `external_labels`, `scrape_configs`, and `alerting` configuration.
- Keep recording rules in `/prometheus/recording_rules.yml`.
- Keep alerting rules in `/prometheus/alerting_rules.yml`.

Alertmanager
- Configure route and receiver definitions in `/alerts/alertmanager.yml`.
- Use templates in `/alerts/templates/` for alert notifications.

Grafana
- Dashboards are stored as JSON in `/grafana`. Import them or provision via Grafana provisioning.

Secrets & Credentials
- Do NOT commit sensitive credentials or API keys. Use your secret management solution (SealedSecrets, SOPS, Vault, etc.) for production.

## Alerts and Escalation
- Alerts should be actionable and include runbook pointers.
- Follow the on-call escalation policy defined in `/playbooks/oncall.md` (if present).
- Tune alert thresholds to reduce noise; favor recording rules for expensive queries.

## Development & Testing
- Use a CI pipeline to lint JSON dashboards, validate Prometheus rule syntax, and run any unit tests for exporters.
- Keep PRs small and focused: one dashboard, one alert, or one config change per PR.
- Include screenshots or test evidence for dashboard changes.

Suggested checks
- grafonnet or grafana-jsonnet linting (if used)
- promtool check rules /prometheus/alerting_rules.yml
- jsonlint for dashboard files

## Contributing
Please read CONTRIBUTING.md for details on how to contribute, the PR process, and coding conventions.

## License
Specify the repository license here (e.g., MIT, Apache-2.0). Add a LICENSE file if missing.

## Acknowledgements
- Prometheus, Grafana, Alertmanager communities
- Any internal teams or external open-source projects used
