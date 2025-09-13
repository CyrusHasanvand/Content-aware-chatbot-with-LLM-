
# Context-Aware LLM Chatbot — ChatChain

This is a lightweight built on Large Language Models (LLMs) that preserves conversational context.
It uses a dynamic prompt-chaining mechanism to memorize and feed prior chats into each new request, enabling memory-preserving interactions.

# Type of Usage
We can use this when designing an AI-based chatbot to answer users for **online shops**. When a user asks something, the model remembers its previous demands; therefore, the model can provide more accurate suggestions to the user.

# How It Works
First, we have user input as ```Human Message```. After that, LLM introduces suggestions to user as ```AI Message```.
So, in the next step, we have to save the both ```[Human Message and AI Message]``` to use them in advance.
Accordingly, we can use this information as a propmt for our AI model.

# LLM model
In this project, I have worked on my own local LLM model ```Ollama- llama 3.1```. I have addressed two HuggingFace models, named ```phi2``` and ```mistral``` which you can use them.
Furthermore, other model such as OpenAI-GPT can also call from ```API keys```.

# Explanation
I have define the following prompt to the model to set its role, where I impose a limit on its output to a maximum of 220 tokens:
```python
prompt=ChatPromptTemplate.from_messages([
    ('system','You are an AI assistant, and provide no more than 220 tokens whey write a response to a question'),
    MessagesPlaceholder(variable_name='chat_history'),
    ('user','{text}')
])
```
where I have indicated the variable ```chat_history``` as a varible to save and load the previous messages.

I set the following consideration toward my local llama 3.1 as:
```python
LLM_03 = ChatOllama(model="llama3.1", Temperature=0.7,do_sample=True)
ChainLLama31=prompt|LLM_03|StrOutputParser()
```
where I designed my Chain (including both my LLM model and prompt).

# Request
Assume that I want to use this Chain for an online shop or other online chatbots to respond to questions.
At the first step, I should define my ```chat_history```variable to save the information. Therefore
```python
chat_history=[]
```
So, assume that the user writes the following sentence to the model:
```'I am an AI specialist who is an expert in online knowledge augmentation from nonstationary Data Streams'```
Therefore, we have:
```python
query='I am an AI specialist who is expert in online knowledge augmentation from nonstationary Data Streams'
response_message=ChainLLama31.invoke({'chat_history':chat_history,'text':query})
```
where the model generates the following response:
```
That's fascinating! Online knowledge augmentation from non-stationary data streams is a challenging problem in the
field of Artificial Intelligence and Machine Learning. Non-stationarity refers to the fact that the underlying
distribution of the data changes over time, making it difficult to develop models that can adapt and learn from the new patterns.

As an expert in online knowledge augmentation, you must be well-versed in techniques such as incremental learning,
transfer learning, and few-shot learning. You likely also have experience with real-time data processing, streaming
algorithms, and model adaptation strategies.

Can you tell me more about your work in this area? What are some of the most significant challenges you've encountered,
and how do you approach solving them?
```
As you can see, the model is intelligent enough to provide insightful data.

# Saving chats
In this step, we have to retain the conversations; therefore, by using ```chat_history``` we have:
```python
chat_history.extend([HumanMessage(content=query), AIMessage(content=response_message)])
```
and save both message as a list of ```[Human, AI]```.
To check the memory of our model, I asked ```'What is my area of expertise?'``` from the model as follows to see the respond:
```python
query='What is my area of expertise?'
response_message=ChainLLama31.invoke({'chat_history':chat_history,'text':query})
print((response_message))
```
where the response was:
```
Your area of expertise is Online Knowledge Augmentation from Non-Stationary Data Streams. Specifically,
you're an expert in developing AI models that can adapt and learn from changing data distributions over
time, making it possible to continuously update and improve knowledge based on new information.
```

Consequently, we can use the old chats to respond the question or analyze the previous data. For example, we can analyze the ```sentiment``` of the previous chats in ```chat_history``` variable for further consideration.



