Monitoring is an important aspect of ensuring available workloads. In this project we will delve on the monitoring of a Kubernetes Cluster hosted on the Google Cloud Platform then deploy the Managed Service for Prometheus(to ingest metrics from a simple application) a Google Cloud's fully managed storage and query service for Prometheus metrics. 
**_This service is built on top of Monarch, the same globally scalable data store as Cloud Monitoring._**

_Key highlights of our project:_

- **Deploy the Managed Service for Prometheus to a GKE cluster.**




- **Deploy a Python application to monitor.**

- **Create a Cloud Monitoring dashboard to view metrics collected.**

# Task 1. Setup a Google Kubernetes Engine cluster

- Access GCP Console and open Cloud Shell. Run the command ```gcloud beta container clusters create gmp-cluster --num-nodes=1 --zone Zone --enable-managed-prometheus``` to deploy a standard GKE cluster, which will prompt you to authorize and enable the GKE API: **_In google cloud the first step in a new account while using resources is always to enable the required APIs._**

![image](https://github.com/user-attachments/assets/c6d0c996-8584-4664-a759-90b14ab4a749)

- Run the command ```gcloud container clusters get-credentials gmp-cluster --zone Zone```to authenticate to the cluster:

![image](https://github.com/user-attachments/assets/ebbc0c19-45cf-4c38-afe4-ee9e1872fa32)

# Task 2. Deploy the Prometheus service

Run the command ```kubectl create ns gmp-test``` to create a namespace to do the work in

![image](https://github.com/user-attachments/assets/43e7ff3d-1e4e-4ef2-8aaf-3017900610bd)


# Task 3. Deploy the Test application

Deploy a simple application which emits metrics at the /metrics endpoint:(I'll be using the qwiklabs lab test application provided)

```kubectl -n gmp-test apply -f https://raw.githubusercontent.com/kyleabenson/flask_telemetry/master/gmp_prom_setup/flask_deployment.yaml```

![image](https://github.com/user-attachments/assets/a41bf1cc-2e52-401e-98f6-3a4d1a5bac07)


```kubectl -n gmp-test apply -f https://raw.githubusercontent.com/kyleabenson/flask_telemetry/master/gmp_prom_setup/flask_service.yaml```

![image](https://github.com/user-attachments/assets/1b61b017-4d18-4b99-8dcb-bf251a7db53f)


Verify that this simple Python Flask app is serving metrics with the command ```url=$(kubectl get services -n gmp-test -o jsonpath='{.items[*].status.loadBalancer.ingress[0].ip}')```



 ```curl $url/metrics```

Output will look like

```sh
# HELP flask_exporter_info Multiprocess metric
# TYPE flask_exporter_info gauge
flask_exporter_info{version="0.18.5"} 1.0
```

Tell Prometheus where to begin scraping the metrics from by applying the PodMonitoring file:

```kubectl -n gmp-test apply -f https://raw.githubusercontent.com/kyleabenson/flask_telemetry/master/gmp_prom_setup/prom_deploy.yaml```

![image](https://github.com/user-attachments/assets/7c18f8f1-1a81-4728-9d6d-57835da20355)


Before finishing up here, generate some load on the application with a really simple interaction with the app:

```timeout 120 bash -c -- 'while true; do curl $(kubectl get services -n gmp-test -o jsonpath='{.items[*].status.loadBalancer.ingress[0].ip}'); sleep $((RANDOM % 4)) ; done'```

![image](https://github.com/user-attachments/assets/3fed9468-3e5f-44db-8836-fba2c9dafa3a)


# Task 4. Observe the app via metrics

Use gcloud to deploy a custom monitoring dashboard that shows the metrics from this application in a line chart.

```
gcloud monitoring dashboards create --config='''
{
  "category": "CUSTOM",
  "displayName": "Prometheus Dashboard Example",
  "mosaicLayout": {
    "columns": 12,
    "tiles": [
      {
        "height": 4,
        "widget": {
          "title": "prometheus/flask_http_request_total/counter [MEAN]",
          "xyChart": {
            "chartOptions": {
              "mode": "COLOR"
            },
            "dataSets": [
              {
                "minAlignmentPeriod": "60s",
                "plotType": "LINE",
                "targetAxis": "Y1",
                "timeSeriesQuery": {
                  "apiSource": "DEFAULT_CLOUD",
                  "timeSeriesFilter": {
                    "aggregation": {
                      "alignmentPeriod": "60s",
                      "crossSeriesReducer": "REDUCE_NONE",
                      "perSeriesAligner": "ALIGN_RATE"
                    },
                    "filter": "metric.type=\"prometheus.googleapis.com/flask_http_request_total/counter\" resource.type=\"prometheus_target\"",
                    "secondaryAggregation": {
                      "alignmentPeriod": "60s",
                      "crossSeriesReducer": "REDUCE_MEAN",
                      "groupByFields": [
                        "metric.label.\"status\""
                      ],
                      "perSeriesAligner": "ALIGN_MEAN"
                    }
                  }
                }
              }
            ],
            "thresholds": [],
            "timeshiftDuration": "0s",
            "yAxis": {
              "label": "y1Axis",
              "scale": "LINEAR"
            }
          }
        },
        "width": 6,
        "xPos": 0,
        "yPos": 0
      }
    ]
  }
}
'''
```

**_Once created, navigate to Monitoring > Dashboards to see the newly created ```Prometheus Dashboard Example```_**

![image](https://github.com/user-attachments/assets/f9adf7a1-70c3-47bc-8e4e-d5a80a147da6)

![image](https://github.com/user-attachments/assets/7fdf9d93-6645-4373-b3c1-6c9d17e2dc6b)







