---
layout: post
title: How to make your own app of an expert assistant
subtitle: Using the ChatGPT API and Streamlit
tags: [chatgpt,api,streamlit,app,basketball,marketing,advertising]
readtime: true
social-share: true
---

I was playing around with the [ChatGPT API](https://platform.openai.com/docs/api-reference) and thought what a better way to test it by making my own specialised assistant, which answers only questions on a specific topic/field/subject/industry. In the meantime, I was reading about prompt engineering for generative AI applications, like ChatGPT, see [this free online course](https://learnprompting.org/docs/intro). In ChatGPT, good prompting techniques are essential for getting the best responses. We need to provide the AI the most context so that it returns the best content for us.

## Setting up ChatGPT API
Let's get started. First, we head to the [API-keys page](https://platform.openai.com/account/api-keys) and create an api key, if there isn't one already. The account should have credits, any new accounts come with $5.00 free credit. We can check the usage on the [Usage page](https://platform.openai.com/account/usage). If you don't have any credit, you might need to add, see the [Billing page](https://platform.openai.com/account/billing/overview).

We also need the organisation id, which is found on the [Settings page](https://platform.openai.com/account/org-settings).

Save the API key and ORG ID as environment variables with names `OPENAI_API_KEY` and `OPENAI_API_ORG`, respectively.

Before, we go on and write our Python app, we can experiment with the ChatGPT API, its endpoints and parameters on the [Playground page](https://platform.openai.com/playground). The cool thing with the API is that one can control the model hyper-parameters, like the "Temperature", "Top P" and "Best of". These parameters determine how deterministic or random the model should be. We can try all the prompts on the Playground page before we code up our application. Beware that the requests on the Playground page charge from the credit.


## Prompt
We want to create an expert assistant that answers questions about a specific subject and ignores everything else. We pick basketball as the subject. Part of every query should be the role of the assistant. We prepend to each prompt the following:

"You are a basketball specialist. If a question is not related to basketball answer 'This is a non-basketball question'"

To generalise to any subject simply replace "basketball" to your <subject> of choice.

This way, we give ChatGPT context for more accurate results and let it deal with the unrelated questions.

We use the `Completion` endpoint and "text-davinci-003" model. See the [models](https://platform.openai.com/docs/api-reference/models) and [endpoints](https://platform.openai.com/docs/models/overview) sections for a full list of capabilities and limit rates.

## Streamlit App

The Streamlit app is simply to make, here is a snippet of code

```python
# main.py
import os
import streamlit as st
import openai


def main():
    openai.organization = os.getenv("OPENAI_API_ORG")
    openai.api_key = os.getenv("OPENAI_API_KEY")

    st.title("GIASE: Your basketball specialist.")

    st.write("This is your basketball specialist. Ask me any basketball related question. Ah! I have no knowledge of 2022 onwards, because I am powered by ChatGPT. So, I don't do predictions.")

    st.write("**Example:** *'Who won the NBA finals in 2011 and who won the finals MVP?'*")
    st.write("I don't answer questions like *'Who was US president in 2010?'*")

    input_txt = st.text_area("Ask me your question. Answers will be limited to 256 tokens")

    prompt = "You are a basketball specialist. If a question is not related to basketball answer 'This is a non-basketball question'. Limit your answer to 256 tokens if possible."

    if input_txt != "":
        prompt += input_txt

        response = openai.Completion.create(model="text-davinci-003", prompt=prompt, max_tokens=256)
        # Print answer to the screen
        st.write(response["choices"][0]["text"])

    return

if __name__ == "__main__":
    main()
```

To run the app locally, simply run

```
streamlit run main.py
```
and the app should open on http://localhost:8501.

Streamlit apps can be deployed via GitHub. Head to [Streamlit](https://streamlit.io/), login and follow the instructions for adding a new app. The code should be on a public GitHub repo.


### **Basketball assistant**

- [App link](https://giasemidis-openai-basketball-assistant-srcmain-jcc4xt.streamlit.app/)
- [GitHub repo](https://github.com/giasemidis/openai-basketball-assistant/blob/main/src/main.py)

### **Marketing & Advertising assistant**
- [App link](https://giasemidis-openai-marketing-assistant-srcmain-n98a5e.streamlit.app/)
- [GitHub repo](https://github.com/giasemidis/openai-marketing-assistant/blob/main/src/main.py)
