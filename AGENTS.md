# AGENTS.md

This file provides guidance to agents when working with code in this repository.

## Repository Structure
- GitHub Pages Helm chart repository hosted at https://eyalabraham.github.io/helm-charts/
- Charts packaged as .tgz files in root directory
- index.yaml is auto-generated and must be regenerated after chart changes

## Chart Packaging & Publishing
```bash
# Package chart (creates .tgz in current directory)
helm package filebrowser/

# Update index.yaml after adding/updating charts
helm repo index . --url https://eyalabraham.github.io/helm-charts/
```

## OpenShift-Specific Patterns
- Charts use OpenShift Route resources (route.openshift.io/v1) instead of standard Ingress
- Default storage class is "ocs-storagecluster-cephfs" (OpenShift Data Foundation)
- Security contexts are commented out in deployment.yaml (lines 29-38) - uncomment if pod security enforcement needed
- values.schema.json enforces required PVC configuration for OpenShift form view

## Non-Standard Template Patterns
- ConfigMap uses subPath mounting to inject .filebrowser.json as a file (not directory)
- Volume names in deployment.yaml use hardcoded "filebrowser-*-vol" instead of template helpers
- Helper template defines app.kubernetes.io/part-of label (non-standard, typically uses instance)
- Config values are converted to JSON using toPrettyJson in config.yaml template

## Chart Validation
- values.schema.json provides form view in OpenShift console
- Storage size pattern enforces format: ^[0-9]+[GM][i]$ (e.g., "10Gi", "1Gi")
- Storage class pattern: ^[a-z0-9-]+$ (lowercase alphanumeric with hyphens only)

## Icon Hosting
- Chart icons must be hosted at https://eyalabraham.github.io/helm-charts/icons/
- Referenced in Chart.yaml icon field for OpenShift catalog display