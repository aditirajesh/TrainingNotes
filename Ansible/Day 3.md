**Ansible Vaults:**
- Lets you encrypt sensitive data - db credentials, api keys, passwords, files
- Can store it securely and pass it in playbooks whenever needed 
- ansible-vault used to encrypt
- vault password required to encrypt a variable -> a random string provided to encrypt and decrypt your data 
- different vault passwords can be used for different data. Vault id helps with this. 
- can also define passwords in ansible config file if only one password is going to be used for encryption/decryption 
- can create secrets file using the 'create' command 
- You can use encrypted variables and files in ad hoc commands and playbooks by supplying the passwords you used to encrypt them. You can modify your `ansible.cfg` file to specify the location of a password file or to always prompt for the password.
- ansible-vault encrypt_string --vault-password-file a_password_file 'foobar' --name 'the_secret'
- ansible-vault create --vault-id test@multi_password_file foo.yml

**Loops:**
- Repeat a task multiple times, to avoid writing the same task
- until - simulates while loop 

**Conditionals:**
-  can use 'when'
-  can be implemented using ansible facts - gathers information about your host 
- executes conditional statements differently for dynamic and static reuse. 
- import task -> importing tasks from other playbook files and can define a condition in that particular scenario. This condition is applied to all the imported tasks 
- include task -> when is only applied to the tasks in the main file, not the imported tasks 

**Error Handling:**
- block/rescue/always is used to handle failures gracefully
- can also be used for logical grouping -> grouping related tasks. Example, grouping tasks that have to be executed together using sudo