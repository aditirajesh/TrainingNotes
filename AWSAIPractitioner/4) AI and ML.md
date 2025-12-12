**What is AI?**
- broad field for development of intelligent systems capable of performing tasks requiring human intelligence. 
- Perception
- Reasoning
- Learning
- Problem solving
- Decision-making
- ![[Pasted image 20251204125107.png]]
- Use cases: computer vision, facial recognition, fraud detection, IDP 
- Components: data layer - collect vast amount of data, Algorithm layer - understand use cases, frameworks, requirements Model layer- implement a model and train it Application - how to serve the model and expose it for users to use 

**Machine Learning:**
- ML : type of AI -> machines learn based on training data 
- Data is used to improve performance 
- Predictions based on the data used to perform a model
- Models - regression and classification. No explicit programming of rules. 
- AI -> explicit programming of rules provided by humans. 

**Deep Learning:**
- Subset of ML: uses neurons and synapses to train a model 
- Process more complex patters of data compared to normal ML 
- Eg: used for computer vision, NLP, Image classification 
- Computationally heavy, large amount of input data, requires GPU 

**Neural Networks:**
- Input data is passed to the input layer and connections are made to inbetween layer finally connecting to the output layer -> done with nodes and each node has a weight based on the input. Each layer will learn about the pattern in the data. 

**Generative AI:**
- Lot of unlabeled data given to a foundation model which then adapts to a broad range of general tasks 
- FM backed by multiple NN
- Transformer Model (LLM): optimization that allows the model to process a sentence as a whole rather than word by word. Gives relative importance to certain words and get trained on a lot of data. EG: Google BERT, OpenAI ChatGPT
- Diffusion model: used for images. With forward and reverse diffusion models 
- Multi-modal models: multiple types of input and outputs based on format.  

**Terms:**
- GPT (Generative Pre-Trained transformer) - generate text or computer code based on input prompts 
- BERT (Bidirectional Encoder Representation of Transformers)- similar to GPT, but reads text in two directions. 
- RNN (Recurrent Neural Networks) - analyze sequential data -> time series and used for speech recognition, time-series prediction. 
- ResNet (Residual networks) -  Deep CNN used for image recognition, object and facial recognition 
- SVM (Support Vector Machines)- ML Algorithm used for classification and regression 
- GAN (Generative Adversial Network) - models for generating synthetic data - images, videos, sounds that resembles the training data. Great for data augmentation. 
- XGBoost (extreme gradient boosting) - implementation of gradient boosting and used for regressions. 
- WaveNet - model to generate raw audio waveform

**Training Data:**
- Good data is required to train the model -> needs to be properly cleaned 
- Labeled data - has both input features as well as output variables. Eg: images of animals as well as the type of animal it is. Supervised learning: can map inputs to the known output
- Unlabeled data - only input features, no output labels. Unsupervised Learning: model tries to find patterns or structures in the data. More cost efficient since labelling huge data sets can be costly 
- Structured Data - data organized in a structured format -> usually rows and columns (tabular), or time series. 
- Unstructured Data - text-heavy or multimedia content, does not contain any structure. 

**Supervised Learning:**
- Learn a mapping function that can predict the output for new unseen input data 
- Requires labelled data: powerful but difficult for millions of data points.  
- Regression: predict numerical value on input data, output variable which is continuous. 
- Classification: predict the categorical label of the input data, output is discrete. Binary classification (spam email), Multi-class classification -> classify animals in a zoo, Multi-label classification -> assign multiple labels to a movie. Key algorithm: KNN
- Training: used to train the model (60-80% of dataset), Validation: used to tune model parameters and validate performance(10-20%). Test: actually test and evaluate the final model performance(10-20%)
- Feature Engineering: using domain knowledge to select and transform raw data into meaningful features. Enhances model performance. Techniques: feature extraction, feature selection (choosing only important features), feature transformation -> great for supervised learning. 
- Can convert text into numerical features, and can get edges and textures using techniques like CNN - for unstructured data

**Unsupervised Learning:**
- made on data that is unlabelled -> discover inherent patterns, structures or relationships within the input data. 
- Clustering, associate rule learning and anomaly detection. Feature engineering can improve the quality of the data. 
- Clustering: group similar data points into a cluster based on their features. Use case: customer segmentation. By: K-means clustering. 
- Association rule learning techniques: associations between different objects can be discovered to group objects together. By: Apriori algorithm. Eg: supermarket can place associated products together to boost sales. 
- Anomaly Detection: identify data that deviates from typical behaviour -> outlier detection. Can be used for fraud detection. 

**Semi-supervised Learning:**
- Small amount of labelled data and a large amount of unlabelled data 
- Train the data on models and then use the model to assign labels to the unlabelled data -> called pseudo-labeling. Model is then retrained on the resulting mix without being explicitly programmed.  
- Pre-text tasks -> have the model  perform and solve simple tasks and then learn patterns in the dataset. Will teach the model to create a representation of the dataset. 

**Reinforcement Learning:**
- ML: agent is going to learn to make decisions by performing actions in an environment to maximize cumulative rewards. 
- Agents: learner/decision maker
- Environment: external system the agent is interacting with 
- Action: choice made by agent. 
- Reward: feedback from the env based on the agent's actions 
- State - current situation of the env
- Policy - strategy agent uses to determine action to do based on the state. 
- learning process - looks at env and state -> looks at policy -> action chosen -> env changes it state and provides reward -> updates policy to improve future decision. 
- Goal: maximize cumulative reward. 
- Applications: gaming, robotics, finance - portfolio management, healthcare, autonomous vehicles - path planning 

