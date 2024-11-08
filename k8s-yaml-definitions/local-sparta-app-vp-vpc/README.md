# ***PERSISTANT VOLUME AND PERSISTANT VOLUME CLAIM***
- [***PERSISTANT VOLUME AND PERSISTANT VOLUME CLAIM***](#persistant-volume-and-persistant-volume-claim)
  - [***What is the difference Between Persistent Volume (PV) and Persistent Volume Claim (PVC) in Kubernetes?***](#what-is-the-difference-between-persistent-volume-pv-and-persistent-volume-claim-pvc-in-kubernetes)
  - [***Persistent Volume (PV)***](#persistent-volume-pv)
  - [***Persistent Volume Claim (PVC)***](#persistent-volume-claim-pvc)
  - [***Key Differences***](#key-differences)
  - [***Summary***](#summary)
  - [***Adding PV and PVC to existing sparta test app***](#adding-pv-and-pvc-to-existing-sparta-test-app)
    - [**Pre-requisites**](#pre-requisites)
    - [***Steps***](#steps)

## ***What is the difference Between Persistent Volume (PV) and Persistent Volume Claim (PVC) in Kubernetes?***

- In Kubernetes, **Persistent Volume (PV)** and **Persistent Volume Claim (PVC)** work together to provide persistent storage to pods. Here's a breakdown of their differences and how they interact:

## ***Persistent Volume (PV)***

- **Definition**: A Persistent Volume (PV) is a piece of storage provisioned by an administrator or dynamically created by Kubernetes in the cluster.
- **Purpose**: It serves as the actual storage resource in Kubernetes, allowing data to persist independently of pod lifecycles.
- **Configuration**: PVs are configured by specifying attributes such as storage capacity, access modes, and reclaim policies.
- **Lifecycle**: PVs exist independently of any specific pod and can be reused or reclaimed based on the policy set by the administrator.

## ***Persistent Volume Claim (PVC)***

- **Definition**: A Persistent Volume Claim (PVC) is a request for storage made by a Kubernetes user.
- **Purpose**: It allows users to specify the storage size and access mode they need, and Kubernetes will match the PVC to an available PV with compatible specifications.
- **Binding Process**: When a PVC is created, Kubernetes attempts to find an available PV that meets the request and binds the two together.
- **Dynamic Provisioning**: If no suitable PV is available, Kubernetes can dynamically create a new PV to satisfy the PVC, provided dynamic provisioning is configured.

## ***Key Differences***

| Aspect                | Persistent Volume (PV)                              | Persistent Volume Claim (PVC)                         |
|-----------------------|----------------------------------------------------|------------------------------------------------------|
| **Type**             | Storage resource                                    | Request for storage                                  |
| **Created By**       | Admin or dynamically by Kubernetes                  | User or application                                  |
| **Purpose**          | Provides actual storage in the cluster              | Requests storage with specific requirements          |
| **Lifecycle**        | Independent of pods                                 | Tied to a specific workload or pod                   |
| **Matching Process** | Supplies storage if it matches a PVC's requirements | Matches to an available PV based on requirements     |

## ***Summary***

- **PV** is the actual storage volume in the Kubernetes cluster.
- **PVC** is a request to claim a portion of that storage.
- Together, PV and PVC abstract storage provisioning, allowing Kubernetes applications to use persistent storage without requiring users to manage the underlying infrastructure.



## ***Adding PV and PVC to existing sparta test app***

### **Pre-requisites**
- a working sparta test app image that you can pull from Docker Hub **`docker pull ihassan777/sparta-test-app:v1-no-db`**
- a working mongoDB image, that you can also pull from Docker Hub **`docker pull mongo:7.0.6`**
- a working app homepage, see **[sparta-app instructions](/tech264-k8s/k8s-yaml-definitions/local-sparta-app-no-db/README.md)** for a how-to on running your sparta test app homepage running
- a working posts page, see **[sparta-app with database connected instructions](/tech264-k8s/k8s-yaml-definitions/local-sparta-app-no-db/README.md)** for a how-to on deploying your app with a seeded database

### ***Steps***

1. create a new folder inside your **`k8s-yaml-definitions`** folder named **`local-sparta-app-vp-vpc`**
2. Here, you should have your files that we already know are running correctly to produce your sparta test app with a seeded posts page.
3. Now we want to create them with persistent volume, meaning the dummy data on our posts page should remain the same, even if we delete our deployments and re-run them.
4. 














