# Upgrading OCP Logging from version 5.x to version 6.x
## Prerequisites
- You should have an OCP cluster installed and
  * able to access its API URL through the `oc` client
  * able to access its Console URL through a normal web browser
  * you have access to a supported object store (AWS S3, Google Cloud Storage, Azure, Swift, Minio, OpenShift Data Foundation).
 
## Install the version 5.x OCP Logging stack
- Follow the steps in the official documentation [Installing Logging and the Loki Operator using the web console](https://docs.redhat.com/en/documentation/openshift_container_platform/4.14/html/logging/cluster-logging-deploying#logging-loki-gui-install_cluster-logging-deploying) and [Installing log storage](https://docs.redhat.com/en/documentation/openshift_container_platform/4.14/html/logging/log-storage-2#installing-log-storage) and [Loki object storage](https://docs.redhat.com/en/documentation/openshift_container_platform/4.14/html/logging/log-storage-2#logging-loki-storage_installing-log-storage)

For this demonstration we will deploy the Logging stack on an OCP cluster installed on Google Cloud Platform with Google Cloud Storage available as supported object store through the default `standard-csi` storageclass.

As further prerequisites we list the following:
- You created a project on Google Cloud Platform (GCP)
- You created a bucket in the same project.
- You created a service account in the same project for GCP authentication.

### Create an object storage bucket

Go to https://console.cloud.google.com/storage/ and create a bucket with a globally unique name, e.g. "9a4e5d52-logging-loki"

![Google Cloud Platform Bucket](images/deploy-59/00-logging-loki-bucket.png)

### Create an object storage secret

```
$ oc -n openshift-logging \
> create secret generic logging-loki-gcs \
> --from-literal=bucketname="9a4e5d52-logging-loki" \
> --from-file=key.json="./key.json"
secret/logging-loki-gcs created
```

### Install Loki Operator version 5.9

![Loki Operator 1](images/deploy-59/01-loki-operator-5.9.png)

![Loki Operator 2](images/deploy-59/02-loki-operator-5.9.png)

![Loki Operator 3](images/deploy-59/03-loki-operator-5.9.png)

### Install Logging Operator version 5.9

![Logging Operator 1](images/deploy-59/04-logging-operator-5.9.png)

![Logging Operator 2](images/deploy-59/05-logging-operator-5.9.png)

![Logging Operator 3](images/deploy-59/06-logging-operator-5.9.png)

### Switch project to openshift-logging

By navigating to the `Installed Operators` menu, one is presented with a list of operators installed in all namespaces:

![Installed Operators](images/deploy-59/07-installed-operators-5.9.png)

Using the drop-down `Project:` menu we switch view to see only the operators in the `openshift-logging` project:

![Installed Operators Logging](images/deploy-59/08-switch-openshift-logging.png)

### Create LokiStack instance

![All instances of Loki](images/deploy-59/09-all-instances-loki-5.9.png)

We create a LokiStack instance named `logging-loki` and referencing the previously created object storage secret. Please notice that loki sizing used is `1x.demo`. For a full overview of the possible loki deployment sizing and requested resources, please refer to the official documentation [Loki deployment sizing](https://docs.redhat.com/en/documentation/openshift_container_platform/4.14/html/logging/log-storage-2#loki-deployment-sizing_installing-log-storage)

![LokiStack yaml](images/deploy-59/10-lokistack-yaml-5.9.png)

If one has not previously created the referenced secret, the instance appears in status `Degraded`:

![LokiStack Degraded](images/deploy-59/11-lokistack-degraded-5.9.png)

If all the components are fully validated, the instance appears in status `Ready`:

![LokiStack Ready](images/deploy-59/12-lokistack-ready-5.9.png)

### Create OpenShift Logging instance


