**Intro to AI:**
- Solving problems associated with human intelligence. 
- How does it work: takes a training dataset to train a model (a computer code having certain statistical tendencies)
- 1950s birth of ai: turing test -> john mccarthy joins AI -> 1970s expert system MYCIN (ai rule-based system to detect bacteria) -> 1990s machine learning and data mining -> 1997 deep blue defeats world champion -> 2010 deep learning revolution (neural networks) 2016 alphago defeats go champion lee sedol -> 2020a ai in everyday life virtual
- Use cases: transcribe and translate spoken language, playing humans in games, driving cars, speech recognition, suggesting code for developers, medical diagnosis, automating business processes, fraud detection

**Gen AI:**
- Subset of deep learning -> used to generate new data similar to data it was trained on 
- Trained on text, audio, video, image etc.
- can combine its knowledge into new and unique ways 
- unlabeled data is used to train a foundation model which is very adaptable to a broad range of general tasks. 
- Foundation model - trained on a variety of inputs and it may cost millions of dollars and takes a lot of data and time to train it. 
- Eg: GPT-4o is a foundation model behind ChatGPT
- some models are open source and others are under a commercial license.
- Diffusion models - adds noise to an image (called the forward diffusion process) - this is done for a lot of pictures and once the algorithm is trained to created noise from the image - it does a reverse diffusion process to generate an image requested from the noise.

**LLM:**
- LLM: Type of AI designed to generate coherent human-like text. Eg: GPT-4. Trained on a large amount of text data, billions of parameters and trained on books, articles, websites and other text data
- LLM is given a prompt then the model will leverage the existing content it has learnt from to generate the new content. The output is non-deterministic - the generated text may be different for every user that uses the same prompt. 
- Why is it non-deterministic? : the llm generates a list of potential words that fits to the answer of the prompt alongside probabilities. And the algorithm based on the random possibilities selects a word from the list. 

**Amazon Bedrock:**
- used to build gen-ai applications 
- fully-managed service, no servers for you to managed. 
- Keep control of your data used to train the model 
- pay-per-use model 
- unified APIs
- leverage a wide range of foundation models 
- out of box features - RAG (Retrieval Augmented Generation), LLM Agents 
- Foundation models in bedrock: AI21labs, meta, anthropic, amazon, stability.ai, cohere 
- Bedrock makes a copy of the FM available only for you which you can fine-tune with your own data and none of your data is used to train the FM. 
- Interactive playgrounds help us select the FM we want to use and we can ask the users questions.
- Knowledge bases can also be added for more relevant and accurate responses by fetching data from external data sources 
- Can also fine-tune model and update it with your data, can use an s3 bucket in conjunction with this. 

**Bedrock: base foundation model**
- How to choose: types, performance requirements, capabilities, constraints, compliance, level of customization, model size, multimodal models 
- Amazon Titan: high-performing FM from AWS -> image, text, multimodal model choices via a fully-managed APIs. Can customize with your own data -> 8k tokens accepted for input. High performance text model, +100 languages used for content creation, classification, education, etc.
- Can compare models on features using the compare model mode in the playground. 
- Can also add customization to models : fine-tuning (configuring the type of output you want from the model), continued pre-training (giving extra data to train the model)
- Need to purchase provisioned throughput to use a fine-tuned model. Since you are creating your customized version of the model - the models needs some dedicated, guaranteed compute to run the version reliably and they use provisioned throughput units to do the same. 

**Bedrock: Fine-tuning a model**
- Adapt a copy of FM and add your own data 
- changes the weights of the base FM 
- Training data must adhere to a specific format and also be stored in s3. 
- Must provisioned throughput or on-demand options for using the model 
- not all models can be fine-tuned. 
- Instruction based fine tuning - improve performance of pre-trained FM on domain-specific tasks. Used to further train on a particular field of knowledge. This uses labeled examples that are prompt-response pairs. 
- Continued Pre-training : provide unlabeled data to continue the training of an FM - also called domain-adaptation fine-turning to make a model an expert in a specific domain. Good to feed industry-specific terminology into a model 
- Single-Turn Messaging: give a hint to user and assistant -> what the user is asking and what the assistant should reply. Part of instruction based fine tuning and is used to tell the model how it should reply to prompts. 
- Multi-Turn Messaging: conversation-based fine-tuning which is also a part of instruction-based. Chatbots=multi-turn environments. 
- Re-training an FM requires a higher budget. Therefore instruction-based is usually cheaper since computations are less intense. 
- Must prepare model, do fine tuning and evaluate 
- Use cases: chatbot designed with a persona/tone or for a particular purpose -> trained with more up-to date info/exclusive data

**Transfer learning:
- using a pre-trained model to adapt it to a new task. 
- widely used for image classification and for NLP 

