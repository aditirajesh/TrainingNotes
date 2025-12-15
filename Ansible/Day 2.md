
**Controlling Playbook Execution:**
- Default: Ansible runs all the tasks on all the affected hosts in the playbook before proceeding with the next set of tasks 
- Can override this using a different plugin
- default strategy: linear. Also has debug, and free (host is made to run till the end of the playbook as fast as it can)
- setting number of forks: can be set under [defaults]forks in ansible.cfg
- keywords can also be used to control the execution of a playbook
- serial: helps in setting the batch size. So task execution is done one batch at a time. Can also specify a percentage, list, and can mix and match values. 
  ![[Pasted image 20251212140147.png]]
- throttle: limits the number of workers executing a job -> used to restrict tasks that may be cpu intensive. 
- order: controls the order in which hosts are executed. options: (inventory, reverse_inventory, shuffle, sorted, reverse_sorted)
- run_once: used to run a task on a single host, but the results will be set for all the hosts. To run the task on one specific host, delegate the task using (delegate_to)
- tags can be used to mark relevant tasks which in turn can be used to execute the task without having to trigger other tasks before it. Can configure multiple tags and also skip certain ones. 
- ```
  ansible-playbook playbook.yml --tags [tagname]
  ```

**Task Validation:**
- There are two ways to validate a task in ansible: check and diff.
- --check runs without making any changes in the remote systems. This will show what modules would be changed if the playbook ran, but would not implement any of the changes. 
- --diff provides before and after comparisons -> display detailed information 
- ```
  ansible-playbook foo.yml --check
  ```
  
**Verbosity:**
- --verbose causes ansible to print more debug messages. Adding multiple -v will increase the verbosity, can do upto -vvvvvv

**Using ansible modules and plugins:**
- plugins are used to extend ansible's functionality 
- modules - discrete pieces of code that can be defined in a playbook as a task. Execution is usually carried out by the remote machine and the results are collected. Can also be executed in the command line 
- Example:
  ```
  ansible webservers -m service -a "name=httpd state=started"
ansible webservers -m ping

**Variables:**
- used to manage differences between systems 
- can execute tasks and playbooks on multiple systems with a single command. 
- encloses {{}}
- can define variables with multiple values as a list, or dictionary. 
- set_fact built in command can be used to merge two variable lists to create one 
- variables are defined in a play using vars:
- all.yml - all the environments/groups share this variable
- dev.yml - only applies to the dev group 
- prod.yml - only applies to the prod group 

**Groups:**
- how to organize managed servers in an inventory file, allowing you to target them collectively for tasks 
- groups: inventory (for grouping hosts for execution ), nested groups (create hirearchies), 
- targeting: done by the hosts variable. 
- labels that are attached to one or more hosts -> used to group hosts together for a specific reason 
- for example -> two groups dev and prod can be defined. groups let you run the same playbook except for different hosts and for different variables
- environments can be implemented in ansible using groups
- a single host can belong to multiple groups

**Jinja2:**
- enables dynamic expressions and access to variables and facts 
- you can create a template for a config file and then deploy it in multiple environments, and supply the correct data for said environments

**Roles:**
- self-contained unit of behavior 
- the same setup can be used for another project 
- used for more isolation -> can make changes to only one component and test it separately 
- 