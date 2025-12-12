
**Commands and Arguments in Pod 
- Allows you to control container behavior during pod creation 
- command - used to override the default entrypoint (the executable that should run inside the container). These are seperate system binaries that exist within the container
- args - used to override the default parameters passed. This provides additional instructions on how the container should behave after the command has been passed 
- In this way, you don't need to rebuild the image to change how the container behaves. 
- This can be used when 1) you want to modify how the application runs (example enable debug) or 2) Run a completely different program than what specified in the image or 3) when you need shell logic or multiple commands

**ConfigMaps 
- Hardcoding configurations in images - makes them inflexible. If the database url changes, then the whole image has to be rebuilt and redeployed
- Configmaps help in separating the configuration data with the application code. This is useful especially when you need the same image for different envs (dev/prod/stage), version control, centralized config management and to update configs without redeployment. 
- Create a configmap as key-value pairs -> this goes to the apiserver -> stores it in etcd in the master node. now the configmap data will persist in etcd -> the config map is referenced in the pod spec -> the scheduler picks the right worker node to deploy the new pod that has to be created -> kubelet, the agent actually running in the worker node will deploy the pod -> request the config details from the api server -> the api server reads the configs from the configmap and sends it to kubelet -> kubelet passes this to container runtime
- Configmaps can be deployed in two ways - one is as environment variables. If it is deployed as env variables, then the changes won't be automatically refreshed. If it is mounted as a volume, there is an automatic file refresh, this takes upto one minute. For env vars, you need to restart the pod to get the new configs 
- Secrets should not be stored, and the size of the config file should not be too large. Can't be shared across namespaces and most apps won't reload config automatically 
- The configurations as NOT encoded 
- Access control managed by RBAC which is enforced by the api server, both users (user account) and pods (service account)

**Secrets 
- Kubernetes object designed to hold sensitive information such as credentials, token, certificates, etc. 
- Secrets are encoded in base-64 format. Strict RBAC + audit logging
- By default, Secrets are only base64 encoded, which is **easily reversible**. Anyone with cluster access can decode them. True security comes from: Enabling encryption at rest in etcd, RBAC policies limiting who can read Secrets, Audit logging to track access, External secret management (HashiCorp Vault, AWS Secrets Manager)
- Anyone with cluster admin will be able to see all the secrets
- They are visible in container memory 
- Compliance requirements - use external secret manager. for dev/test env - normal k8s secrets are fine. 

**Service Accounts and Tokens 
- Service account provides an identity for processes running in the pods. - user accounts for pods. 
- Every namespace has a default service account and each pod in the namespace also has a service account attached. If not explicitly defined, its default. Service Account uses a token to prove its identity and this token is automatically mounted in the pod in a well-known path  `/var/run/secrets/kubernetes.io/serviceaccount/token'
- Authentication flow:
1. . Pod starts with ServiceAccount specified in spec
2. Kubernetes creates/retrieves authentication token for that ServiceAccount
3. Token is mounted as file in pod: `/var/run/secrets/kubernetes.io/serviceaccount/token`
4. Application reads token and includes it in API requests (Bearer token)
5. API server validates token and checks RBAC permissions
6. Request is allowed or denied based on Role/RoleBinding
   
- Legacy tokens used long-lived tokens previously. Static, base encoded, stored as a secret in etcd, mounted into pods and lasts forever 
- Bound tokens: short-lived, auto-rotated and issued dynamically by the api server. The token is very much tied to the specific context. 

**Resource Management 
- Without proper res management, since multiple pods with containers run in a node, one application can starve others, critical apps might not get enough resources, cluster becomes unreliable and the cost management becomes impossible. 
- Three layer res management: container, namespace, default enforcement 

**Task 1: Spin up a simple pod with commands & arguments and learn about Pod lifecycle, by adjusting restartPolicy.

Key Points: 
- the only value supported for pods created by Deployments, Replica sets and DaemonSets is restartPolicy: Always. Kubelets are responsible for restarting pods. This is because kubelets are no longer responsible over the control of the pod, the controller does. And the deployments expect pods to be long-running services and if a pod fails, deployment will restart the pod immediately in an available worker node. Therefore the restartPolicy is set to always. 
- Types of policy: always, never, onfailure

**Task 2: Create a simple secret and use them in a pod as environment variable & explore how to mount as a volume in a pod.

- configure a secret in secrets.yaml and deploy it 
- create a pod using the secret as an env variable as well as a volume mount. make sure to specify command and args to see the secrets being printed 
- deploy the pod and see the realtime logs for the secrets. 
- env variables - it decodes the secret data and injects the raw data bytes as the env var value at container start. Therefore, updates don't propogate 
- volume mount - each secret key becomes a file, kubelets refreshes the secrets whenever there is a change. these rotate without restart 

**Task 3: Create a configMap with multiple files and data keys 
Mount the files as volume to the pods
Set the data keys from ConfigMaps as pod env variables
Exec into the pods to verify if the volumes are mounted correctly.

- Create some files for the config, and create some direct key-value pairs in the config map. 
- Create a file for the pod - some environment variables, and create a volume to store each config key-value pair as files and mount the volume in a container path 
- Run the script and deploy the pod. Exec into the pod and see if the config files are available. If the config values are changed, they are reflected in the files, not the environment variables

**Task 4: Try to spin up a huge pod with 4vCPUs & 4Gi RAM and see why it's not getting scheduled.

k config use-context minikube --namespace training-space

k config use-context arn:aws:iam::853973692277:role/trainee-aditi-1f6afefd --namespace=trainee-aditi

 k config use-context training-aditi
 k config set-context --current --namespace=trainee-aditi

krew 
kctx

1 : CTF{H3D7L2Q9W6R1N5T8Y4}
2 : CTF{P1Z9M4C7V2K6T3R8J5}
3: CTF{Q8R2T7Y1N5W3L9D6K4}
5: CTF{N4T8R2W6L1Q9Y3D7K5}
4: CTF{M7V3K1P9R6Z2T8Q4N5}

1. Scaled web-app deployment to 3 replicas
2. Deleted orphan ReplicaSet
3. Fixed mislabeled pod
4. Verified logging-prod pod is running
5. Deployed database StatefulSet