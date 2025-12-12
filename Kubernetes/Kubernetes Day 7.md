
**Admission Controller:**
- With RBAC different restrictions can be implemented such as restricting access, controls, etc. 
- Most of the rules is in the Kubernetes API level. But what if you want to do more than what a user can do with an object - eg: only allow docker images from certain registry, disallowing version tag on an image, disabling run as root user etc. 
- AC help enforce better security measures - change the request and perform additional ops before creation of a pod. Present in-between authorization and the creation 
- Pre-built AC - **AlwaysPullImages** - everytime a pod is created an image is pulled, **DefaultStorageClass** - created automatically when a PVC is created. It is a template that defines how a storage should be crated , **EventRateLimit** - restricts how many events the api server can respond to prevents flooding , **NamespaceExists** - restricts to a particular Namespace, checks if the namespace is available and is enabled by default. 
  **NamespaceAutoProvision** - this creates a namespace if it does not exist, and is not enabled by default. 
- --enable-admission-plugins= [plugins to activate] - flag to activate 

**Validating and Mutating Admission Controllers:**
- NamespaceExists - helps validates if a namespace exists, reject otherwise. This is a type of Validating Admission Controller. 
- DefaultStorageClass - checks the PVC to see if a storage name is specified. If not, it modifies the request to specify a default storage class. This is a type of Mutating AC. This can mutate the object before it has been created. 
- MAC -> VAC. This can help in validating the mutation done by the MAC
- Mutating Admission Webhook and Validating Admission Webhook - used to provision our own MAC and VAC

**Kustomize:**
- When different environments are required to test a certain deployment (dev/stg/prod), some configs might change. One solution would be to have three separate configs and deployment scripts for each environment, but this makes most of the code repetitive. If this expands with creating more services - this would have to be copied to the other environments as well which would become tedious especially with modifications. 
-  Solution - kustomize. It has base config and overlays. Base config - config that is going to be identical across all environments as well as default values. Overlays - allow to customize behaviour on a per-environment basis. 
- Folder structure - also follows base and overlays. Within overlays for each env there is a different folder. 
- Kustomize comes built-in with kubectl so no other packages need to be installed. 
- a kustomization.yaml file needs to be defines - needs to have a list of all the resources that need to be managed by kustomize, the second should be the all the changes that have to be made across envs

**Helm:**
- It is a package manager for kubernetes 
- For an application/website multiple moving parts are required - deployments, pv, pvc, secrets, services, etc and all these would have to be developed as separate yaml files and deployed accordingly, which is tedious. 
- Writing all obj declarations in a single file makes it harder to find objects especially when troubleshooting. Helm helps to resolve this
- Built from ground-up: looks at the objects as part of the big package. A single command can be used to deploy an application that contains a hundred objects. We can specify values we desire in the values.yaml file - for custom settings. 
- Can upgrade the app with a single command - helm knows what individual objs need to change to make this happen 
- Can also use a single command to uninstall the app and helm automatically keeps track of all related objects and removes them. 
- install, uninstall, upgrade, rollback 
- Lets treat kubernetes apps as apps rather than related objects 
- All charts are stored at the online repository ArtifactHub.io 