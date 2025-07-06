---
layout: post
title: Cloud Free central logging with Grafana Loki
categories:
- DevOps
- Logging
tags:
- Logging
- Loki
- Grafana
- Cloud
- DevOps
---

### Set up
- Create a [Grafana free account](https://grafana.com/auth/sign-up/create-user)
- Create a new "Cloud access policies" at "Administration" -> "Users and access" -> "Cloud access policies" -> "Create access policy"
    + Status: Active
    + Scopes: metrics - Write, logs - Write, traces - Write
![Create access policy](/images/2025/07/06/2025-7-6-Cloud-free-central-logging_create-access-policy.png)    
- Create a new token at "Administration" -> "Users and access" -> "Cloud access policies" -> "Add token" under the newly created policy above    

### Test
- Get **URL** and **User** at "Connections" -> "Data sources" -> "grafanacloud-USERNAME-logs"
![Connection information](/images/2025/07/06/2025-7-6-Cloud-free-central-logging_entry-information.png)
- The timestamp can be generated using the following script with Postman
```shell
const timestamp = Date.now();
console.log("Current Unix timestamp (ms):", timestamp);
pm.environment.set("current_timestamp_ms", timestamp * 1000 * 1000);
```
![Postman timestampt body](/images/2025/07/06/2025-7-6-Cloud-free-central-logging_postman2.png)
![Postman authorization](/images/2025/07/06/2025-7-6-Cloud-free-central-logging_postman1.png)
- Send a sample log using HTTP
```shell
curl --location 'https://logs-prod-020.grafana.net/loki/api/v1/push' \
--header 'Content-Type: application/json' \
--header 'Authorization: Basic USERNAME_AND_PASSWORD_HASHED' \
--data '{
    "streams": [
        {
            "stream": {
                "foo": "bar2"
            },
            "values": [
                [
                    "1751776130789000000",
                    "fizzbuzz24"
                ]
            ]
        }
    ]
}'
```
- Send a complex log
![Complex log Postman](/images/2025/07/06/2025-7-6-Cloud-free-central-logging_complex-message-postman.png)

### View log
View log at "Drilldown" -> "Logs"
![Complex log Loki](/images/2025/07/06/2025-7-6-Cloud-free-central-logging_complex-message-loki.png)
