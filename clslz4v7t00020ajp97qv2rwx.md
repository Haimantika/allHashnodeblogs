---
title: "Come build an AI powered app with me"
datePublished: Wed Feb 14 2024 15:55:54 GMT+0000 (Coordinated Universal Time)
cuid: clslz4v7t00020ajp97qv2rwx
slug: come-build-an-ai-powered-app-with-me
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1707922923460/0d2352d2-d20b-4085-8c09-a179bb77432a.png
tags: ai, javascript, html, apis, openai, tailwind-css

---

Year 2024 is called year 1 of AI (year 0 was 2023), and by now you have seen almost every company adopt AI in some way or the other. As a developer, I loved playing around with prompts and also the [OpenAI API](https://openai.com/blog/openai-api) and ended up building a Contributing Guide generator. Watch it in action here 👇

%[https://twitter.com/HaimantikaM/status/1747638951094141202] 

In this blog, we will cover how to build a simple app from scratch using HTML, TailwindCSS and JavaScript.

*Prerequisites:*

* *Understanding of HTML, CSS (Tailwind preferred) and JavaScript*
    
* *Understanding of how APIs work*
    
* *Very basic understanding of GenAI*
    
* [*OpenAI API key*](https://openai.com/blog/openai-api)*\[Note: We will be using the free version, you can find about the rate limits*[*here*](https://platform.openai.com/docs/api-reference)*\]*
    

### OpenAI capabilities

Before diving straight into code, here's an overview of everything that you can do with OpenAI API:

* [Text generation](https://platform.openai.com/docs/guides/text-generation) - The text API helps in generating texts for you. It can be anything, a full essay or poems or captions.
    
* [Assistants](https://platform.openai.com/docs/assistants/overview) - The Assistants API allows you to build AI assistants within your own applications.
    
* [Embeddings](https://platform.openai.com/docs/guides/embeddings) - OpenAI’s text embeddings measure the relatedness of text strings. It can be used for search, recommendation, anomaly detection, etc.
    
* [Speech to text](https://platform.openai.com/docs/guides/speech-to-text) - The Audio API helps you convert audio into text.
    
* [Image generation](https://platform.openai.com/docs/guides/images/introduction?context=node) - The Images API helps you interact with images, be it creating variations of an existing image, or creating images based on a prompt.
    
* [Vision](https://platform.openai.com/docs/guides/vision) - `gpt-4-vision-preview` in the API, allows the model to take in images and answer questions about them.
    

### Ready to code? 👩‍💻

Now that you have a basic understanding of OpenAI's capabilities, it is time to build with their [Completions API](https://platform.openai.com/docs/guides/text-generation/completions-api).

* **Building the frontend:**
    
    Let's start with making a list of components that is needed for this application:  
    \- A header  
    \- Text elements  
    \- A form to enter the prompt  
    \- A card to display the result
    
    And that's it! Let's get started 💪
    
* First step is to install the [Play CDN script](https://tailwindcss.com/docs/installation/play-cdn)
    
    ```html
    <html>
    <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <script src="https://cdn.tailwindcss.com"></script>
    </head>
    ```
    
* Since we are using Tailwind, you can use their documentation to design the [UI components](https://tailwindui.com/?ref=top) and if you want to refer to my design, you can find the entire code [here](https://github.com/Haimantika/Contributing-guide-generator).
    
* The next step is to ensure that the API gives a response only when the button is clicked. We can do this by writing a few lines of JavaScript:
    
    ```javascript
    document.addEventListener('DOMContentLoaded', () => {
              const generateButton = document.getElementById('generateButton');
              generateButton.addEventListener('click', async (event) => {
                event.preventDefault(); // Prevent the form from submitting traditionally
                const prompt = document.getElementById('prompt').value;
    ```
    
    * *Note: If you want the API to respond if you click "Enter", you just need to make the follow changes:*
        
        ```xml
        <button
                    type="submit"
                    id="generateButton"
                    class="rounded-md bg-black px-3 py-2.5 text-sm font-semibold text-white shadow-sm hover:bg-black/80 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-black"
                  >
        ```
        
    * Next, we would want the model to respond correctly for misspelling or variations of `Contributing Guide` . You can either handle it by fine-tuning the model or hard-coding it with some common variations such as:
        
        ```javascript
        // List of common misspellings or variations
                    const validPhrases = [
                    "Contributing guide",
                    "Contributing Guide",
                    "Contributing",
                    "Contribution",  
                    "contributing guide",
                    "contributeing guide",
                    "contributing gide",
                    "contirbution guide",
                    "contribution"
                  // Add more variations as needed
                ];
        
                // Check if the prompt includes any of the valid phrases
                  const isValidPrompt = validPhrases.some(phrase => prompt.includes(phrase));
                  if (!isValidPrompt) {
                    alert("Please include 'contributing guide' or a similar phrase in your prompt.");
                    return; // Exit the function if no valid phrase is found
              }
        ```
        
    * The final step is call the `Completions API` and do some error handling.
        
        ```javascript
         try {
                      const response = await fetch('https://api.openai.com/v1/completions', {
                        method: 'POST',
                        headers: {
                          'Content-Type': 'application/json',
                          'Authorization': 'Bearer <API Key>' // Replace with your actual OpenAI API Key
                        },
                        body: JSON.stringify({
                          model: 'gpt-3.5-turbo-instruct',
                          prompt: prompt,
                          max_tokens: 1000
                        })
                      });
              
                      if (!response.ok) {
                        throw new Error('Network response was not ok');
                      }
              
                      const data = await response.json();
                      document.getElementById('guideOutput').innerText = data.choices[0].text;
                    } catch (error) {
                      console.error('Error:', error);
                      document.getElementById('guideOutput').innerText = 'Error generating text';
                    }
        ```
        
        And bam! That's all you need to create a simple AI app.
        

### Ending notes

Hope you had fun reading this article. It was built as a weekend hack to play around with the OpenAI API. If you have any suggestions feel free to drop it [here](https://github.com/Haimantika/Contributing-guide-generator).

Here are some improvements to work on (it is open source, contributions are welcome):

* Write server-side code to not expose the API key or use Vercel's serverless functions to hide the key.
    
* Adding an option for users to add their own API key and generate contributing guide.
    

*P.S. - The application is live on* [*guidemaster.xyz*](https://guidemaster.xyz/)*, but it will show you "Error generating text" as the API is now disabled.*