**Bedrock: evaluating a model
- *Automatic evaluation:* evaluate the model for quality control. Give some built-in task, along with your prompt dataset and then scores are calculated automatically. 
- Some benchmark questions with some benchmark answers are set, and these questions are given to the model to evaluate. The generated answers are then compared with the benchmark answers using a judge model to calculate a grading score on how well a model is performing. 
- Benchmark dataset: curated set of data for evaluating the performance of a model. Has a wide range of data, complexities, linguistic phenomena -> measure accuracy, speed, efficiency and scalability. 
- *Human evaluation:* a work team is chosen to evaluate the model based on the benchmark answers and generated answers . Substitutes the role of a judge model. 
- Metrics to evaluate an FM: ROUGE -> evaluate automatic summarization and machine translation systems. 
- ROUGE-N: measures the number of matching n-grams between reference and generated text.
- ROUGE-L: longest common subsequence between reference and generated text 
- BLEU: quality of generated text especially for translations. Considers prescision and looks at combination of n-grams. 
- BERTScore: semantic similarity between generated text. Capable of capturing more nuance between the texts. 
- Perplexity: how well the model predicts the next token. 
- Business metrics to evaluate a model: user satisfaction, average revenue per user, cross-domain performance, conversion rate, efficiency

**RAG and Knowledge Base:**
- Retrieval augmented generation - allows FM to reference a data source outside of its training data without the model being fine-tuned by it.
- the FM is linked to some data with an S3 bucket as a data source - which forms the knowledge base. When the user asks a prompt to the model, it searches for relevant info in the knowledge base which is backed by a vector database. This helps the model  retrieve relevant info from the knowledge database and goes as an augmented prompt. Text + retrieved text goes as an augmented prompt into the Foundation model and then it generates a response. 
- Therefore this is used to pass in real-time user data to the FM
- RAG Vector databases - used to store indexes - can be stored in s3 vectors, aurora, mongodb, redis, pinecone. 
- S3 data is converted to document chunks and these chunks are passed to an embeddings model done by amazon titan/cohere and these embeddings are stored in the vector database. 
- OpenSearch Service: for serverless and managed cluster -> good and scalable index management. Store, index and retrieve embeddings, perform similarity search. 
- Graph analytics : amazon neptune analytics 
- S3 Vectors: cost-effective and durable storage
- Aurora - relational database 
- RAG data sources: s3, confluence, salesforce, web pages, sharepoint. 
- Use cases: can build a customer service chatbot, legal research analysis 
- Need to create an IAM user and give access to aws access console for creating your own KB.
- ![[Pasted image 20251201131617.png]]

**GenAI Concepts**
- Tokenization: converting raw text into a sequence of tokens. 
-  Context window: number of tokens an LLM can consider when generating text 
- Larger the context window: more info you can feed and coherence. Needs more memory and processing power. 
- Embeddings: create vectors out of text, images or audio. text -> tokenization -> every token converted to a token id -> embeddings model -> a vector is converted for each token -> stored in vector database. 
- Vectors have a high dimensionality and can capture many features for one input token, syntactic role, sentiment and semantic meaning. Embeddings model can power search applications 
- Words that have a semantic relationship have similar embeddings. 

**Guardrails:**
- Control the interaction between users and FM. 
- Filter undesirable and harmful content. 
- Remove personally identifiable information 
- Enhance privacy 
- Reduce hallucinations - make sure that answers are safe and sound and not just invented. 
- Ability to create multiple guardrails and monitor and analyze user inputs that can violate the guardrails. 
- can have different filters - content filters like filtering harmful categories, denied topics, profanity filter with custom words and phrases to be blocked.

**Agents:**
- Manage and carry out various multi-step tasks related to infrastructure, application deployment and operational activities. 
- Configured to perform specific pre-defined action groups. 
- Can interact with other systems, services, db and API to exchange data or initiate action 
- Can leverage RAG to retrieve info whenever necessary. 
- Instruction for an agent is given -> agent knows about a few action groups -> agent can invoke these and then interact with a db in the backend -> also has access to knowledge bases 
- Action groups can either be api defined but can also be lambda functions. 
- task -> given to the bedrock agent -> looks through prompts, convo history, actions, instructions and then passes it to the bedrock model -> chain of thought -> gives a step by step process to get the answers -> final result given back to bedrock agent -> task + result sent to another bedrock model and then the final response is finally forwarded to the user. 

**Bedrock + Cloudwatch:**
- Cloudwatch - can do model invocation logging - send all invocations of a model into amazon s3/cloudwatch logs. Can get a history of everything happening on bedrock. 
- Can analyze inputs further and build alerting 
- Metrics can be published from bedrock -> cloudwatch. ContentFilteredCount - help to see if the guardrails are functioning as expected. 
- Alarms can be built on top of the metrics. 
- Settings in bedrock -> model invocation logging

**Pricing:**
- On demand: pay-as-you-go, text models: charged for every text input/output processed. Embeddings model: charged for every input token processed. Image models: charged for every image generated
- Batch: Multiple predictions at a time and can provide discounts upto 50%. Output is a single file in s3
- Provisioned throughput: purchase model units for a certain time -> max number of input/output tokens processed per minute. 
- Model improvement techniques - prompt engineering: cheap because no training, RAG: external knowledge. More expensive than prompt due to external data, but no model training required. Instruction-based fine-tuning: FM fine-tuned with specific instructions. Expensive. Domain adaptation fine-tuning: model trained on a domain specific dataset. Very expensive. 
- On-demand: great for unpredictable workload, batch-50% discounts, provisioned throughput - not a cost-saving measure, just for reserving capacity, temperature, top k, top p - no impact on pricing. Model size - usually smaller model is cheaper. Number of input/output tokens - main driver of cost. 