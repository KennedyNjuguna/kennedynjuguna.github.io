Monitoring is an important aspect of ensuring available workloads. In this project we will delve on the monitoring of a Kubernetes Cluster hosted on the Google Cloud Platform then deploy the Managed Service for Prometheus(to ingest metrics from a simple application) a Google Cloud's fully managed storage and query service for Prometheus metrics. 
**_This service is built on top of Monarch, the same globally scalable data store as Cloud Monitoring._**

_Key highlights of our project:_

- **Deploy the Managed Service for Prometheus to a GKE cluster.**

- **Deploy a Python application to monitor.**

- **Create a Cloud Monitoring dashboard to view metrics collected.**

# Task 1. Setup a Google Kubernetes Engine cluster

- Access GCP Console and open Cloud Shell. Run the command ```gcloud beta container clusters create gmp-cluster --num-nodes=1 --zone Zone --enable-managed-prometheus``` to deploy a standard GKE cluster, which will prompt you to authorize and enable the GKE API: **_In google cloud the first step in a new account while using resources is always to enable the required APIs._**
- Run the command ```gcloud container clusters get-credentials gmp-cluster --zone Zone```to authenticate to the cluster:

# Task 2. Deploy the Prometheus service

Run the command ```kubectl create ns gmp-test``` to create a namespace to do the work in

# Task 3. Deploy the Test application

Deploy a simple application which emits metrics at the /metrics endpoint:(I'll be using the qwiklabs lab test application provided)

```kubectl -n gmp-test apply -f https://raw.githubusercontent.com/kyleabenson/flask_telemetry/master/gmp_prom_setup/flask_deployment.yaml```

```kubectl -n gmp-test apply -f https://raw.githubusercontent.com/kyleabenson/flask_telemetry/master/gmp_prom_setup/flask_service.yaml```

- Verify that this simple Python Flask app is serving metrics with the command ```url=$(kubectl get services -n gmp-test -o jsonpath='{.items[*].status.loadBalancer.ingress[0].ip}')```

 ```curl $url/metrics```

Output will look like

```sh
# HELP flask_exporter_info Multiprocess metric
# TYPE flask_exporter_info gauge
flask_exporter_info{version="0.18.5"} 1.0
```
