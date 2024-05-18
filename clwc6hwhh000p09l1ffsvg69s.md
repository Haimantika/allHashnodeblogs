---
title: "Understanding the basics of RAG with a demo"
seoTitle: "RAG Basics Explained with Demo"
seoDescription: "Learn the basics of Retrieval Augmented Generation (RAG) with a practical demo and step-by-step guide"
datePublished: Sat May 18 2024 14:03:07 GMT+0000 (Coordinated Universal Time)
cuid: clwc6hwhh000p09l1ffsvg69s
slug: understanding-the-basics-of-rag-with-a-demo
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716040941704/d3c9ac93-5a6a-4861-8abb-45b80dfd781d.png
tags: ai, programming-blogs, python, generative-ai, rag

---

Ever since Gen AI took the front seat, RAG became a very popular term. By definition, RAG stands for Retrieval Augmented Generation; it is a natural language processing technique that combines the strengths of both retrieval- and generative-based artificial intelligence models.

**When do you build a RAG application?**

Let's understand by example. If you ask a question to ChatGPT about events that happened after 2022, it cannot answer. The reason is, it was last trained in January 2022.

Try with your own prompts to see for yourself. Here's an example below 👇

![A screenshot of a conversation with ChatGPT where a user asks if India won the 2023 World Cup, and ChatGPT responds that it doesn't have information on events after January 2022.](https://cdn.hashnode.com/res/hashnode/image/upload/v1714895483292/1ad1282b-d842-40e4-ab42-704470656d66.png align="left")

*Are the wounds still fresh? 🫣*

If you need to build a system that requires the latest data, existing large language models may not be suitable. LLMs face several challenges, including:

* Presenting false information when they lack the answer.
    
* Providing outdated or generic information when users expect specific, current responses.
    
* Generating responses from non-authoritative sources.
    
* Producing inaccurate responses due to terminology confusion, where different training sources use the same terms to describe different things.
    

In such cases, RAG becomes valuable. It guides the [LLM](https://www.cloudflare.com/en-gb/learning/ai/what-is-large-language-model/) to retrieve relevant information from authoritative, predetermined knowledge sources.

## How does a RAG work?

RAG uses pre-trained LLMs to create text while adding a way to access external knowledge sources. By combining retrieval with generation, RAG helps LLMs produce more accurate and relevant outputs.

RAG works by first finding relevant passages or documents from external sources. These sources are then used as inputs for the generative model, which combines the information and creates coherent and relevant responses. This two-step process, retrieval and generation, is done end-to-end, allowing the model to learn how to choose the best documents and generate the most suitable responses at the same time.

## Steps to build a RAG

Building a RAG involves two main functions: a. Retrieval and b. Generation. This process can be broken down into five steps:

* **Data Collection**
    
    * Gather essential data such as user manuals, product databases, and FAQs relevant to the application, for example, a customer support chatbot.
        
* **Data Chunking**
    
    * Break down large documents into smaller, topic-focused chunks to enhance relevancy and efficiency in data retrieval.
        
* **Document Embeddings**
    
    * Convert text data into embeddings, which are numeric representations that capture semantic meanings. This enables the system to match user queries with relevant information based on content, not just keywords.
        
* **Handling User Queries**
    
    * Transform user queries into embeddings using the same model as for documents to maintain uniformity.
        
* **Generating Responses**
    
    * Use a language model to generate coherent responses based on the relevant text chunks and the initial user query.
        

## Building a RAG

Let us understand how to build one using a simple example. We will be building a PDF summarizer that reads and extracts text from the PDF we upload, retrieves keywords from the corpus, and finally uses the `gpt-3.5-turbo-instruct` model to generate summaries.

1. **First we import the necessary libraries:**
    
    ```python
    from flask import Flask, request, render_template
    from dotenv import load_dotenv
    load_dotenv()  # This loads the variables from .env
    
    OPENAI_API_KEY = os.getenv('OPENAI_API_KEY')
    
    import fitz  # PyMuPDF
    import requests
    import os
    ```
    
2. Next, we define the directory path for uploaded files using the `UPLOAD_FOLDER` constant. If the specified directory doesn't exist, it's created using `os.makedirs()`.
    
    ```python
    app = Flask(__name__)
    
    # Define the directory for uploaded files
    UPLOAD_FOLDER = 'path/to/your/upload/folder'
    app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER
    if not os.path.exists(UPLOAD_FOLDER):
        os.makedirs(UPLOAD_FOLDER)
    ```
    
3. Then we add a static list of corpus, that our RAG model will use to retrieve information:
    
    ```python
    # Sample corpus: List of paragraphs on different topics
    corpus = [
        "Climate change is the long-term alteration of temperature and typical weather patterns in a place.",
        "Machine learning is a type of artificial intelligence that allows software applications to become more accurate at predicting outcomes without being explicitly programmed.",
        "The stock market consists of exchanges where stocks, bonds, and other securities are bought and sold.",
        "Biodiversity refers to the variety and variability of life on Earth, crucial for ecosystems to function and humans to survive."
    ]
    ```
    
4. Then we define 3 functions. The `extract_text_from_pdf()` function takes a file path as input, opens the PDF file using PyMuPDF (`fitz`), extracts text from each page, and returns the concatenated text.
    
    ```python
    def extract_text_from_pdf(filepath):
        doc = fitz.open(filepath)
        text = ''
        for page in doc:
            text += page.get_text()
        return text
    ```
    
5. The `retrieve_relevant_text()` function takes a query as input, splits it into keywords, and returns paragraphs from the `corpus` that contain any of the keywords.
    
    ```python
    def retrieve_relevant_text(query):
        keywords = query.lower().split()
        relevant_texts = [text for text in corpus if any(
            keyword in text.lower() for keyword in keywords)]
        return ' '.join(relevant_texts)
    ```
    
6. The `generate_summary()` function generates a summary of the given text with relevant context using the OpenAI API. It constructs a prompt with the input text and context, sends a request to the OpenAI API, and returns the generated summary.
    
    ```python
    def generate_summary(text, context):
        headers = {
            "Authorization": f"Bearer {OPENAI_API_KEY}",
            "Content-Type": "application/json",
        }
        data = {
            "prompt": f"Summarize the following text:\n\n{text}\n\nRelevant information:\n{context}",
            "temperature": 0.7,
            "max_tokens": 150,
            "model": "gpt-3.5-turbo-instruct",
        }
        response = requests.post(
            "https://api.openai.com/v1/completions", json=data, headers=headers)
        response_data = response.json()
        if 'choices' in response_data and response_data['choices']:
            summary = response_data['choices'][0]['text'].strip()
            return "Here's the summary:\n" + summary
        else:
            error_message = response_data.get(
                'error', {}).get('message', 'Unknown error.')
            return f"Failed to generate summary. Error: {error_message}"
    ```
    
7. We then write code to the main route of the application. When a user accesses the root URL (`/`) or submits a form (POST request) with a file named 'document', this function is called. It checks if a file was uploaded and if it's a valid PDF file, saves the file, extracts text from it, retrieves relevant text from the corpus, generates a summary, and then renders the `index.html` template with the generated summary.
    
    ```python
    def index():
        summary = None
        if request.method == 'POST' and 'document' in request.files:
            file = request.files['document']
            if file and allowed_file(file.filename):
                filepath = os.path.join(app.config['UPLOAD_FOLDER'], file.filename)
                file.save(filepath)
                text = extract_text_from_pdf(filepath)
                context = retrieve_relevant_text(text)
                summary = generate_summary(text, context)
        return render_template('index.html', summary=summary)
    ```
    
8. The `allowed_file()` helper function checks if the filename has a valid extension (PDF) and returns `True` if it does, otherwise returns `False` and then finally runs the Flask app.
    
    ```python
    def allowed_file(filename):
        return '.' in filename and filename.rsplit('.', 1)[1].lower() in ['pdf']
    if __name__ == '__main__':
        app.run(debug=True)
    ```
    

With this demo, you learnt the basics of building a RAG. If you want to see the entire code, you can find it [here](https://github.com/Haimantika/RAG_TIL_conference_demo).