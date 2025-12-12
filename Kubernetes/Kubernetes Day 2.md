**Container Communication 
- Every pod has unique IP address and reachable from all other pods in the cluster. 
- When hundreds of port allocation is used for running containers its difficult to keep track of which ports are available. 
- Pod abstraction solves this port allocation problem. Pod gets its own network namespace and a virtual ethernet connection - therefore its a host. Therefore each container has its own ports and don't have to worry about the port mappings with the host machine. 
- Container runtime is also easily replaceable because its all in the pod level. 

**Multiple containers in a pod:
- sometimes used when you need a helper container along with your main application - for example for replication of db pods, authentication gateway, scheduler, etc. 
- Containers can talk to each other via localhost and port because pods are an isolated virtual host with its own network namespace. 
- Pause container in each pod/ sandbox container - reserves and holds the network namespace and makes sure the containers talk to each other. 

**Task 1: Spin up a simple nginx pod, Expose it to a service and access it on localhost (Check what to do for accessing from localhost)
- Create an nginx pod and expose it to the cluster (kubectl apply)
- Create a service to make sure the pod has the same IP address even after it dies. (kubectl expose). 
- To enable curling via localhost -> use port forwarding. Forward a local port to the pod's port and then try to access nginx locally. 
- After this step, you should be able to curl the nginx service on <cluster-ip>:<port> from any node in the cluster and in localhost. 


Task 2: Deploy a MySQL stateful set application with 3 replicas and configure a volume of 5Gi each.
- First, you need to create a config map from a yaml configuration. The replicas should have read-only. Apply the configmap 
- Create a service - one for mysql (headless) the other for the client for connecting to mysql instance for reads. The headless service provides a home for the DNS entries that the statefulset controllers creates for each Pod. Eg if the headless service is named mysql, pods are accessible by resolving <name>.mysql from within any other pod. 
- Create a statefulset. Make sure to include the config maps and the volume mounts as well. 
- Alternative: skip the configmap and just create the service and the statefulset

**Task3:- Create a custom nginx image, push to docker-hub/ECR and pull the image.
- Create a `deployment.yaml` with contains custom-nginx as an image and deploy in k8's cluster.
- Create `service.yaml` route to created pod, expose outside world and verify it.**

- Create a custom nginx image by creating a dockerfile and copying your custom index.html page into /usr/share/nginx/html 
- tag the image and push it to your docker repository 
- Create a deployment.yaml file for deploying the files with a container loading the custom nginx image just created. If the image was pushed to a public repository it can be directly referenced else you need to create a kubernetes secret storing your docker login credentials 
- Create a service.yaml file with your pod created in the deployment script as your selector and select the load balancer type service. Make sure to create a http port with port and target port as 80 for internet exposure
- deploy the deployment and the service scripts. 
- Run the minikube tunnel command. This assigns a public ip to the pod, making it accessible via the internet. Make sure to give the proper sudo password in this minikube tunnel command else curling the external ip will not work. 
- Your pod is now accessible through an external ip and the curl command will display the web page files you had hosted in your custom nginx image!