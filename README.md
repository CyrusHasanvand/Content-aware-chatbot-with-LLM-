
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
At the first step, I should define my ```chat_history``` varible to save the information. Therefore
```python
chat_history=[]
```
So, assume that the user writes the following sentence to the model:
```'I am an AI specialist who is expert in online knowledge augmentation from nonstationary Data Streams'```



