# Filebrowser application deployment

This Helm chart deploys the [filebrowser](https://filebrowser.org/) demo application. To access the application UI after installation find the application's Route and follow the URL. Use the admin/admin username and password for the first login, and then optionally change it to suit your needs.

> The installation uses several defaults including the Ceph storage class ```ocs-storagecluster-cephfs```. If Data Foundation is not installed, or you prefer using a different storage class, then this value should be modified in the Helm Chart Form or YAML view before installation.