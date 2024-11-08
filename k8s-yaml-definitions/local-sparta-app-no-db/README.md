# ***DEPLOYING THE SPARTA TEST APP WITHOUT THE DATABASE***

- [***DEPLOYING THE SPARTA TEST APP WITHOUT THE DATABASE***](#deploying-the-sparta-test-app-without-the-database)
  - [***Pre-requisites***](#pre-requisites)
  - [***Steps***](#steps)
    - [***Seeding the database manually***](#seeding-the-database-manually)

## ***Pre-requisites***
- a working sparta test app image that you can pull from Docker Hub **`docker pull ihassan777/sparta-test-app:v1-no-db`**
- Kubernetes downloaded and the service running, see [download k8s](/tech264-k8s/download-kubernetes.md) for further instructions


## ***Steps***

1. Create a new repo in your **`k8s-yaml-definitions`** folder named **`local-sparta-app-no-db`**
2. Inside this repo, create two files named **[`app-deploy`](app-deploy.yaml)** and **[`app-service`](app-service.yaml)**
3. You will find a detailed explanation of what each line in the YAML file does when you follow that link.
4. Once your YAML files are ready to run, you can run them using the **`kubectl apply -f <file-name>`** command
5. Result:
   1. You should be able to see the test homepage but not the posts page at **http://localhost:30001**
### ***Seeding the database manually***
1. Seed the database manually by using the **`kubectl exec -it <pod-name> -- sh`** to enter into one of the pods.
2. You may face an error if you are using a windows computer ⚠️ *(may be able to mitigate this using command prompt)*

![alt text](/tech264-k8s/K8S-images/winpty.png)

3. use **`winpty`** at the start of the command to fix this error
4. Once, you are inside the pod, use **`ls`** or **`pwd`** to find your current location within the pod
5. Check if the **DB_HOST** variable is available in the app folder

![alt text](/tech264-k8s/K8S-images/seeding-database-manually.png)

6. You should now be able to see your data on your recent posts page at **http://localhost:30001/posts** *(Note ⛔ once you delete and re-run these files, posts page will no longer work!)*