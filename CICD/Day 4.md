**Env variables and secrets:**
- Storing dynamic values -> endpoint or database urls -> envs used 
- Secure values that aren't printed in execution for authentication/ other security purposes -> secrets 
- Different env variables and secrets can be configured seperately for diff environments (dev/test/prod, for example) whereas repository variables defines secrets and variables for the entire repository 
- Step -> job -> workflow -> repository/environment -> .env (have to manually execute this file) : order of precedence of variables and secrets. 
- Definition: can be added in any of the above mentioned scopes.
  env:
	global_var: global_value
	app_env: test 

**Parallel jobs:**
- Multiple jobs can be made to run in parallel and the jobs can also be dependent on one another. 
- If using a github-hosted runner, then multiple runners have to be used to execute the jobs in parallel. If self-hosted runner, there needs to be many to run jobs in parallel as a runner can only execute one job at a time 

**Controlling Workflow:**
- When the job needs to get triggered
- if github.ref = 'refs/heads/main' (if the branch is main, trigger this job)
- continue-on-error: if a step fails, the step gets skipped and the pipeline continues to run. By default if a step fails it completely halts the workflow. 

**Strategy Matrix:**
- Needs to be defined at the job level
- Can run the same steps/job over and over with different flavours of os/runners etc/ 
- Eg: 
  strategy:
  matrix:
	  os: [ubuntu-latest,windows-latest,macos-latest]
	  python-version:[3.7,3.8,3.9,3.10]
  steps:
	  .........

**Github Output and Environment:**
- github_env -> an environment variable can be used only for one step, when referenced in the next step it doesnt retain the value passed in the previous step. 
- github outputs -> used to store and reference output variables across different steps in an job. 


