**Amazon Q Business:**
- Fully managed Gen-AI assistant for your employees 
- Based completely on company's knowledge and data 
- Can answer questions, provide summaries, generate contents, automate tasks. Can also perform routine tasks
- Can create and move data in the company as well
- Built on amazon bedrock - but can't choose the underlying FM
- Data connectors (fully managed RAG) - connects to 40+ popular enterprise data sources: s3, RDS, Aurora, Microsoft 365, Salesforce. Need to add data sources for the model to refer to for information. 
- Plugins: allows you to interact with 3rd party services : jira, servicenow. Custom plugins are also allowed - connect to any 3rd party apps using APIs. 
- Users can be authenticated through IAM identity center - user receive responses generated only from the documents they have access to. 
- Admin controls - controls used to customize responses based on what your company needs - very similar to guardrails in bedrock. Block specific words or topic and respond only with internal information. Can also set up global controls & topic level controls. 

**Amazon Q Apps - Q Business:**
- Can create Gen-AI powered applications without coding using natural language. 
- Leverages company's internal data and plugins can also be used for this 

**Amazon Q Developer:**
- Answer questions about the AWS documentation and AWS service selection 
- Answer questions about resources in your AWS account. 
- Suggests CLI to run to make changes to your account 
- Helps detect errors, do bill analysis, etc. 
- AI code companion to help code new applications - similar to github copilot 
- Supports many languages: java, javascript, python, typescript, c#
- Real-time code suggestions and security scans, software agent can be used to implement features, generate documentation, create base files for new projects 
- Also works with many IDEs - VSCode

**Amazon Q for QuickSight:**
- QuickSight: create dashboards and visualize data 
- amazon q understands natural language that you use to ask questions about your data to create dashboards automatically 
- ask and answer questions for data, generate graphs and dashboards for the same. 

**for EC2:**
- provides guidance on which instance type to choose for your workload. 
- Can provide more requirements and ask advice on what ec2 instance to take 

**for AWS Chatbot:**
- used to deploy an AWS chatbot in a slack/Microsoft teams channel that knows about your AWS Account 
- Troubleshoots issues, receives notifications for alarms, security findings, billing alert, creates support requests. 

**Glue:**
- ETL service and used to move data across places 
- Chat to answer general questions for glue, generate code, answer questions for certain scripts, helps with troubleshooting with glue scripts.

**PartyRock:**
- gen-ai app building playground - powered by Amazon Bedrock 
- Allows you to experiment creating GenAI apps with various FMs. 
- No AWS Account required. 

