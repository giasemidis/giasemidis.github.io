---
layout: post
title: Build a chat-box assistant expert
subtitle: Using the ChatGPT API and Streamlit
tags: [chatgpt,api,chatbox,streamlit,app,basketball,marketing,advertising]
readtime: true
social-share: true
---

In the [previous post](https://giasemidis.github.io/2023/05/08/chatgpt-eassistant.html), I described how to build an expert assistant, which answers only questions in a specific field, using ChatGPT API, streamlit and appropriate prompt engineering. However, the previous version of the app lacked a chat-box functionality.

Here, I describe how to adapt the previous code and add a chat-bot capability to our expert assistant, which can follow-up questions from previous conversations.

To set-up the ChatGPT API, see the corresponding section of the [previous post](https://giasemidis.github.io/2023/05/08/chatgpt-eassistant.html).

We need to make two changes to the previous code:
1. Give chat-box functionality to the streamlit app.
2. Change the ChatGPT API mode and model for tracking the message exchange.

## Chat-box

For the former, we use the [streamlit-chat](https://pypi.org/project/streamlit-chat/) library, which provides a nice interface for a chat-box functionality. Newest messages appear at the top of the chat.

These two references helped me to get started.
- [How to build an LLM-powered ChatBot with Streamlit](https://blog.streamlit.io/how-to-build-an-llm-powered-chatbot-with-streamlit/)
- [Build Your Own Chatbot 🤖 with openAI GPT-3 and Streamlit 🎈](https://medium.com/@avra42/build-your-own-chatbot-with-openai-gpt-3-and-streamlit-6f1330876846)

## ChatCompletion API endpoint

The API endpoint and model used in the previous version did not support continuous conversations, as new messages are not linked with previous ones. ChatGPT API provides the [ChatCompletion](https://platform.openai.com/docs/guides/chat) end-point for that purpose, which is used with model `gpt-3.5-turbo-0301`. This API allows past messages to be included in the query. Quoting the API documentation

> Including the conversation history helps when user instructions refer to prior messages.

> The main input is the messages parameter. Messages must be an array of message objects, where each object has a role (either "system", "user", or "assistant") and content (the content of the message). Conversations can be as short as 1 message or fill many pages.
>
> Typically, a conversation is formatted with a system message first, followed by alternating user and assistant messages.

As the documentation suggests, the `messages` variable starts by giving the chat-bot the "system" role with the prompt:

```python
prompt = (
    "Classify if the following prompt questions are related basketball. "
    "If they are, answer the question. If they are not, reply only "
    "'This is not a basketball question' and do not answer the question."
)
messages =  [
    {
        "role": "system",
        "content": prompt
    },
]
```

For every user input query, we append the `messages` list with

```python
user_input = "<User input>"

messages.append(
    {
        "role": "user",
        "content": user_input
    }

)
```

whereas for the ChatGPT responses, the `messages` is appended with the role `assistant`, i.e.

```python
chatgpt_response = "<ChatGPT response>"

messages.append(
    {
        "role": "assistant",
        "content": chatgpt_response
    }
)
```

After every query, the `messages` variable is passed on the API,

```python
def generate_response_chat(messages):
    """Generate Reponse using GPT3.5 API"""
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo-0301",
        messages=messages,
        max_tokens=256,
        temperature=0
    )
    return response["choices"][0]["message"]
```
where `temperature=0` is used for more deterministic results.

This way, the chat-bot follows the discussion and answers a question in the context of the previous conversation.

In the picture, I demonstrate how the chat-bot follows the context of the discussion and answers without specifying the year in the second question or the player in the third (newest messages appear at the top).

![chatgpt-chatbox-example](https://raw.githubusercontent.com/giasemidis/giasemidis.github.io/master/_posts/figures/chatgpt-api-chatbox-example.png)

### The importance of prompt engineering

The previous version of the app used the model `text-davinci-003` which is available via the `Completion` API end-point. The prompt was:

```python
prompt = (
    "You are a basketball specialist. If a question is not related to "
    "basketball answer 'This is a non-basketball question'. "
    "Limit your answer to 256 tokens if possible."
)
```

and it worked very well separating basketball to non-basketball related questions. However, this prompt does not work well with the `gpt-3.5-turbo-0301` model. The old prompt with the gpt-3.5-turbo model could not distinguish simple non-basketball like "Where is Paris?", in which the model replied as usual to that query, without flagging the query as non-basketball. The funny thing was that even after instructing the model that this was not a basketball-related question and that it should have replied differently, it kept answering the question. The following dialogue actually occurred:

> \> User: Where is Paris?
>
> \> Assistant: Paris is the capital city of France, located in Western Europe.
>
> \> User: Is this a basketball related question?
>
> \> Assistant: This is a non-basketball question.
>
> \> User: What should you have answered then?
>
> \> Assistant: I apologize for the confusion. As a basketball specialist, I should have answered questions related to basketball only.
>
> \> User: Where is Paris?
>
> \> Assistant: Paris is the capital city of France, located in Western Europe.


I had to experiment with the prompt to make it work, with the working version being

```python
prompt = (
    "Classify if the following prompt questions are related basketball. "
    "If they are, answer the question. If they are not, reply only "
    "'This is not a basketball question' and do not answer the question."
)
```

Chat-gpt-3.5-turbo model requires to explicitly state the instruction *classify* in the prompt in order to first perform a classification task of the query and then answer if it is basketball related.

*Obviously, the example illustrated here focuses on basketball, but it can easily be extended to any other topic/field/area, see the example on marketing and advertising below.*

## Code and links
Code and app links can be accessed in the links below.

### **Basketball assistant**

- [App link](https://giasemidis-openai-basketball-assistant-srcmain-jcc4xt.streamlit.app/)
- [GitHub repo](https://github.com/giasemidis/openai-basketball-assistant/blob/main/src/main.py)

### **Marketing & Advertising assistant**
- [App link](https://giasemidis-openai-marketing-assistant-srcmain-n98a5e.streamlit.app/)
- [GitHub repo](https://github.com/giasemidis/openai-marketing-assistant/blob/main/src/main.py)
