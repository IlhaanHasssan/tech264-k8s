# ***DEPLOYING NGINX***

- [***DEPLOYING NGINX***](#deploying-nginx)
    - [***Pre-requisites***](#pre-requisites)
    - [***Steps:***](#steps)
      - [***Different editing methods for yaml file to change deployment***](#different-editing-methods-for-yaml-file-to-change-deployment)

### ***Pre-requisites***
- Kubernetes downloaded and the service running, see [download k8s](/tech264-k8s/download-kubernetes.md) for further instructions

### ***Steps:***
1. Once, you have downloaded Kubernetes using **[these instructions](./download-kubernetes.md)**, we can now check if NGINX will run
2. create a repo named **`k8s-yaml-definitions`** to house all of our yaml files
3. Within that repo, create a repo named **`local-nginx-deploy`** and within that create two yaml files named **[`nginx-deploy.yaml`](/tech264-k8s/k8s-yaml-definitions/local-nginx-deploy/nginx-deploy.yaml)** and **[`nginx-service.yaml`](/tech264-k8s/k8s-yaml-definitions/local-nginx-deploy/nginx-service.yaml)**. You will find a detailed explanation for each command in those files.
4. Run the **`kubectl create -f <file-name>`** command for both files

![alt text](/tech264-k8s/K8S-images/createt1.png)

#### ***Different editing methods for yaml file to change deployment***
5. You can edit your yaml file configuration on the fly with a few different methods
6. Firstly, you can use this command


![alttext](/tech264-k8s/K8S-images/editdeployt1.png)

7. This will bring up a notepad with the yaml file


![alttext](/tech264-k8s/K8S-images/notepadt1.png)

1. You can edit the file and apply the changes to see them change in real-time, I changed the number of replicas to 4 and this was the result when I checked the pods using **`kubectl get pods`**, as you can see there are now 4 pods

![alttext](/tech264-k8s/K8S-images/4podst1.png)

9. You cann also edit the file using **`nano <file-name>`**
10. Here you can see I have changed the number of pods from 4 to 5:


![alttext](/tech264-k8s/K8S-images/nginxdeploytask1.png)

<br>

![alttext](/tech264-k8s/K8S-images/getpodst1.png)


11. You can use the command below to scale, here was have scaled up from 5 to 6:

![alttext](/tech264-k8s/K8S-images/kbscaletask1.png)

12. Pods are ephemeral, meaning they are easily deleted, re-created or modified. However, if you specify a number of replicas, that will be the minimum number of pods that exist at all times. If i delete a pod, it will be replaced by another one *(pay attention to the pod names below to see how one was deleted and replaced by another)*


![alttext](/tech264-k8s/K8S-images/rmpodsrecreate.png)

1.  If you wish to delete successfully, you must delete the deployment

![alttext](/tech264-k8s/K8S-images/delete-nginxtask1.png)