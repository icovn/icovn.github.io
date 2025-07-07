---
layout: post
title: How do we scale job based containers in K8S?
categories:
- DevOps
- K8S
tags:
- DevOps
- K8S
---

### Overview
- The module consumes messages from Kafka to generate PDF files.
- Use Kafka is a must because of the existing resources (infrastructure, operator, developer).
- The existing K8S uses the default HPA, we can't add any other tools to it.
- The HPA configuration for the module is: min replicas 2, max replicas 10, CPU or RAM reach 80% to scale up

### Issue
The containers didn't scale to 10 during the load test, it scaled to 5

### Problem
- We didn't understand correctly how K8S uses HPA to scale containers. To scale from 2 pods to 10 pods the average CPU must reach 250% as the following formular
```txt
desiredReplicas = ceil[currentReplicas * currentMetric / targetMetric]
desiredReplicas = ceil(2 * 250 / 50) = ceil(10) = 10
```
- We didn't understand our module, it never reaches 100% CPU average because it processes message one-by-one

### Solution
- Reduce the average CPU and RAM from 80% to 40%
- Update the scaleUp policy using "selectPolicy: Max", "stabilizationWindowSeconds: 0" and "periodSeconds: 15" to raise the number of pods quickest

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: generate-pdf
  namespace: documents
spec:
  # HPA controls the lab-generate-pdf Deployment
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: lab-generate-pdf
  # HPA will scale between 2 and 10 pods based on CPU and memory utilization
  minReplicas: 2
  maxReplicas: 10
  # If average CPU or memory utilization across pods exceeds 40%, the HPA will try to scale up
  # Both CPU and memory are considered independently
  # desiredReplicas = ceil[currentReplicas * currentMetric / targetMetric]
  metrics:
    - resource:
        name: cpu
        target:
          averageUtilization: 40
          type: Utilization
      type: Resource
    - resource:
        name: memory
        target:
          averageUtilization: 40
          type: Utilization
      type: Resource
  behavior:
    # HPA checks every 15 seconds and is allowed to scale up by up to 100 pods (but maxReplicas = 10, so it’s a soft cap)
    scaleUp:
      policies:
        - periodSeconds: 15
          type: Pods
          value: 100
      selectPolicy: Max # selectPolicy: Max -> means the most aggressive scaling (max possible)
      stabilizationWindowSeconds: 0 # stabilizationWindowSeconds: 0 -> No delay; it can scale up immediately
    # HPA evaluates scale-down every 60 seconds
    # It can reduce by max 2 pods or 25% of current pods (whichever is smaller due to selectPolicy: Min
    scaleDown:
      policies:
        - periodSeconds: 60
          type: Pods
          value: 2
        - periodSeconds: 60
          type: Percent
          value: 25
      selectPolicy: Min
      stabilizationWindowSeconds: 300 # stabilizationWindowSeconds: 300: After CPU/memory drops, it waits 5 minutes before scaling down to avoid flapping

```