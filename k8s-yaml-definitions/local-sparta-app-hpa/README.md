# ***HORIZONTAL POD AUTOSCALER***
- [***HORIZONTAL POD AUTOSCALER***](#horizontal-pod-autoscaler)
  - [***Kubernetes Autoscaling Explained***](#kubernetes-autoscaling-explained)
    - [1. **Horizontal Scaling (HPA)**](#1-horizontal-scaling-hpa)
    - [2. **Vertical Scaling (VPA)**](#2-vertical-scaling-vpa)
    - [3. **Cluster Proportional Autoscaler**](#3-cluster-proportional-autoscaler)
    - [4. **Event-Driven Autoscaling (KEDA)**](#4-event-driven-autoscaling-keda)
    - [5. **Scheduled Autoscaling**](#5-scheduled-autoscaling)
    - [6. **Cluster Autoscaling**](#6-cluster-autoscaling)
  - [**Resource Request**](#resource-request)
    - [Pod Disruption Budget](#pod-disruption-budget)
  - [***Adding HPA to existing sparta test app***](#adding-hpa-to-existing-sparta-test-app)
    - [**Pre-requisites**](#pre-requisites)
    - [***Steps to take***](#steps-to-take)
      - [**Downloading Apache Bench**](#downloading-apache-bench)
      - [**Steps to Fix the Metrics**](#steps-to-fix-the-metrics)


## ***Kubernetes Autoscaling Explained***
- Kubernetes autoscaling helps your applications adapt automatically to changes in resource needs. Here’s a breakdown of the main types of autoscaling in Kubernetes:

### 1. **Horizontal Scaling (HPA)**
- **What It Does**: Adds or removes copies (or "replicas") of your app to handle varying workloads.
- **Example**: If there’s a sudden traffic spike, HPA will add more replicas to keep up. When traffic drops, it scales down to save resources.
- **How It Works**: Uses the **HorizontalPodAutoscaler (HPA)**, which monitors metrics like CPU or memory usage to adjust the number of replicas. The HPA requires a **Metrics Server** to gather data about resource usage.

### 2. **Vertical Scaling (VPA)**
- **What It Does**: Adjusts the CPU and memory of existing replicas instead of adding or removing them.
- **Example**: If each replica of your app needs more memory to perform well, VPA increases the memory for each replica. This prevents the need for more replicas while improving performance.
- **How It Works**: Uses the **VerticalPodAutoscaler (VPA)**, which needs to be installed separately. It has different modes:
  - **Auto**: Automatically updates resources and may require restarting.
  - **Recreate**: Restarts replicas if needed to apply the updated resources.
  - **Initial**: Sets resources only at creation; no updates are made afterward.
  - **Off**: Simply provides recommendations without making changes.

### 3. **Cluster Proportional Autoscaler**
- **What It Does**: Adjusts app replicas based on the size of the cluster (number of nodes or cores).
- **Example**: If you add nodes to the cluster, this autoscaler might add more replicas to maintain balance.
- **How It Works**: Useful for apps where replicas should increase as the cluster grows, such as system components that need to scale proportionally.

### 4. **Event-Driven Autoscaling (KEDA)**
- **What It Does**: Scales your app based on external events, like the number of messages in a queue.
- **Example**: If a queue fills up with tasks, KEDA adds replicas to process them quickly, then scales down as the queue empties.
- **How It Works**: **Kubernetes Event Driven Autoscaler (KEDA)** listens for events and can use different event sources to trigger scaling.

### 5. **Scheduled Autoscaling**
- **What It Does**: Scales your app at specific times or during certain periods.
- **Example**: Scale down during the night to save resources, and scale up during business hours when demand is expected to rise.
- **How It Works**: Can be achieved using KEDA’s **Cron scaler**, allowing you to define schedules for scaling.

### 6. **Cluster Autoscaling**
- **What It Does**: Adds or removes nodes from the cluster itself rather than scaling individual apps.
- **Example**: If all your apps need more resources, more nodes are added to support the extra load.
- **How It Works**: This adjusts the cluster size to ensure you have enough infrastructure to handle demand.

Each type of autoscaling serves a different purpose, but they all work to optimize resource use in a Kubernetes cluster and ensure that applications perform well even as demand fluctuates.


## **Resource Request**
- When you configure a Pod, you specify how much of each resource it needs.
The most common resources to specify are CPU and memory, but there are others.
Per Pod you can specify;

- The amount of CPU & memory you expect this Pod needs; the request.
- The amount of CPU & memory you’re allowing the Pod to use; the limit.
- The scheduler takes the resource request into account when determining which node has the resources available to run this Pod.
- When there is not a node available that would fit the Pod’s resource request, the Pod goes to the Pending state.
- The Cluster Autoscaler will notice a Pod is pending because of a lack of resources and acts upon it by adding a new node.

- Configuring the resource request
    - The resource request is configured per Pod like this;
```
resources:
  requests:
    cpu: "200m"
    memory: "128Mi"
```
- To come up with sane values for CPU & memory you can take the following into account;

**`kubectl top pods`** -A shows the actual CPU & memory usage of all Pods.
- Be aware that this is a snapshot of that moment, it’s better to gather this information from a monitoring system that can show the trend over a longer period.
- CPU is a resource that can overbooked, if the actual usage is higher than defined in the resource request then this could result in performance issues but the Pod wouldn’t get evicted because of it.
- Memory can’t be overbooked and is reserved based on the resources request.
- When a Pod is using more memory than configured and the node runs out of memory the scheduler could evict this Pod.

### Pod Disruption Budget

- Pod disruption budgets allow you to configure the number of Pods that can be down simultaneously from voluntary disruptions.
- Voluntary disruptions are mostly triggered by the application owner or cluster administrator.
This happens for example when a deployment is changed or a node is drained.
- The scheduler makes sure that when it’s evicting Pods, it keeps enough Pods running from the same deployment, statefulset or other controllers to don’t exceed the Pod disruption budget.
- The cluster autoscaler is performing cluster administrator actions like draining a node to scale the cluster down. That’s why it’s important to configure these correctly when you want the cluster to autoscale and auto-heal.

- Example of a Pod Disruption Budget that allows for 1 Pod to be unavailable at the same time.

```
apiVersion: policy/v1beta1
kind: PodDisruptionBudget
metadata:
  name: myapp
spec:
  maxUnavailable: 1
  selector:
    matchLabels:
      app: myapp
```



## ***Adding HPA to existing sparta test app***

### **Pre-requisites**
- a working sparta test app image that you can pull from Docker Hub **`docker pull ihassan777/sparta-test-app:v1-no-db`**
- a working mongoDB image, that you can also pull from Docker Hub **`docker pull mongo:7.0.6`**
- a working app homepage, see **[sparta-app instructions](/tech264-k8s/k8s-yaml-definitions/local-sparta-app-no-db/README.md)** for a how-to on running your sparta test app homepage running
- a working posts page, see **[sparta-app with database connected instructions](/tech264-k8s/k8s-yaml-definitions/local-sparta-app-no-db/README.md)** for a how-to on deploying your app with a seeded database

### ***Steps to take***

#### **Downloading Apache Bench**
- You need to :
  1. [Download](https://www.apachelounge.com/download/) x64 binaries from here Apache VS17 binaries and modules download 
  2. Extract the files
  3. Create a .apache folder at the same level you have the .ssh folder
  4. Move extracted content from inside the Apache24 folder into the .apache folder
  5. Create an alias for ab using **`alias ab=~/.apache/Apache24/bin/ab.exe`**

#### **Steps to Fix the Metrics** 
- Run this command in your gitbash window **`kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml`**
- Check Metrics Server Logs: Review the logs for the Metrics - Server to identify any errors:
  - **`kubectl -n kube-system logs deployment/metrics-server`**
- Patch the Metrics Server Deployment: Since the logs previously indicated a TLS certificate validation error, let's ensure the Metrics Server is configured to bypass this validation. You can patch the deployment to add the **`--kubelet-insecure-tls`** argument:
```yaml
kubectl patch deployment metrics-server -n kube-system --type='json' -p='[
  {
    "op": "add",
    "path": "/spec/template/spec/containers/0/args/-",
    "value": "--kubelet-insecure-tls"
  }
]'
```

- Verify the Metrics Server Deployment: After patching, check the status of the Metrics Server deployment again:
**`kubectl get deployment -n kube-system metrics-server`**
- Check Metrics Availability: Once the Metrics Server is running, verify that metrics are available:
**`kubectl top nodes`**
**`kubectl top pods`**
- Add the CPU resource to your **[app-deploy](./app-deploy.yaml)** YAML file to outline CPU as a metric we are tracking
- Monitor HPA: After ensuring the Metrics Server is running and metrics are available, monitor the HPA to see if it starts reporting CPU metrics correctly:
**`kubectl get hpa`**


![](/tech264-k8s/K8S-images/b4hpa.png)

- Overload the CPU using **`ab -n 10000 -c 100 localhost:30001/posts`**

![](/tech264-k8s/K8S-images/overload-cpu.png)

- Once the CPU is overloaded, you should be able to see new pods being made, due to the HPA

![](/tech264-k8s/K8S-images/newhpapods.png)

- Now, when you check the cpu metrics again, it should be higher since we have overloaded it

![](/tech264-k8s/K8S-images/afterhpa.png)















