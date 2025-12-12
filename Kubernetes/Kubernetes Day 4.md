
**Init containers:
- These are containers that are run in the pod before anything else, must always complete before the next container begins. 
- If a pod's init container fails it keeps restarting until it succeeds. 
- But, if the pod has a restartPolicy of never, then the pod treats the init container as failed if it didn't complete in the start-up
- Add the 'initContainers' field in the pod specs, as an array of container items 
- They do not support lifeCycle, readinessProbe, livenessProbe, or startupProbe as fields. 
- multiple init containers -> they all execute sequentially 
- They are not continuously run alongside the main containers unlike sidecars. 
- They used shared volumes for data exchange with the main containers 
- It offers a mechanism to delay the execution of the main container till a set of prerequisites are fulfilled.
- aws eks describe-cluster \
  --name interns-k8s-training \
  --region us-east-1 \
  --query "cluster.identity.oidc.issuer" \
  --output text

**Sidecar Containers:
- Secondary containers than run along with a main container in a pod. 
- Used to enhance the functionality of the already existing main container - providing additional services such as logging, monitoring, security, etc without directly altering the primary app code. 
- They remainn running after pod startup unlike init containers. 
- Can be started, stopped and restarted without affecting the main container or other init containers. 

**Taint and tolerations 
- Used to set restrictions on what pods can be scheduled on a node 
- Scheduler always tries to place pods on available nodes 
- Suppose if a node has a particular type of resource to fulfill a certain purpose - would want the pods to only take up that node if they were to use those resources. 
- Taint can be placed on the node and by default all pods are intolerant of taints. Therefore no unwanted pods can be placed on this node 
- To enable certain pods to inhabit this tainted node, we need to make those pods tolerant. 
- Taints are set on nodes and tolerations are set on pods 
- k taint nodes node--name key=value:taint-effect
- Taint effect describes what happens to pods that do not tolerate this taint. 1)No schedule - Pod will not be scheduled on the node PreferNoSchedule - pod will try to avoid placing on the node, not guaranteed 2) NoExecute - new pods not scheduled, existing pods will be evicted if they are not tolerant to the taint.
- Toleration added in the spec file of pod. All values need to be encoded in double quotes.
- Taint and toleration does not guarantee that the pod with a tolerance to a particular taint will be placed in the tainted node.
- If the requirement is to restrict a pod to certain nodes - its called Node Affinity. 
- There is already a taint present in the masterNode - to make sure no pods are deployed on it. Command: k describe node kubemaster | grep Taint. It is of type NoSchedule.
  
  
  **Task: Create a small node in your local K8s clusters and taint the nodes.Now, try to spin up a deployment with right tolerations so that pod can able to get scheduled on the above node that you created.
  
  - Create a node in the minikube context using minikube add node 
  - Taint the node by executing k taint node [node-name] key=value:NoSchedule
  - Create a pod, specifying tolerations under spec, with the key-value pair specified when tainting the node
  - Deploy the node, and check to see if it is in the tainted node using [k get pod podname -o wide]

**Node Selectors, Annotations and Labels
- Node selectors - limit pods to run on a particular node, or to limit pods to run on certain type of nodes (eg: large) - labels on the nodes. 
- Label nodes - [k label nodes node-name label-key=label-value]. Used to group and filter nodes on a certain criteria
- If requirement for placing a pod on the node is more complex (eg: place pod on large/med nodes) - to resolve this: node affinity was created. 
- For specifying label for a pod: can add label under the metadata section
- Getting pods acc to label: [k get pods --selector app=App1]
- Annotations - record other details for informatory purposes (contact, buildversion, etc) - also under metadata.
  
  **Task: Explore on default labels and annotations associated with Kubernetes nodes. Use `topology.kubernetes.io/zone` label in the nodeSelectors to schedule a simple nginx pod in "us-east-1c" zone. Once pod is `RUNNING`, check it's node details where it got provisioned.
  
  - Cloud providers automatically attach topology labels to nodes - which is what topology.kubernetes.io/zone is. 
  - Spin up a simple pod.yaml script - with the node selector specifying [ topology.kubernetes.io/zone = us-east-1c]
  - Deploy the script in minikube. Since minikube does not have any access to any cloud providers and just spins up vms in your local machine, the container will forever be in a pending state as there are no nodes with the us-east-1c label.

 **Readiness Probe and Liveness Probe 
 - Liveness probes - determine when to restart a container. Eg: it could catch a deadlock when an app is running but unable to make progress. If a container fails its liveness probe repeatedly - kubectl restarts the container. 
 - Readiness probe - determines when a container is ready to accept traffic. This is useful when waiting for an app to perform any time-consuming initial tasks. If readiness probe fails, kubernetes removes the pod from all matching server endpoints. Runs on the container during its whole lifecycle 
 - Startup probe - verifies if the application within a container has started. Can be used to adopt liveness checks on slow starting containers, avoid them getting killed by the kubelet before they show up in the container. It disables liveness and readiness checks until it succeeds. 

**Storage:
- Docker Storage - storage drivers, volume drivers 
- File system - when docker is installed, it creates a folder structure at var/lib/docker - has container, images, volumes etc. This is where all docker files are stored. 
- Docker's layered architecture - docker builds an image in a layered architecture - base layer -> package managers -> pip packages -> source code -> update entrypoint. The built layers are reused over and over in the cache - builds images faster and saves memory. When a container is run based on the image - a writable layer is created on top of the base read layer. It contains logs and the life of this layer is only there till the container is running. The layer is shared by all the containers built from this image. 
- Container layer - read write, image layer - read only
- 