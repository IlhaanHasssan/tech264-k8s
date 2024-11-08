# ***DEPLOYING THE SPARTA TEST APP AND DATABASE***

- [***DEPLOYING THE SPARTA TEST APP AND DATABASE***](#deploying-the-sparta-test-app-and-database)
  - [***Steps:***](#steps)
    - [***Pre-requisites***](#pre-requisites)

## ***Steps:***
### ***Pre-requisites***
- a working sparta test app image that you can pull from Docker Hub **`docker pull ihassan777/sparta-test-app:v1-no-db`**
- a working mongoDB image, that you can also pull from Docker Hub **`docker pull mongo:7.0.6`**
- a working app homepage, see **[sparta-app-no-db](/tech264-k8s/k8s-yaml-definitions/local-sparta-app-no-db/README.md)** to have your sparta test app homepage running

1. Once you have your sparta test app homepage up and running, you now need to create a database service and deployment.
2. To do this, you will need to create two YAML files, one for the **[mongodb service](mongodb-service.yaml)** and one for the **[database deployment](db-deploy.yaml)**
3. Inside those files, you will find a detailed explanation of how I have structured my database service and deployment. 
4. Run your new files using **`kubectl apply -f <file-name>`** 

![](/tech264-k8s/K8S-images/local-sparta-app-deployment.png)


5. You can check it is running using **`kubectl get deploy`**
6.  Now your posts page should be visible, however it will not be seeded automatically
7.  You need to add the environment variable to your **[`app-deploy`](app-deploy.yaml)**

![](/tech264-k8s/K8S-images/dbhost.png)

8. Your database should be visible when you run **`kubectl get deploy`**

![](/tech264-k8s/K8S-images/deploy-with-db.png)

9. Now you posts page should be populated at **http://localhost:30001/posts**

![](/tech264-k8s/K8S-images/postspgtask3&4kb.png)




