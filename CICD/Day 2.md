**Why master slave?**
- workload distribution 
- different environments 
- distributed builds 
- improved performance 
- Just like how in kubernetes there is a master/control node for scheduling worker nodes, storing job configs, secrets -> similarly jenkins also has a master node and a slave node 
- The master node is responsible for storing job configs, scheduling worker nodes, Whereas the slave node is the worker node responsible for actually executing the pipelines. 
- Agents (the slave nodes) can connect via SSH where the controller ssh-es into the agent machine and runs a small program there. 

**Multi-branch pipelines:**
- Having a centralized pipeline for all environments. Stages in the multi-branch pipeline can be configured according to the environment. 
- Pull request validation and verification 
- We don't need to maintain a seperate pipeline file for each environment
- Handles multiple branches automatically 
- How it works: ![[Pasted image 20251203110807.png]]

**Multistage pipeline:**
- Multiple stages running in parallel 
- Focuses on workflow structure 
- Use case: can be used for running multiple test cases in parallel - for different levels of testing and all the reports are published to an open source reporting tool (eg:allure report)
- Defines stages within a single pipeline
- ![[Pasted image 20251203110927.png]]

**Shared libraries:**
- Write once, use everywhere. 
- Having a file in a library structure and publishing it. This can then be pulled and reused as configurations. 
- Version controlled 
- Reduces code duplication 
- More standardization 
- Centralized pipeline code management 
- Scanning tools such as checkov (IAC scanning) and sonarqube (analyze the entire code and gives a quality score -> if the score is less then the build can be stopped from deploying.) are used to check for package vulnerabilities (such as in npm) -> notifies in the build stage if package dependencies are compromised and can stop deployment early hand. secret scanning -> scans passwords and other vulnerable information - by gitLeaks. 

**Tasks:**
3. Create a multi-branch pipeline that auto-detects branches from a Git repo. Create a shared library (vars/ folder) and integrate it using `@Library()`. For feature branches, run a simple build; for main branch, run full flow including shared library functions.
   
   **Solution:**
   - 