**Reinforcement Learning from Human Feedback:**
- Use human feedback to help ML models to self-learn more efficiently 
- There is a reward function -> human feedback directly incorporated with it. Model's response compared with human's, then the human assess the quality of the model's answer.
- Data collection (set of human-generated prompts and responses) -> supervised fine-tuning of a language model (fine tune the existing model with data. The model creates a response and then this is mathematically compared to the human responses) -> new reward model built (humans indicate which prompt they prefer -> model learns to estimate which prompt would be preferred by human) -> optimize the language model with the reward-based model. 
- ![[Pasted image 20251204142407.png]]

**Model fit:**
- Overfitting: model performs well on training data but bad on the test data. The model's parameters is too tuned to the training data and works badly on any unprecedented/new data. 
- Underfitting: model performs poorly on the training data, model is too simple or poor data features 
- Balanced: neither overfitting nor underfitting 
- Bias: difference/err between predicted and actual value. Occurs due to the wrong choice in the ML process. 
- High bias: model predicted is very different from the actual value, and doesn't closely match the training data -> leads to underfitting. Reducing bias can be done by increasing features or using a more complex model 
- Variance: how much the performance of a model changes if a different training dataset is used. 
- High variance: model is very sensitive to the training data, case of overfitting, performs well on training but poor on test -> reduce variance by feature selection (only selecting the most important features) or by splitting into training and test data sets multiple times. 
- Balance: low bias, low variance. 
- ![[Pasted image 20251210111136.png]]

**Model evaluation metrics:**
- **Confusion matrix:** comparing binary positive negative values with the actual positive and negative values 
- Have to maximize true positive and negatives and minimize false positive and negatives 
- Precision = TP/TP+FP
- Recall = TP/TP+FN
- F1 = 2* precision * recall/precision + recall 
- Accuracy = TP+TN/TP+TN+FP+FN
- ![[Pasted image 20251210111424.png]]
- Can also be multi-dimension, it is the best way to evaluate the performance of a model that does classifications 
- Precision - best when FP are costly 
- Recall - best when FN are costly 
- F1 - when you want a balance between precision and recall 
- Accuracy - when you want a balanced dataset 
- **AUC-ROC:** (Area under the curve-receiver operator curve) value from 0 to 1, uses sensitivity (true positive) and "I-specificity"(false positive)
- AUC-ROC shows what the curve for true  positive compared to false positive looks like at various thresholds with multiple confusion matrixes. 
- Regression metrics: sum of the distance between predicted and the actual value
- MAE: Mean absolute err between predicted and actual values 
- MAPE: mean absolute pecentage err 
- RMSE: root mean square error
- R^2 : explains the variance in the model, close to 1 means the predictions are good. 

**Inferencing:**
- model making predictions based on new data 
- real-time: putting a prompt in a chatbot with expecting a respose. Speed > accuracy 
- batch: large amount of data is analyzed all at once, often used for data analytics. accuracy > speed
- Inferencing at the edge: edge devices are devices that have less comp power and is close to where the data is generated, in places where internet connection is limited 
- SLM -> small language models on the edge device, like raspberry pi -> low latency, low compute footprint, offline capability, local inference 
- LLM on a remote server -> edge device will do api calls over the net to get a response -> more powerful model, high latency, must be online or accessible. 

**Phases of ML Project:**
- Identify business problem: define val, budget, success criteria 
- frame as ml problem: convert the business to ml problem, determine if ml is appropriate 
- data collection and preparation: convert data, collect it, data preprocessing and visualization 
- feature engineering: create, transform and extract variables 
- EDA: visualize the data with graphs, and make a correlation matrix to see how linked variables are to one another. 
- model training and parameter tuning: train the hyperparameters
- model evaluation 
- are business goals met?
- if no, data augmentation to enhance the data, or feature augmentation
- if yes, model testing and deploying 
- starts making predictions 
- monitoring and debugging : deploy a system to check the desired level or performance, early detection and mitigation
- if predictions are correct, it is added to the model for retention and the loop continues. Done for continuous improvement and as requirements may change and to make sure the data is relevant over time

Hyperparameter tuning 
- settings that define the model structure and learning algorithm 
- set before training 
- learning rate, batch size, number of epochs, regularization 
- Finding the best hyperparameter values to optimize the model performance. This improves model accuracy, reduces overfitting and increases generalization 
- How? grid search, random search and using services like SageMaker Automatic Model Tuning (AMT)
- Learning rate: how large/small the steps are when updating the model's weight during training. High learning rate can lead to a faster convergence but risk in overshooting the optimal solution (in other words, reaching the local maxima rather than the global maxima), while low learning rate may result in precise but a much slower convergence. 
- Batch size: how many training examples used to update the model's weights in one iteration. Smaller batch -> more stable but more time. Larger -> faster but can lead to less stable updates
- Epochs -> how many times the model will iterate over the entire training dataset. Too few -> underfitting, too many -> overfitting. 
- Regularization: balance between  a simple and complex model -> reduce overfitting, increase regularization. 
- What to do if overfitting? : increase the training data size, early stopping the training of the model, data augmentation to increase diversity in the data, adjust hyperparameters. 

**When is ML not appropriate:**
- For deterministic problems where the sol can be computed, it is better to write computer code that is adapted to the problem. ML solutions may have an approximation of the results. 
