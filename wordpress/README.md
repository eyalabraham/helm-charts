# WordPress application deployment

This Helm chart deploys the [WordPress](https://wordpress.com/) application with a MySQL database backend. To access the application UI after installation, find the application's Route and follow the URL. You will have to configure the setup when you access the application for the first time. Simply follow the setup wizard.

## Prerequisites

- OpenShift cluster with appropriate permissions
- Storage class available (default: `ocs-storagecluster-ceph-rbd`)

## Installation

### Using Helm CLI

```bash
# Add the repository
helm repo add eyalabraham https://eyalabraham.github.io/helm-charts/

# Install the chart
helm install my-wordpress eyalabraham/wordpress

# Install with custom values
helm install my-wordpress eyalabraham/wordpress \
  --set mysql.password=mySecurePassword \
  --set wordpress.pvc.size=5Gi \
  --set mysql.pvc.size=2Gi
```

### Using OpenShift Console

1. Navigate to Developer perspective
2. Click on "+Add" → "Helm Chart"
3. Search for "wordpress"
4. Click on the chart and configure values in the form view
5. Click "Install"

## Configuration

The following table lists the configurable parameters:

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of WordPress replicas | `1` |
| `wordpress.image.repository` | WordPress image repository | `wordpress` |
| `wordpress.image.tag` | WordPress image tag | Chart appVersion |
| `wordpress.pvc.storageClassName` | Storage class for WordPress files | `ocs-storagecluster-ceph-rbd` |
| `wordpress.pvc.size` | Storage size for WordPress files | `1Gi` |
| `wordpress.service.type` | Service type | `LoadBalancer` |
| `wordpress.service.port` | Service port | `80` |
| `mysql.image.repository` | MySQL image repository | `mysql` |
| `mysql.image.tag` | MySQL image tag | `5.6` |
| `mysql.password` | MySQL root password | `password` |
| `mysql.pvc.storageClassName` | Storage class for MySQL database | `ocs-storagecluster-ceph-rbd` |
| `mysql.pvc.size` | Storage size for MySQL database | `1Gi` |
| `mysql.service.type` | MySQL service type | `LoadBalancer` |
| `mysql.service.port` | MySQL service port | `3306` |

## Storage Classes

> **Important:** The installation uses the Ceph storage class `ocs-storagecluster-ceph-rbd` by default. If Data Foundation is not installed, or you prefer using a different storage class, modify this value in the Helm Chart YAML view before installation!

## Accessing the Application

After installation:

1. Get the Route URL:
   ```bash
   oc get route <release-name>-wordpress -n <namespace>
   ```

2. Open the URL in your browser
3. Follow the WordPress setup wizard to configure your site

## Security Notes

- The MySQL password is stored in a Kubernetes Secret
- Change the default password in production environments
- The Route uses edge TLS termination for secure access

## Uninstallation

```bash
helm uninstall my-wordpress
```

Note: PersistentVolumeClaims are not automatically deleted. Delete them manually if needed:

```bash
oc delete pvc <release-name>-wordpress <release-name>-wordpress-mysql