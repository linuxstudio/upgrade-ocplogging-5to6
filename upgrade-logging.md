# Upgrading OCP Logging from version 5.x to version 6.x
## Prerequisites
- You should have an OCP cluster installed and
  * able to access its API URL through the `oc` client
  * able to access its Console URL through a normal web browser
  * you have access to a supported object store (AWS S3, Google Cloud Storage, Azure, Swift, Minio, OpenShift Data Foundation).
 
## Install the version 5.x OCP Logging stack
- Follow the steps in the official documentation [Installing Logging and the Loki Operator using the web console](https://docs.redhat.com/en/documentation/openshift_container_platform/4.14/html/logging/cluster-logging-deploying#logging-loki-gui-install_cluster-logging-deploying) and [Installing log storage](https://docs.redhat.com/en/documentation/openshift_container_platform/4.14/html/logging/log-storage-2#installing-log-storage)

For this demonstration we will deploy the Logging stack on an OCP cluster installed on Google Cloud Platform with Google Cloud Storage available as supported object store through the default `standard-csi` storageclass.




