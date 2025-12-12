
- Fully managed service to build ML models 
- dificult to do all processes in one place + provision servers 
- historical data -> feature engineering -> build model on sagemaker -> train and tune -> test data -> applied to model -> prediction 

**Built-in Algorithms:**
- Supervised alg: linear regressions and classification, KNN
- Unsupervised alg: PCA, K-means, Anomaly Detection
- Textual algs - NLP, summarization 
- Image processing 

**Automatic Model Tuning:**
- Trying different parameter combinations to see which fits best for the model 
- Define an objective metric, and AMT automatically chooses the hyperparameter ranges, early stop condition, search strategy, etc. 
-  Model deployed with one click, automatic scaling, no servers to manage. Reduced overhead 

**Sagemaker model deployment Comparison:**
- Real-time: one prediction at a time
- Serverless: idle period between traffic spikes, can tolerate more latency. Has a cold start problem
- Asynchronous: for large payloads which has long processing times, uses a job queue for processing the payload and stores the request and response in an S3 bucket
- Batch: prediction for an entire dataset, request and response in amazon s3 
![[Pasted image 20251212083558.png]]

**Sagemaker Studio:**
- End to end ml development from a unified interface 
- Team collaboration 
- Tune and debug ml models and deploy them 
- Automate workflows. 

**Data Wrangler:**
- Prepare tabular and image data for ml 
- data preparation, transformation and feature engineering 
- single interface for data selection, cleansing, exploration, visualization 
- has sql support and data quality tools
- can import, preview, visualize, transform, quick model/quick analysis, and export the data flow

**ML Features:**
- given as an input for your ML models used for training and inference
- eg: extract the age from birthdate 
- important to have high quality features for your model 
- Feature store: ingest features from a variety of sources, you can define the transformation of a data into a feature from within the feature store. 

**Sagemaker clarify:**
- evaluate foundation models
- can evaluate human factors such as friendliness or humor 
- leverages an aws-team or employees for evaluation 
- part of sagemaker studio 
- Model explainability: how the model is working and why the model is predicting what it is predicting. It understand the model's behaviour, helps improve trust.
- clarify can also detect human biases in the datasets or models using statistical measures. 

**Ground Truth:**
- RIHF: Reinforcement Learning from human feedback: human feedback as the reward function for reinforcement learning. This is to align model to human preferences. 
- Reviewers: amazon mechanical turk, employees or third party vendors 
- Ground truth plus: also for data labelling 

**Governance:**
- sagemaker model cards: get model info in one place. 
- model dashboard: can see all the models in sagemaker, get info and insights for each. Can track which models are deployed for inference. Can find models that violate thresholds you have set for the data 
- role manager: define permissions and roles for personas 
- model monitor: used to monitor the quality of the data either continuous or on-schedule. Deviations trigger an alert. 
- model registry: way to have a centralised repository to track and manage ml models. Can manage versions, associate metadata, can manage approval status of a model, automate model or share models 
- pipeline: workflow for building, training and deploying an ml model. CI/CD service for machine learning. 
- step types: processing, training, tuning, automl (automatically train the model), model (registering the sagemaker model), clarifycheck (data bias, model bias, model explainability), qualitycheck

**Consoles:**
- Jumpstart: ML hub to find pre-trained foundation models, computer vision models, or nlp models 
- large collection of models 
- models can be fully customized to your usecase
- models are deployed on sagemaker directly 
- ml hub: browse -> experiment -> customize -> deploy 
- ml solutions: access and browse -> select and customize -> deploy 
- canvas: build ml models using a visual interface (no coding required). Access to ready to use models from jumpstart/bedrock 
- build your own custom model using automl powered by sagemaker autopilot 
- Canvas also has ready to use models. 
- MLFlow: open source tool to help ML teams manage the entire ML lifecycle. Can launch MLFlow tracking servers to track runs and experiments and launch SageMaker with a few clicks. 

**Extra Features:**
- Network isolation mode: run sagemaker job containers without any outbound internet access -> ^ security and to make sure there are no data leaks. 
- DeepAR forecasting algorithm: used to forecast time series data and leverages RNN for the same

