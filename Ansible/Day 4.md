**Ansible Roles:**
- When you want usable modular pieces of code to use across playbooks 
- provides a clean structure -> a template gets created with all the structured files 
- other people can use roles easily 
- automation also becomes scaleable 
- ansible-galaxy role init roles/nameofrole -> command 
- ![[Pasted image 20251216110511.png]]
- meta - details about the role created 
- tasks - core section that defines what this role does 
- templates - contain jinja2 syntax templates 
- test - can create a sample inventory and test file to test role against some simple servers 
- vars - will have the highest precedence of variables (overrides default)
- should not define any secrets under the roles -> should be defined in ansible vault and should be referenced in the role as variables. 

- ansible.builtin.import_role -> can do a static import of the role and will only be parsed when the playbook is parsed. Variables defined in the role will be available to all the tasks in the playbook 
- ansible.builtin.include_role -> dynamically includes the role when the playbook is running. Variables defined in the roles is not exposed to the other tasks/sections of the playbook. 
- roles: -role:role1 this is the first section of the playbook that gets executed 
- when defining roles multiple times, ansible looks for duplicate roles. If a role has to be executed twice, then the variables along with the execution of the role have to be different (eg the service)
- if role dependencies are present, need to be defined in the meta/main.yml file

**Ansible Collections:**
- distribution format for the ansible content such as playbooks, plugins, modules, roles, etc. 
- can install and use a collection throughout a distribution server
- ansible-galaxy collection install [collection-name]
- by default the galaxy server is taken
- when creating a custom collection, if this collection has to be used in a different directory, that means the collection has to be installed. Use the tar file for creating the custom collection in the ansible-galaxy install command. 
- requirements.yaml is used to install multiple roles and collections (have to mention the source) in one shot
- ansible-galaxy collection init my_namespace.my_collection -> used to create a collection with the default templates included with ansible -> build can be used to get the tar file which can be used in other playbooks or can be published to the collection artifact in galaxy. 