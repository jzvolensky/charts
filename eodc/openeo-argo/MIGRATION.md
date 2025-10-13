# Bitnami Charts Migration Guide

## Background

As of August 28th, 2025, Bitnami has changed its business model and chart distribution strategy. The legacy HTTP-based chart repository at `https://charts.bitnami.com/bitnami` is being deprecated and migrated to a new OCI-based registry.

For more details, see the [Bitnami Secure Images announcement](https://github.com/bitnami/charts/tree/main).

## What Changed

### Before (Deprecated)
```yaml
dependencies:
  - name: redis
    version: 19.5.2
    repository: "https://charts.bitnami.com/bitnami"
  - name: postgresql
    version: 15.5.1
    repository: "https://charts.bitnami.com/bitnami"
```

### After (Current)
```yaml
dependencies:
  - name: redis
    version: 19.5.2
    repository: "oci://registry-1.docker.io/bitnamicharts"
  - name: postgresql
    version: 15.5.1
    repository: "oci://registry-1.docker.io/bitnamicharts"
```

## Prerequisites

- **Helm 3.8.0 or later** is required for OCI registry support
- Check your Helm version: `helm version --short`

## Migration Steps

### For New Installations

1. Ensure you have Helm 3.8.0 or later installed
2. Clone/download the updated chart
3. Build dependencies:
   ```bash
   helm dependency build
   ```
4. Install as normal:
   ```bash
   helm install openeo -n <namespace> -f values.yaml .
   ```

### For Existing Installations

1. Update your local chart repository
2. Update dependencies:
   ```bash
   helm dependency update
   ```
3. Upgrade your release:
   ```bash
   helm upgrade openeo -n <namespace> -f values.yaml .
   ```

## Troubleshooting

### Error: "OCI not supported"
- **Solution**: Upgrade Helm to version 3.8.0 or later

### Error: "failed to download chart"
- **Solution**: Ensure you have internet connectivity and can access Docker Hub registry
- Try manually pulling: `helm pull oci://registry-1.docker.io/bitnamicharts/redis --version 19.5.2`

### Error: "dependency build failed"
- **Solution**: Remove the old Chart.lock file and rebuild:
  ```bash
  rm -f Chart.lock
  helm dependency build
  ```

## What Stays the Same

- Chart functionality remains unchanged
- Same Redis (19.5.2) and PostgreSQL (15.5.1) versions
- All configuration options in `values.yaml` remain compatible
- Deployment behavior is identical

## Additional Resources

- [Helm OCI Support Documentation](https://helm.sh/docs/topics/registries/)
- [Bitnami Charts Repository](https://github.com/bitnami/charts)
- [Docker Hub Bitnami Charts](https://hub.docker.com/u/bitnamicharts)
