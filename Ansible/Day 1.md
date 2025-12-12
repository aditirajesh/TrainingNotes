**What is ansible:**
- Tools to automate IT tasks 
- IT task execution manually is tedious -> especially when there are many and are complex. Repetitive tasks: updates, back ups, system rebooks, create users, assign groups 
- With ansible: can execute tasks for all the servers into your own machine, can configure steps in a single yaml file, the same file can be reused for different environments, more reliable and less likely for errors. 
- Ansible also supports all infrastructure -> from os to cloud provider. 
- It is agentless -> need to give ssh access to the target servers, that is all that is required. No deployment or upgrade effort. 

**How does it work?**
- Modules: small programs that execute work 
- Modules get sent from the control machine to the target server -> do their work and get removed. 
- Modules are very granular. It does only one small specific task
- When complex applications and configs need to be applied, multiple modules must be grouped together in a certain sequence. This is done by ansible playbooks. 
- Hosts: tell where the tasks should execute and the remote_user tells with which user should the task execute. Recurring values can be defined as variables. 
- Multiple plays in a single yaml file. Playbooks orchestrate the modules execution
- ansible inventory list: contains a host file which contains list of all the machines involved in the task executions. IP addesses/hostnames can be provided.  
- Playbook gives an alternative to docker containers -> can define the config file, env variables, log dir -> can directly create container and can even deploy this on a cloud instance/dir. 
- Ansible Tower: UI dashboard to centrally store all automation tasks across teams, configure permissions and manage inventory 
- Alternative: Puppet and Chef. They use Ruby, installation required for puppet/chef, need for managing updates on the target systems. 

Installation Requirements:
- Control node: should be run on a UNIX-like machine with python installed 
- Managed node: requires python to run the ansible code. Also needs a user account that can connect via ssh to the node 
- 