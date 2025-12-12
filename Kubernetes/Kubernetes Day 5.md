
**Kubernetes Services:
- Each pod has its own IP address. But they are ephemeral - get destroyed frequently. When they restart, it gets a new IP address. 
- Solution: a service gives a stable ip address even when the pod dies. Also provides load balancing. Good abstraction for loose coupling within and outside the cluster. 

**Types of Services:
- **ClusterIP: **default type. It is an internal service which is mostly used when you want to expose mods/microservice applications to the internet. It obtains traffic from the ingress and reroutes it to the pods containing the microservice applications. 
- Server will know which pods and ports to forward the request to via selectors (for pods. Pods have metadata as identity, and the selectors will match with the labels to identify certain pods) and targetPorts. Therefore all pods that match the selector become endpoints, and will send the request to one of those endpoints (since it also acts similar to a load balancer) to the targetPort specified. 
- When a service is created, an endpoint object is also created which has the same name as the service. This keeps track of which Pods are endpoints of a service.
- Service port is arbitrary, but the targetPort must match the port at which the container is listening at.
  ![[Pasted image 20251117123914.png]]
- **Multi-port services**: Used to handle requests from two different endpoints, and the service needs to have multiple ports exposed and these ports need to be named. 
  ![[Pasted image 20251117124906.png]]
- **Headless Service**: Suppose if a client wants to communicate with one particular pod, or a pod wants to talk directly to another pod/endpoint without routing through the service. Common use cases are stateful apps like databases. In such apps, pod replicas are not identical, and they have their own characteristics and states. 
- So, if the client needs the Ip address of the pod, one way would be to directly communicate with the k8s api server. But this would make the app too tired to the spi service and would be inefficient as you would return multiple ips of all the pods running. 
- The second option is the usage of a DNS lookup. The DNS lookup for a service returns a single cluster IP. But if it is set to none, it returns a pod ip instead. This is what a headless service does. 
- Cluster IP and Headless are in most cases alongside each other
- Type attributes: ClusterIp, NodePort, Loadbalancer
- **NodePort:** creates a service that is accessible on a static port on each worker node in the cluster. ClusterIp is only accessible within the cluster. Therefore external traffic cannot reference the clusterip
- NodePort makes external traffic accessible at a static port on each worker node. Defined in the nodePort attribute (defined range 30000-32767). NodePort services are not secure as the external traffic can directly talk to the worker nodes. 
- A cluster ip service is automatically create along with a node port. 
- **LoadBalancer:** Service becomes accessible externally through cloud providers loadbalancer. NodePort and ClusterIp services are created automatically, lb will route traffic to this. Entry point is the loadbalancer which directs traffic to the nodeport -> worker node -> pod. 
  ![[Pasted image 20251117130118.png]]


**Ingress:
 - Helps user access applications using a single url that you can configure to route traffic to different services and also implement ssl security as well. 
   ![[Pasted image 20251117131249.png]]
 - It still has to be published externally - either as a loadbalancer or as a nodeport 
 - First deploy a supported solution (reverse proxy by nginx, haproxy etc) and second configure rules to specify ingress. The supported solution is ingress controller and the rules are ingress resources. 
 - K8s cluster doesn't have ingress controller built into it. 
 - Ingress controllers are not just loadbalancers. they have additional intelligent logic. Must pass in env variables to pass in the pod name and the namespace it has deployed to. And also specify the ports 
 - Service needs to be created to expose the ingress to the external world (of the nodePort type)
 - Rules are used to route traffic for different domain names, different end path urls, and so on. Usually defined under spec 
 - If a user tries to access a url that does not match any rules, it is redirected to a service called default-backend. This service needs to be deployed. 
 - Hostname has to be specified to handle different hostnames, and paths has to be specified to deal with different paths. 

**HPA: Horizontal Pod Autoscaler**
- Scales the number of pods in a deployment/stateful set based on cpu, memory, or custom metrics 
- Vertical scaling - assigning more resources to existing pods. Horizontal scaling - assigning more pods. 
- Cannot be applied to objects that can't be scaled such as DaemonSet
- implemented by the k8s api resource and a controller. The resource determines the behavior of the controller. 
- K8s implements this as a control loop that runs intermittently - queries resource utilization against the metrics specified once during each defined period and scales up/down based on necessity. 