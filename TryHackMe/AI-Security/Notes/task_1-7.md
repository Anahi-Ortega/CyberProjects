# AI/ML Security Threats
Learn AI basics, key terms, and how it's used by both attackers and defenders.

## ***The building blocks of AI***
**Artificial intelligence** - a machine or computer system that is able to carry out tasks that would otherwise require human reasoning, comprehension, problem-solving, or creativity

**Machine Learning** - a subfield of AI that refers to a computer’s ability to learn from data without being given instructions and is comparable to how the human brain learns


**Machine Learning Structured Lifestyle**
----
Define Problem → Data Collection → Algorithm Selection + Model → Data Cleaning → Feature Engineering → Model Evaluation + Training → Model Development → Model Monitoring

----
**Machine learning algorithms have three components:**  
1. **Decision process** - makes predictions or classifications based on input data
2. **Error function** - evaluates performance and provides feedback
3. **Model optimization process** -  fine-tunes the algorithm to minimise errors and improve accuracy

**ML algorithms four main categories:** 

**Supervised** -  relies on **labeled data** to train models for classification or regression tasks (predicting outcomes based on confirmed past experience)  

**Unsupervised** - works with **unlabeled data** to discover hidden patterns, often using clustering, association, or dimensionality reduction techniques (reviews previous outcomes to try and )  

**Semi-supervised** - combines the two,  using a **small portion of labeled data** to guide the learning process  

**Reinforcement learning** -  mimics human learning by **rewarding correct decisions and penalizing mistakes**, allowing an agent to refine its actions over time to achieve the best outcome  

**Neural networks and Deep learning**
The main objective of AI is to enable computers to behave like humans. In order to achieve this we created neural networks that behave similar to the synapses of our brains. It includes three layes which are the input layer, hidden layer, and output layer. The nodes of a neural network are assigned weights when making decisions and their weights become greater or smaller depending on the accuracy or quality of an answer. Neural networks with more than three layers of nodes are classified as a deep learning (DL) algorithm. 

The **key difference between a DL and an ML** is that a DL does not need the data to be labelled. A DL can take raw unlabelled and/or unstructured data and determine it's key features. Therfore, a DL does not requrie human interactoin (the human interaction being that of labeling the data). A DL is possible thorugh leveraging neural networks. 


---

### Task 2 Q & A
What category of machine learning combines both labelled and unlabelled data?
Correct Answer: semi-supervised learning


What is the first layer in a neural network that handles incoming raw data?
Correct Answer: input layer

Which learning method does not require human-labeled data and can extract features from raw, unstructured input?
Correct Answer: deep learning

What are the weighted connections between nodes in a neural network meant to simulate in the human brain?
Correct Answer: synapses

---


## ***LLMs***
**What are LLMs, and how do they work?**

**Large Language Models (or LLMs)** - are deep learning-based AI models that can process and generate text by predicting the next word in a sequence. 

LLMs are first tarind in a "pre-training" phase where they process vast amounts of text. Instead of relying on labbeled data, LLMs use billions of parameters that enable them to understand and generate human-like language when assessed together. 

Their predictions are then compated to what the correct word was and the paramaters are fine-tuned to make it more likely to predict the correct word and therefore less likely to predict the incorrect word. This is by using a am algorith called backpropagation. 

This process repeates trillions of times over is possible thanks to the hardware advancements in GPUs, Neural Networks, and specifically transformer neural networks. 

**Transformer neural networks** - revolutionized LLMs by enabling parallel text processing instead of sequential word-by-word analysis. This allowed models to assign "attention" to key words and improve contextual understanding. By encoding words into numerical values and calculating attention scores, transformers enhance accuracy, helping models correctly interpret ambiguous references, like distinguishing whether "it" in this sentence refers to "the bank" or "the loan."

**Reinforcement learning from Human Feedback** - this is when predictions are reviewed, and any that would be considered unhelpful by a user or have issues are flagged and the parameters are adjusted accordingly. Once trained and reinforced, the LLM can be used

**How they all correlate:**
**AI** - the overarching field, encompassing all systems that mimic human intelligence
**ML** -  a subfield of AI that enables systems to learn patterns from data without explicit programming
**DL** - a specialised branch of ML, which uses neural networks to process vast amounts of data in complex ways without the need for human interaction, making it effectively scalable ML
**Gen AI**
**LLMs** - advanced DL models built on neural networks, specifically transformers, designed to understand and generate human-like text


---

### Task 3  Q & A
What type of AI model enabled major advancements in ChatGPT and similar tools?
Correct Answer: large language models

What is the first training stage where an LLM processes massive amounts of data?
Correct Answer: pre-training

What type of neural network introduced by Google in 2017 powers modern LLMs?
Correct Answer: transformer

---


## ***AI Security Threats***
**The Implications of AI in Cyber Security**
Vulnerability in AI Models - new threats introduced by the inclusion of AI technology
Existing Attacks that can now be enhanced by leveraging AI 

**[ATLAS MITRE] (https://atlas.mitre.org/matrices/ATLAS))** - the ATLAS framework was built on top of the ATT&CK framework to help guide more specifically to the AI Cyber threats 

**Vulnerabilites in AI Models**  
Prompt Injection  
Data Poinsoning  
Model Theft  
Privacy Leakage  
Model Drift  


**Enhanced Attacks**  
Malware  
Deepfakes  
Phishing  
