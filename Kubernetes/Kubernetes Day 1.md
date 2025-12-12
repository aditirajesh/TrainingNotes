**What is Kubernetes?**
- Open source container orchestration framework - by Google 
- Helps manage application made up of hundreds of containers and manage them in different env - physical/cloud/vm 
  
**What problems does it solve?**
- Increased use of containers 
- shift from monolith from microservices 

**Features
- high availability 
- scalability 
- disaster recovery

**Components 
- Worker node: simple server phy/vm 
- pod: smallest unit in kubernetes - abstraction over a container. Creates a running env/layer over the container - abstract the container tech to replace them if you want to / do not need to work directly with the container tech (docker). Meant to run one container in it
- Each pod gets its own IP address and can communicate with one another (an internal IP address). Pods are ephemeral, can die very easily -> a new IP address on creation. 
- Service is used to mitigate the ephemeral IP address problem - permanent IP address attached to each pod . Lifecycle of the service and the pod is not connected so even if the pod dies the IP address will stay. It is also a load balancer
- Endpoints is used to keep track of all the pod IPs behind a service. EndpointSlices were introduced as an alternative to record the IPs as smaller shards/list. Each slice holds a few pod IPs and if a pod dies/new one comes up only the relevant slice needs updating - therefore more faster and scaleable. 
- External service: service that opens comm from external services. HTTP protocol of the node IP address and port number of the service. 
- Internal service: for keeping your db, private data etc 
- Ingress: request first goes to ingress and does the forwarding to the service 
- Config map: contains config of your application, connects this to the pod so that the pod gets the data the config map contains. Prevents rebuilding of images when suppose the url for db connectivity changes. Don't put creds in config map 
- Secret: like config map - but used for storing secret data, stored in a base64 encoded format. This is also connected to the pod for the pod to access the secrets. 
- Volumes - for persistent data storage to make sure data is present even after the pod gets restarted. Storage can be on the local machine/ remote storage (cloud/on-premise not part of kubernetes cluster)
- Replication - to manage downtimes in case the primary pod in a node fails. It is connected to the same service. Service catches the req and load balances between the pods. Blueprints for the pod is used to specify how many replicas are needed for the pods. 
- Deployments - blueprints for the pod, can scale up and scale down the number of pods required. Another abstraction on top of pods. FOR STATELESS.  
- Databases can't be replicated by deployment because databases have a state which is their data. 
- StatefulSet - meant specifically for apps like databases, work similarly to deployments except for dbs. All replicas of a db would have to access a master db storage and to avoid data inconsistencies, proper tracking of which pod is writing into the db, which pod is accessing data etc has to be tracked. StatefulSet helps achieve all of this. FOR STATEFUL. This is however challenging so dbs are often outside of the k8s cluster. 
![[Pasted image 20251111114252.png]]

**K8S Architecture 
- Worker machine/Nodes: has multiple pods with many containers. Worker nodes are the ones that do the actual work. 3 processes have to be run on every node. 1) container runtime 2)kubelet - interacts with both the container runtime and the node itself, responsible for starting a pod with the container inside and assigning it resources. 3) Kube proxy - forwards the request and makes sure communication works with low overhead. 
- Master nodes: manages processes. 4 processes: 1) Api server: cluster gateway -> gets the initial requests into the cluster and queries from the cluster -> acts as a gatekeeper for authentication 2) Scheduler: used to start new pods in the worker nodes -> has an intelligent way of deciding on which worker node to choose to create the new pod. Only decides, the kubelet is the one that actually executes the request. 3) Controller manager - detect state changes of pod -> when pods die -> CM tries to recover the cluster state as soon as possible -> makes a request to the scheduler to start up new pods. 4) etcd - key value store of a cluster state. Cluster changes get stored in etcd. Application data is not stored here. 
- Api server is load balanced and etcd forms a distributed storage across all the master nodes. 
- To add master/worker nodes: get a bare server, install all the kubernetes processes and add it to the cluster. 
![[Pasted image 20251111123133.png]]

**Minikube and kubectl 
- Production cluster setup: multiple master and worker nodes. 
- Minikube - master and node processes run on one machine -> will have the docker container runtime already preinstalled. Creates a virtual box in the laptop and the node will run in the virtual box. This is used for testing kubernetes in your local machine. 
- Kubectl - used for interacting with your cluster. Command line tool for k8s cluster. Api server is the main entry point in the kubernetes cluster. Worker nodes are the one that actually execute the job. Minikube also requires virtualization. 
![[Pasted image 20251111123748.png]]

**Installation of minikube:
- brew install minikube - also install kubectl. Uses the docker desktop virtualization by default. 
- minikube start - spins up a cluster. 