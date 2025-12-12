**Why?**
- Are pre-trained ML services geared for your use cases
- Can process text and documents, vision, search, chatbots, speech, recommendation, machine learning as a whole can be done with amazon sagemaker 
- Have responsiveness and availability, can be deployed across multiple azs. Good performance and token-based paying. Can have provisioned throughput for predictable workloads. 

**Amazon Comprehend:**
- Amazon Comprehend: for NLP, fully managed and serverless service. Used to find insights and relationships in a text. 
- Language of a text 
- Understand sentiment of a text - positive or negative 
- Extract key phrases 
- Analyse text using tokenization 
- Organize text by topic. 
- Use case: analyse customer interactions, create and group articles by topic 
- Custom classification: how we want comprehend to organize the documents into categories. 
- training data -> s3 -> comprehend uses the custom classifier -> test -> classifies it according to cc
- NER(Name entity recognition): extract entities like people, places, organizations, dates from text. 
- Custom entity recognition - analyse the text for specific phrases/texts. Train the model with the custom data such as the list of entities to extract. 
- Targeted sentiment - how the sentence was built, underline the important entities
- Need to purchase an endpoint for the model after the custom classification in order to do real-time analysis of the document

**Amazon Translate:**
- Natural and accurate language translation 
- Allows localization of content 
- Metrics: can view how many transaltions have been done successfully 
- Custom terminology: customize the output of amazon translate for certain words. 
- Parallel data: customizing the style of the translation (eg: formal and informal translations)

**Amazon Transcribe:**
- Convert speech to text
- Uses Automatic speech recognition 
- automatically remove personally identifiable information using redaction 
- supports automatic language identification for multi-lingual audio
- use cases: transcribe customer calls, subtitle generation, etc 
- For improved accuracy: custom vocabs and language models. 
- Custom vocabularies: add specific words, phrases and domain specific terms, good for brand names, acronyms, and increases the recognition of a new word 
- Custom language models: train the model with your own domain-specific text data, and good for transcribing large volumes of domain specific speech. We can learn the context associated with a word. 
- Toxicity detection - uses tone and pitch, and text-based cues. 

**Amazon Polly:**
- turns text into lifelike speech using deep learning 
- Lexicons: define how to read certain specific pieces of text (eg aws -> amazon web services)
- SSML - speech synthesis markup language. markup for your text to indicate how to pronounce it 
- Voice engine: generative, long-form, neural, standard 
- Speech mark: encode where a sentence/word stars or ends in the audio.

**Amazon Rekognition:**
- Find objects, people, text, scenes in images and videos 
- counting people in photo, create db of familar faces
- used for labelling, content moderation, face detection, text detection, face liveness, celebrity recognition, image properties.
- Custom labels - eg: find your own logo in social media products, identify your products on store shelves. Done by labelling your training data and uploading them. Then it creates a custom model for your image sets, and subsequent images will be categorized the custom way you have defined. 
- detects inappropriate, unwanted, offensive content. 
- Custom moderation adaptors: extend cap by providing your own labelled set of images.

**Amazon Lex:**
- build chatbots quickly using voice and text
- supports multiple languages 
- integration with aws lambda, connect, comprehend 
- automatically understands the user's intent to invoke the correct lambda function to fulfill the intent. Bots might as for slots (input params) if necessary  
- Intents: what does the user want to perform?
- utterances: how you want the conversation flow to look like from the bot's size - customizing the bot's response. 
- fulfillment - the lambda function to trigger once an intent has been established. for example if the user wants to book tickets and the intent has been confirmed by the bot, a lambda function can then be executed to carry out that task

**Amazon Personalize:**
- Fully managed ml service to build apps with real-time personalized recommendations 
- used by amazon.com 
- read input data from s3 -> amazon personalize api used for real time data integration -> sent to amazon personalize -> recommendation sent as sms, emails, to websites/mobile apps 
- Can be implemented in days to existing apps 
- use case: retail stores, media, entertainment 
- Recipes: algorithm prepared for specific use cases, must provide training configuration on top of the recipe. 

**Amazon Textract:**
- automatically extracts text, handwriting and data from any scanned documents using ai and ml
- extra data from forms and tables 
- use cases: financial services, healthcare, public sector
- can also understand the layout of the scanned pages and organize accordingly. Also organizes in the form of key-value pairs from the document, as well as tables.  

**Amazon Kendra:**
- document search service 
- extract answers from within the document 
- data sources indexed by amazon kendra and builds a knowledge index to answers prompts accordingly. 
- Learns from user interactions/feedback 
- ability to manually fine-tune search results 

**Amazon Mechanical Turk:**
- crowdsourcing marketplace to perform human tasks
- distributed virtual workforce 
- eg if you have a large dataset that has to be labelled, you distribute the task to mechanical turk and humans from all around the world will tag it. You set a reward per image (1rs per image)
- use cases: image classification, data collection 
- integrated with amazon a2i, sagemaker groundtruth. 

**Amazon Augmented AI:**
- human oversight of ml predictions in production
- input data -> aws ai service/custom ml model for prediction -> amazon augmented ai -> high confidence predictions are sent to the client app whereas low confidence predictions are sent to humans for review -> reviews are then consolidated using weighted scores -> client gets the prediction. This is then fed back into model for learning 
- Can be employees, contractors from AWS
- The ml model can be built on AWS or elsewhere 
- team - mechanical turk, private or vendor 

**Amazon Transcribe Medical:**
- automatically convert medical-related speech to text 
- specializes in medical terminologies 
- support real-time and batch 
- voice apps to dictate medical notes 

**Amazon Comprehend Medical:**
- detect and return useful info from unstructured clinical text. 
- uses NLP to detect protected health information 
- real-time data 
- can be combined with amazon transcribe for the entire flow. speech -> text -> comprehension

**Amazon's Hardware for AI:**
- GPU based EC2 instances (p3,p4...g3,g6) - desirable for ml/ai purposes. 
- AWS Trainium - ML chips to perform deep learning on big parameters. Trn 1 has 16 trainium accelerators.
- 50% cost reduction when training a model 
- AWS Inferentia: deliver inference at high performance and low cost
- Trn&  Inf have a low environment footprint.  


