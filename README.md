# CHA(A)P: Chat With Any Anonymous Page 
### The Chrome extension chatbot that offers private, smart web interactions. Anonymize and converse with ease for secure form filling and browsing.

[DEVPOST LINK](https://devpost.com/software/chaap-chat-with-any-anonymous-page)

[DEMO VIDEO LINK](https://www.youtube.com/watch?v=80qRcPHnKxA&embeds_referring_euri=https%3A%2F%2Fdevpost.com%2F&source_ve_path=Mjg2NjY)

---

## Introduction
**CHAAP** (Chat With Any Anonymous Page) is a Chrome extension and local Python service that enables you to interact with any webpage you visit in a private, intelligent way. It scrapes the text from the current page (and optionally linked pages), anonymizes it to protect sensitive data, sends the anonymized data to a local or remote Large Language Model (LLM) service for inference, and then returns answers or transformations right in your browser.

You can choose from multiple LLM backends:
- **OpenAI**  
- **AWS** (e.g., hosting a Llama2 or other model on AWS)  
- **OLLAMA** (local Llama-based inference on macOS)  

Thanks to the optional anonymization pipeline, you can be confident that you control whether any Personal Identifiable Information (PII) in the page data will be masked before it's ever sent to any third-party model provider.

---

## Inspiration
We wanted to build a LLM interface right in the browser for users since at the time there were limited options to directly communicate with the content of web pages. We also wanted our solution to interact with any LLM provider from a set of options (scalable to different LLM solutions). Additionally, we were inspired by projects like Nightshade and other chatbots that prioritize privacy and secure data handling. We aimed to provide a “chat with any page” experience but with robust anonymization features that protect user data.  

---

## What It Does
1. **Scrapes Webpage Data**: Captures the textual content of the current webpage and recursively follows some links for additional context if desired.  
2. **Anonymizes Content**: Uses a custom pipeline built on Hugging Face Transformers and Presidio to detect and mask PII.  
3. **Flexible LLM Backend**: Sends anonymized content, along with user queries, to an LLM of your choice.  
4. **Delivers Context-Aware Responses**: The extension then displays answers derived from the masked content, ensuring that no personal or private information leaks to the model.  

---

## Anonymization Feature Discussion

This project given an input of tokens parses it and flags it with a small local language model trained to detect and mask the relevant tokens. We then use another local model such as BERT to fill in the missing tokens with likely candidates. The aim is to allow users to send data on pages such as medical documents or other personally identifiable information to the document. This feature can be used at the users discretion

### The Pros: 
- This concept is helpful for some situations where the user wants to mask all PII and scrub any identifiable information
- In a way this prevents companies from harvesting the data as effectively since the small BERT model reduces the spatial variance of the tokens and could possibly have incorrect names tied to information which in a critical mass can lead to less effective models (similar to the Nightshade image protection model)

### The Cons:
- If a website was chatted with our extension about Abraham Lincoln or some other identifiable person, all names and locations would be scrubbed and a lot of information would be lost
- Additionally, the names and locations may not be the same across the 

There is a lot of improvement, but as a concept for the hackathon we decided to keep this feature knowing both pros and cons.
---

## How We Built It
- **Frontend**: A Chrome extension using JavaScript, HTML, and minimal CSS. It provides the user interface to capture the webpage text, sends data to the backend, and displays the AI responses in a simple chatbox UI.  
- **Backend**: A Python FastAPI service that handles incoming requests, anonymizes the data, stores it in a PostgreSQL database, and interfaces with different LLM providers.  
- **Vector Database**: We used [Tembo](https://docs.tembo.io/) for vector storage and retrieval.  
- **Anonymization**: We used [Presidio](https://microsoft.github.io/presidio/) and a Hugging Face `fill-mask` pipeline for detecting and masking personal data.  

---

## Challenges
1. **Chrome Extension Development**: Managing content scripts, background scripts, and messaging between them.  
2. **Vectorization and Query**: Setting up a pipeline so that chunked and anonymized data can be stored and retrieved for relevant context.  
3. **Configuring Tembo**: Ensuring vectorize functionality works properly with large text chunks.  
4. **Separation of Data**: Making sure that the data never leaves the local environment in raw form—our anonymization step runs before sending to an external LLM.  

---

## Accomplishments
- Successfully integrated an anonymization pipeline with multiple LLM backends.  
- Built a fully functioning Chrome extension that can scrape and sanitize data automatically.  
- Achieved reliable communication between the browser extension and Python backend via a REST API.  

---

## What We Learned
- The complexities of Chrome extension development can be surprisingly tricky.  
- Integrating multiple LLM providers is both rewarding and challenging, especially when balancing resource constraints, Docker containers, local development, and ephemeral cloud services.  
- Building an anonymization pipeline with Presidio and Hugging Face can be an involved but powerful approach to privacy in AI.  

---

## What’s Next if the Project Continued
- **Image Integration**: Process images or embedded PDF files for even richer context.  
- **Enhanced Anonymization**: Add more sophisticated detection techniques or custom rulesets to keep replaced PII consistent.
- **Full Integration**: Integrate the LLM switching and handling through the code of the extension itself rather than the local Python API. 

---

## Built With
- **amazon-web-services** (AWS)  
- **chrome**  
- **fastapi**  
- **html5**  
- **huggingface**  
- **javascript**  
- **langchain**  
- **ollama**  
- **openai**  
- **python**  
- **tembo**  

