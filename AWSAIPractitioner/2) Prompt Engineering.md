**Prompt Engineering:**
- A native prompt is submitted to an llm but some prompts sometimes give little guidance and leaves a lot to the model's interpretation 
- PE = developing, designing and optimizing prompts to enhance the output of FMs to the user's needs 
- Enhanced prompting - Instructions (how the model should perform on a task), context (external info to guide the model), input data (input for which you want a response), output indicator (type of output format you are looking for)
- Negative prompting - explicitly instruct the model on what not to include in the response. Avoids unwanted content, maintain focus, enhanced clarity. 

**Prompt Performance Optimization: **
- *How is text generated in an LLM?* - random words have associated probabilities by which words are picked. This is why models are non-deterministic. 
- System prompts - how the model should behave and reply (eg: reply as if you are a teacher)
- Temperature (0-1) - defines the creativity of the model's output. Low: o/p more conservative, repetitive, more likely response. High: o/p more diverse, creative, unpredictable, less coherent. 
- Top P (0-1): Low: considers the 25% most likely words, words will make a more coherent response, low: considers a broad range of possible words - more creative 
- Top K - limits the number of probable words. How many words you want to consider for the model to choose the next word.  Low K (eg:10) more coherent response, less probable words, High K (eg:500) - more probable words, more diverse and creative
- Length - max length of the answer 
- Stop sequences - tokens that signal the model to stop generating output 
- Prompt latency - how fast it is going to respond to the input. Parameters: model size, model type, number of input/output tokens. Not impacted by Top P, Top K, or Temperature 

**Prompt Engineering Techniques:**
- Zero-Shot Prompting: present a task to a model without providing any example or training for that specific task. Rely fully on the model's knowledge. Better and larger the FM, better result 
- Few-Shot Prompting: provide examples of a task to the model to guide its output. Good when you know what type of output you want. One-shot/single-shot if only one example is provided 
- Chain of Thought prompting: divides the tasks into a sequence of reasoning step, leading to more structure and coherence. Helpful when you need to solve a problem as a human -> requires several steps. 
- RAG: combine the model's capability with external data sources for a more informed response. 

**Prompt Templates:**
- simplify and standardize the process of generating prompts. 
- helps with processing user text and outputs from the FM, orchestrates between FM, action groups and KB. Formats and returns responses to the user. You can also provide examples with few-shot prompting to improve the model response. 
- Ignoring the prompt template attack - user could try to enter malicious inputs to hijack our prompt and provide information on a prohibited or harmful topic. 
- Protection against prompt injections - add explicit instructions to ignore any unrelated or potential malicious content. 