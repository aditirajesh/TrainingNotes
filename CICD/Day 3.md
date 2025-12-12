**GitHub Actions:**
- CI/CD - inside github 
- Event-driven automation framework: pipeline can automatically run whenever there is a trigger: manual/push trigger etc. 
- **Name -> on -> Jobs -> Steps
- What do you need? : github account and repo, runner (similar to nodes in jenkins - required to run pipelines/jobs) , pipeline yaml file

**Runners:**
- Runner: when a pipeline is getting triggered - tasks written in the yaml file will be executed in the runner. Github actions instruct the runners step by step on how to execute the tasks in an orderly fashion
- Two types of runner: github hosted (managed by github, ready to use, on-demand) and self-hosted (user managed). 
- Github hosted runners: creates the VM for the runner, we don't need to worry about patching, OS updates, networking, etc. Eg: ubuntu-latest, windows-latest, macos-latest. Defined in config file with keyword [runs-on]. Public only and has a fixed size. 
- Self hosted runners: For high performance, used to control runtime environment, helps with cost optimization. [runs-on: self-hosted/ name of your runner created]. Need to manually install and manage. github: [settings -> runners -> add new runner]. Private networks allowed, any size. 

**Workflow:**
- Should create workflow files in .github/workflows at the root level of the repo 
- Name: name of the repo, on: the triggers which will call the event, jobs: different stages/jobs for the pipeline, steps: instructions on how to execute a job. [depends-on] : ensures sequential execution of jobs, and makes sure one job execution begins only when the previous one ends. 
- there needs to be a pipeline file mandatorily in the root level of the main branch of the repository. 

**Types of triggers:**
- Push: triggers when there is a push in the branch 
- Pull_request: triggers when there is a pull request - can configure the type of pull request to fire the trigger 
- Cron: based on cron schedule 
- Workflow_dispatch: manual trigger
- Push -> branches: specifying which branches the trigger applies to. paths: triggers when there is made to a certain file, ignore: paths to ignore 
- pull_request -> types: opened, synchronize, reopened. 

**Task:**
1. Self-hosted runner in aws -> ec2-based, register runner in github repo, ensure runner is online 
2. Create an s3 bucket using github actions (pipeline) -> code changes only done in feature branch -> when there is a merge being made only then the pipeline has to be triggered 

**Solution: Self hosted runner:**
- Create an instance (t3.medium for safety) with ubuntu image, and install git, curl, awscli, and terraform using the user data script. This can be deployed either in a public subnet with a public ip or a private subnet with a nat gateway attached but make sure ssh inbound is possible either from bastion/your local ip. The outbound should be able to reach github 
- Create the github repository for which you want to create a runner. To create a runner, go to settings -> actions -> runner and copy paste the commands for installation of the self-hosted runner in linux. Copy paste these in the runner. 
- Confirm the runner status -> if it is online it has been configured successfully. Create a test pipeline under .github/workloads and under [runs-on] reference the labels of the runner configured for the repository. Push to github and see if the pipeline is successfully executed and if the runner has picked up the job. 

**Solution: Create S3 Bucket:**
- Create a folder uploads with a simple hello.txt file in the feature branch of your repository. Push the same to your remote repository. 
- Create .github/workloads/s3-deploy.yml pipeline under the feature branch containing the code for the creation an s3 bucket and uploading the file present in the uploads folder in the same repository. Make sure to use a push trigger that triggers after a push has been made to main. Upload the same to the feature branch of the remote repository 
- Ensure your runner is up and running and actively listening for jobs 
- Create a pull request with your base as main and compare to feature branch. Merge the two together. 
- Check the runner and actions -> s3-deploy to see if the pipeline got triggered after the merge and check to see if a bucket has been created with the hello.txt file under uploads. 
- Make sure to attach an IAM role to the ec2 instance executing the self-hosted runner with the correct permissions to create an s3 bucket. 



