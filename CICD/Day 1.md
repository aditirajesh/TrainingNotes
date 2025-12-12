**What is CICD:**
Continuous Integration, continuous deployment
- Plan: project requirements, features and roadmap 
- Code: write code according to plan 
- Build: compile code 
- Test: test to ensure compliance to standards 
- Release: once app is tested in envs, ready to release the application to the customer 
- Deploy: deploy to production environments 
- Operate: managed release software 
- Monitor: monitor the released application 

to automate the entire process - we use pipelines. 

**Why CICD?**
- Faster development 
- Early bug detection - identify issues before they escalate. Everytime theres a change theres a dev release - to check for any bugs and identify them more easily 
- Simple rollback process
- Clear quality checks - quality checks are set in the pipelines to test the code - only if a threshold is passed then the code gets deployed 
- Less manual work 

**Branching strategies:**
- ![[Pasted image 20251202110944.png]]
- main - points to qa, pre-prod and prod env 
- develop - points to dev environment 
- feature - working on additional features not a part of main -> once ready it is merged to the develop branch in the dev environment with dev testing 
- release - once the test has been done, a release is created and then merged with main 
- hotfix - temporary branch used to fix any bugs 
- branch with qa: ![[Pasted image 20251202111150.png]]

**Jenkins:**
- Tool to implement the CI/CD process 
- Stages: can split jobs in the pipeline into stages and have steps to execute the same 
- Nodes: can schedule a job at a particular slave agent -> make sure resources are available to construct pipelines ranging from small to large-scale jobs 
- Plugins: allows for integration with a lot of other CI/CD tools and install other plugins. 
- Pipeline structure: ![[Pasted image 20251202111502.png]]
- Features: pipelines, devops tool integration, plugin ecosystem, role based access, master/agent support (scaleable architecture for distributed build and testing )
- Pipelines: 2 types of pipeline script - declarative vs scripted. Declarative - pre-defined simple structure, easier to understand, less flexibility and suited for standard workloads. Scripted pipeline - allows more customization, more complex, more flexible, allows complex logic and dynamic behavior such as loops
- Declarative vs scripted: ![[Pasted image 20251202112328.png]]

**Installation of jenkins with docker:**
- ![[Pasted image 20251202111820.png]]
- 50000:50000 - port opening for master-slave communication. A volume is also created 
- logs are used to get the admin login and password - only for the initial setup 

**User management:**
- Create users: allows adding new users to the new system 
- Assign roles: matrix-based assignment - will be able to have a lot of different customizations. This is a plugin that can be installed. role-based assignment. This is done to assign roles to certain users to make sure they are only able to access certain pipelines. 
- Common roles - provide pre-defined rules for easier assignment 

**Secret management **
- secret types - different types of secret can be stored 
- secret scope - levels at which the secrets can be accessed - jenkins level or even a certain domain kevel 

**Job configuration - config.xml **
- backup: creates a backup of the job configurations 
- migration: facilitates moving from one environment/system to another.
- debugging 


Tasks:
**1. Install and Configure Jenkins**  
Set up Jenkins on any cloud platform and install the required plugins. Make sure basic setup, plugins, and user access are ready.

1. Launch an EC2 instance with a user data script for installing java, jenkins and starting the jenkins service respectively:
    sudo yum update -y
	sudo yum install -y java-17-amazon-corretto
	java -version

	sudo curl -fsSL https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key \
	  | sudo tee /etc/pki/rpm-gpg/RPM-GPG-KEY-jenkins > /dev/null
	
	sudo tee /etc/yum.repos.d/jenkins.repo > /dev/null <<'EOF'
[jenkins]
name=Jenkins-stable
baseurl=https://pkg.jenkins.io/redhat-stable
gpgcheck=1
repo_gpgcheck=1
enabled=1
gpgkey=https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
EOF
sudo yum install -y jenkins
sudo systemctl daemon-reload
sudo systemctl enable --now jenkins
sudo systemctl status jenkins --no-pager

	3db40fcca42f4dc2b03f092aae025501

2. Make sure to open port 8080 tcp for the security group of the ec2 instance either from your IP or the bastion host using which you will be accessing the instance. Even 0.0.0.0/0 is okay. 
3. Find the password file and use that as credentials to login when you access http://[ip]:8080
4. Install all the necessary plugins and configure the jenkins path. 

**2. Freestyle Job - List S3 Buckets**  
1. Create a freestyle job that lists AWS S3 buckets.  
2. Make sure to create an iam role to list s3 buckets and attach it to the instance where jenkins is running. This eliminates the need for a credentials manager as aws will automatically provide short-lived credentials using the metadata service for jenkins 
3. In the build step, configure the aws region and write the command to list all buckets in the service
4. Click build now and go through the console output to see if the command has been successfully executed. 
5. If you want to do this with the credentials manager - create 3 secret texts using the credentials manager for the access key, secret key and session token of your aws account. make sure the credentials binding plugin is also installed. In the configuration of the freestyle job -> enable secret text and add the three secrets configured. In the build step configure region, and print the buckets. 

**3. Declarative Pipeline - Terraform Deployment**  
Create a declarative pipeline that deploys an S3 bucket using Terraform. Use Terraform code stored in any Git repo. The pipeline should include these stages:
- Init
- Plan
- Approval (manual input before apply)
- Apply
Add automatic triggers:
- Feature branch commits - Run only till Plan stage
- Main/Master branch commits - Run full flow till Apply stage

1. Create a terraform script with the proper aws provider, the main.tf file containing the bucket creation. In the root, create the jenkinsfile for executing the pipeline script. By using when {} conditionals you can make sure a stage is restricted to only certain branches -> therefore for the approval and apply make sure only branch and main are allowed to run. Push the code to your github repository 
2. Create a github webhook with a secret in your repository. This webhook will automatically call jenkins to run the pipeline whenever a change has been made to the repo. Make sure to provide the url at which jenkins is running: http://url:8080/github-webhooks
3. Attach an IAM role to your ec2 instance with all write permissions. This allows jenkins to have the proper permission to create an s3 bucket. Make sure terraform and git is installed in the ec2 machine where jenkins is running. 
4. Create a pipeline. Enable webhooks, and in the pipeline build select git with scm. Enter your repository url along with any specific branches to build. Specify scriptfile as the jenkins file present in the root of your github repository. 
5. Build the pipeline and check the console output to see if the build was successful
