**EKS:**
- When using EKS for managing Kubernetes, it manages and takes care of the control plane for you. Management of the data plane (where the worker nodes exist) is under you. 
- Takes care of patching and upgrading the control plane, manages the etcd backup, ensures availability of master nodes and manages control panel security 
- Control Plane (AWS Managed) - kubeapi server, etcd database, controller manager, scheduler 
- Data Plane (managed by user) - worker nodes containing - kubectl (node agent), kube-proxy (network rules), containerd (container runtime)
- Controller manager continuously watches the cluster state to make the actual state match the desired state. 
- Kubernetes - declarative model. You tell it what you want, not how to do it. 

**KEDA: Event Driven Autoscaling**
- Limitation of HPA is that it does not understand business metrics - such as the depth of an SQS queue, HTTP requests and therefore doesn't know how to scale accordingly to know the same. A queue might contain 1000 messages but the cpu usage might only be 30% in that case the hpa does not scale even when it is necessary 
- Moreover, hpa needs to always have atleast one pod running even when the resource utilization is at 0. 
- KEDA scales based on events not resource metrics - scales based on actual workload 
- kind: ScaledObject 
- This is perfect for background job processes, event handlers, batch workers - anything that processes messages or events. 

**Karpenter:**
- Standard way to autoscale worker nodes - Kubernetes Cluster Autoscaler
- Limitations:
  - Slow: ASG takes 3-5 minutes to update the number of worker nodes
  - Inflexible: Need to pre-define the instance type of the instances launched by ASG 
  - Wasteful: Sometimes provisions bigger instances than required 
  - Complex: Need to configure multiple ASGs for different workloads
- Karpenter bypasses ASG completely and talks to the AWS EC2 api server directly to provision instances. It reads the pod's resource requirements and automatically picks the best instance type of the AWS instances. 
- It launches directly via the EC2 API (30-60 seconds) and when the node becomes empty, it terminates automatically 

**eksctl:**
- eks in one command instead of 40 manual steps. 
- It is an official tool from weaveworks that automates all steps - from networking, iam security, cluster setup and kubeconfig. 

**OpenShift:**
- enterprise kubernetes platform with additions, security hardening and dev tools built-in. 
- Can automatically build and deploy apps from the source code
- Has tightened security rules - such as cannot run container as root. 
- Kubernetes comes pre-configured - with monitoring, logging, registry and ci/cd. 