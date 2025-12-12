**Azure DevOps:**
- Cloud-based devops platform by microsoft 
- End-to-end dev lifecycle
- Why? : secure, reliable, good for enterprise workloads, artifact handling, combines Github and other tools in a single ecosystem. RBAC makes it suitable for large organizations. 
- Jenkins: manual setup, plugins. GHA: SaaS, YAML. Azure DevOps: CI/CD, Boards, Repos, Agile -> preferred for end-to-end deliverable pipelines. 

**Agents:**
- Self-hosted: one machine, can control everything. When you need custom roles, need access to private networks/ on-prem systems. In jenkins, everything is self-hosted. 
- GHA: Workloads designed around Github repository, offers self-hosted and hosted runners. 
- ADO: Enterprise governance + integrated DevOps + end-to-end delivery

**Services:**
- Boards - Agile planning 
- Repos - Git-based version control with branch policies, code reviews and pull requests. Help maintain high code quality and can be used in conjunction with pipelines to trigger automatic testing. 
- Pipelines - CI/CD. Build, test, deploy automation -> multi-stage workflows -> supports YAML script. Ensures consistency and reduce deployment risk
- Artifacts - Package management. Secure and centralized ways to manage packages - npm, nuget, Maven. Rather than downloading dependencies from public internet sources, can download from private feeds -> increase security and compliance -> supports version control. 
- Test Plans - Testing workflows
- Integrations - azure key vaults, github, terraform, teams/slack notifications. 

  
**Task 1: Terraform Deployment Using Azure DevOps**

- Create Terraform configuration that deploys
    - Resource Groups
    - App Service
    - App Service Plan
- Keep all values inside **pipeline variables**
- Set up a **CI pipeline** that:
    - Installs Terraform
    - Authenticates to Azure **inside the pipeline**
    - Runs Terraform init, fmt, and validate
    - **Make validation behave differently based on environment:**
        - If environment is **prod** → run strict validation and fail on warnings
        - If environment is **dev** → allow warnings and continue
    - Generates a Terraform plan
    - Publishes the plan as a pipeline artifact
- Set up a **CD pipeline** that:
    - Authenticates to Azure again inside the pipeline
    - Downloads the plan artifact
    - Pauses for **manual approval** before applying changes
    - Runs Terraform apply to deploy the resources
- Verify the deployment in the selected environment

**Solution:**
1. First, create an azure VM with a public ip. This is the vm that is going to execute the self hosted runner. 
2. Create a managed identity for the VM. This managed identity should have contributor access at the subscription level. This is to override service connection in azure devops so that the pipeline will be able to login to azure cli and actually create the required resources. 
3. Attach this managed identity as a user-defined identity to the VM. 
4. In your organization where the pipelines will be executing, go to settings -> agent pools and create a new agent pool. Create a new agent under the new pool, and copy paste the commands present under it in your linux VM. This will start the self-hosted runner. 
5. Go to marketplace and download terraform, the same terraform in the marketplace will be referenced in the ci pipeline script for planning 
6. Design your repository structure. infra/ should contain main.tf with all the resources that need to be provisioned defined, variables.tf should contain all variables needed, output.tf have the outputs, and providers.tf should contain the provider and hashicorp version. 
7. Make sure to reference the agent pool created earlier with the self-hosted agent as the pooImage for both the pipelines. This is where the pipeline will be executed. 
8. The ci pipeline contains the following steps in one job: first install azure ci and terraform -> login to azure cli (this can be done without referencing a service connection since the managed identity is attached to the vm directly) -> install tflint to implement dev and prod warnings -> terraform init -> plan -> publish the plan as an artifact 
9. Make sure to create two env variables -> dev and prod with a variable groups for each, defining the values for variables specified in the variables.tf file. Make sure the var starts with TF_VAR_[variable name] -> only this will be considered by tf as a var from azure and will inject the value. Make sure to define the variables under env specifically as well. 
10. The cd pipeline must be triggered immediately after the ci pipeline, and the triggering ci pipeline must be referenced in cd as the pipeline from which the artifact has to be downloaded 
11. in cd pipline: install zip, tf and azure -> authenticate via azure cli -> download the artifact produced by ci -> apply the download plan using terraform apply. 
12. After executing ci and cd pipeline check to see if the resources have been successfully deployed. 


**Task 2:** **Deploy an Application to Azure App Service Using Marketplace Tasks**

- Push a sample application to **Azure Repos**
- Create a **CI pipeline** that:
    - Restores dependencies
    - Builds the application
    - Packages it and publishes the artifact
- Create a **CD pipeline** that:
    - Authenticates to Azure **inside the pipeline**
    - Uses variables for configuration, environment, and deployment parameters
    - Downloads the artifact from CI
    - Deploys the application to **Azure App Service** using the _App Service Deploy Marketplace task_
    - Includes a **pre-deployment approval** before deploying to higher environments
    - After deployment, verify the application is accessible via the App Service URL

**Solution:**
1. Build a very simple node.js application, with a simple server.js containing hello world, and package.json holding the dependencies. Make sure package-lock.json is also present in the src folder, and while pushing it to repo do not include node modules. 
2. The ci pipeline has the following steps: installation of node -> restore dependencies ->build the application using npm (this is why package-lock.json is required) -> install zip -> convert the app into a zip package -> publish as an artifact 
3. The cd pipeline should be triggered by the ci pipeline, and the artifact published in the ci pipeline is the one that should be downloaded and deployed by cd. 
4. cd pipeline: download pipeline artifact -> install cli -> login -> webapp deploy -> curl url to see if reachable
5. The application stack must be mentioned, either in the tf script where the web app is created or later after the deployment of the application in the app service: under configurations. Restart the web app and access the web app url directly to see if the content specified under server.js is showing up. 
6. Have two different env variables: dev and prod -> with variable groups respectively containing the app service name, and the region where it is present. These do not have to be prefixed with TF_VAR as it is directly being referenced by azure cli. 