# ***HORIZONTAL POD AUTOSCALER***
- [***HORIZONTAL POD AUTOSCALER***](#horizontal-pod-autoscaler)
  - [***Adding HPA to existing sparta test app***](#adding-hpa-to-existing-sparta-test-app)
    - [**Pre-requisites**](#pre-requisites)
    - [***Steps***](#steps)
      - [Downloading apache bench](#downloading-apache-bench)
      - [creating a hpa.yaml file](#creating-a-hpayaml-file)
      - [](#)






## ***Adding HPA to existing sparta test app***

### **Pre-requisites**
- a working sparta test app image that you can pull from Docker Hub **`docker pull ihassan777/sparta-test-app:v1-no-db`**
- a working mongoDB image, that you can also pull from Docker Hub **`docker pull mongo:7.0.6`**
- a working app homepage, see **[sparta-app instructions](/tech264-k8s/k8s-yaml-definitions/local-sparta-app-no-db/README.md)** for a how-to on running your sparta test app homepage running
- a working posts page, see **[sparta-app with database connected instructions](/tech264-k8s/k8s-yaml-definitions/local-sparta-app-no-db/README.md)** for a how-to on deploying your app with a seeded database

### ***Steps***

#### Downloading apache bench
#### creating a hpa.yaml file
#### 














