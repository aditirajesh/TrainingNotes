- Responsible AI: making sure ai systems are transparent and trustworthy. Mitigate risks and negative outcomes 
- Security: ensure confidentiality, integrity and availability 
- Governance: add valye and manage risks, clear policies, guidelines and make sure the system aligns with legal requirements 
- Compliance: ensure adherence to regulation and guidelines especially for sensitive domains 

**Reponsible AI:**
**Core dimensions**
-  Fairness: promote inclusivity and prevent discrimination 
- Explainability
- Privacy and security: individuals control when and if their data is used 
- Transparency 
- Veracity and robustness: model is reliable even in unexpected situations 
- Governance
- Safety: algorithms are safe and beneficial for individuals and society
- Controllability: ability to align to human values and intent 

- Bedrock: human or automatic model evaluation 
- Guardrails: filter content, block undesirable content 
- SageMaker clarify: evaluate FM on accuracy, bias detection 
- Data Wrangler: used to fix bias by balancing dataset 
- Model monitor: analysis in production 
- A2I: human review of ML predictions
- Governance: sagemaker role manager, model cards, model dashboards 

**AI Service Cards:**
- form of responsible AI documentation 
- help understand the service and its features 
- find intended use cases and limitations 
- responsible ai design choices 
- deployment and optimization best practices 

**Interpretability:**
- degree of which the human can understand the model's decision. Access into the system so that a human can interpret the model's output. High transparency -> high interpretability -> poor performance (linear regression/decision trees)
- poor interpretability -> high performance (neural networks)
- Partial dependence plots can still be used in case your model is not very interpretable to get some understanding of it. 

**Explainability:**
- understand the nature and behavior of the ml model
- being able to look at the inputs and outputs and exactly understand why the model came to the conclusion. 

**Human centered Design for explainable AI:**
- design for amplified decision making: minimize risk and errors in high-pressure env
- design for unbiased decision making: to make sure the decision process is free from bias 
- Design for human and AI learning: eg with RLHF, need personalization with user-centered design. 

**GenAI Challenges:**
**Capabilities:**
- adaptability
- Responsivness
- Simplicity
- creativity
- efficiency
- personalization
- scalability 

**Challenges:**
 - regulatory violations
 - hallucinations: answer/make claims that sound true but are incorrect. Hallucinates due to the next-word probability sampling employed by LLM. Mitigation: educate the users, ensure verification of content with independent sources, mark wrong answers are unreliable
 - interpretability
 - data security and privacy concerns 
 - social risks
 - toxicity: generating content that is offensive, disturbing or inappropriate. Boundary between restricting toxic content and censorship. Mitigation: curating training data, guardrails
 - nondeterminism
 - plagiarism and cheating: genai can be used to write college essays, samples for job applications and other forms of cheating. 
 
**Prompt misuses:**
 - Poisoning - intentional introduction of malicious or biased data into the training dataset or model which results in the production of biased, offensive and harmful outputs. 
 - Hijacking and prompt injection: influencing the outputs by embedding specific instructions within the prompt itself to hijack the model's behavior. 
 - Exposure: risk of exposing sensitive or confidential info to a model during training or inference, can lead to data leaks or privacy violations. 
 - Prompt leaking: unintentional disclosure or leakage of the prompts or inputs used within a model, can expose protected data. 
 - Jailbreaking: AI models are typically trained with ethical and safety contains -> jailbreaks circumvent the constraints to gain unauthorized access or functionality to a model. 

**Compliance for AI:**

