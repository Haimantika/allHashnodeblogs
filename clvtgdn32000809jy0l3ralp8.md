---
title: "Build a translator app using Azure AI"
seoTitle: "Create a Translator App with Azure AI"
seoDescription: "Learn to build your own translator app using Azure AI with this step-by-step guide"
datePublished: Sun May 05 2024 11:32:07 GMT+0000 (Coordinated Universal Time)
cuid: clvtgdn32000809jy0l3ralp8
slug: build-a-translator-app-using-azure-ai
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1714908606852/244c4ff4-7794-4369-9bc9-8767412983fd.png
tags: ai, azure, web-development

---

I travel a lot, and while travelling, `Google translator` becomes my best friend. Be it asking for help with directions or making a new friend, a translator has been my companion. Like any other developer during a weekend, I thought, "Can I build a hacky version of it for myself?" And I did 👉 [https://langbuddy.netlify.app/](https://langbuddy.netlify.app/) (this is my version of "The UI sucks, but it works").

Well enough of the introduction, let's start building!

## Prerequisites

* Azure subscription - To use Azure AI services
    
* Working knowledge of JavaScript - To build the logic
    

## What is Azure AI?

Azure AI services are here to help developers and organizations quickly build smart, advanced, market-ready, and responsible applications. They offer ready-to-use and customizable APIs and models for a variety of tasks. You can use them for things like chatting, searching, monitoring, translating, speech processing, vision tasks, and making decisions.

There are loads of Azure AI services available. Here are just a few to get you started:

* [Azure AI Search](https://learn.microsoft.com/en-us/azure/search/)
    
* [Azure OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/)
    
* [Translator](https://learn.microsoft.com/en-us/azure/ai-services/translator/)
    
* [Immersive Reader](https://learn.microsoft.com/en-us/azure/ai-services/immersive-reader/)
    
* [Face](https://learn.microsoft.com/en-us/azure/ai-services/computer-vision/overview-identity)
    

You can read more about Azure AI services [here](https://azure.microsoft.com/en-in/products/ai-services). In this demo, we will be using Azure AI translator to build the app.

## What is Azure AI translator?

Azure AI Translator is a handy cloud-based service that lets you translate text and documents just by making a simple REST API call. It's powered by the latest neural machine translation technology. Plus, there's a cool feature called the `Custom Translator interface`. This allows you to bring your own translation memories to craft personalized neural translation systems.

Here are some of the features supported by the Translator service:

* [**Text Translation**](https://learn.microsoft.com/en-us/azure/ai-services/translator/text-translation-overview)
    
* [**Asynchronous Batch Document Translation**](https://learn.microsoft.com/en-us/azure/ai-services/translator/document-translation/overview)
    
* [**Synchronous Document translation**](https://learn.microsoft.com/en-us/azure/ai-services/translator/document-translation/reference/synchronous-rest-api-guide)
    
* [**Custom Translator**](https://learn.microsoft.com/en-us/azure/ai-services/translator/custom-translator/overview)
    

## Creating a translator resource in the Azure portal

To use the translator within your app, you can either use the SDK or API. In this demo, we will be using the API.

1. Go to `portal.azure.com`
    
2. Click on `Create a resource` and then search for translator. You should see the following options:
    
    ![Screenshot of Microsoft Azure Marketplace showing various translator services available for subscription, including options like "Translator" by Microsoft, "Power Translate - Power BI .PBIX Translator" by Saga Technology, and "Pig Latin Translator Premium" by ISVTestUKLegacy.](https://cdn.hashnode.com/res/hashnode/image/upload/v1714907032863/a6efb65a-7605-4de8-9ab3-b49d73fe63e8.png align="center")
    
3. Select the first option, that says `translator`.
    
4. In the next step, fill the fields as shown in the image below. (*Make sure to choose a unique name for your app*)
    
    ![Screenshot of the Microsoft Azure portal showing the "Create Translator" setup page with fields for subscription, resource group, region, and instance details filled out.](https://cdn.hashnode.com/res/hashnode/image/upload/v1714907206979/8cb31965-3e01-4efc-8686-f20f3dfe0ca0.png align="center")
    
5. Then click on create and once the resource has been created, copy the key and the location from the `Keys and endpoints` page, as we will need it in the next steps.
    
    ![Screenshot of Microsoft Azure's management interface showing the "Keys and Endpoint" section of a Translator service, with options to view and regenerate API keys, and details of the service's location and endpoints.](https://cdn.hashnode.com/res/hashnode/image/upload/v1714907313080/0f1890c8-3a4b-471b-b011-2930be1344b8.png align="center")
    

## Building the app

The next step is to connect the translator api to your application. For reference, you can take help from the [quickstart tutorial](https://learn.microsoft.com/en-us/azure/ai-services/translator/quickstart-text-rest-api?tabs=csharp) as shown below:

```javascript
const axios = require('axios').default;
    const { v4: uuidv4 } = require('uuid');

    let key = "<your-translator-key>";
    let endpoint = "https://api.cognitive.microsofttranslator.com";

    // location, also known as region.
    // required if you're using a multi-service or regional (not global) resource. It can be found in the Azure portal on the Keys and Endpoint page.
    let location = "<YOUR-RESOURCE-LOCATION>";

    axios({
        baseURL: endpoint,
        url: '/translate',
        method: 'post',
        headers: {
            'Ocp-Apim-Subscription-Key': key,
             // location required if you're using a multi-service or regional (not global) resource.
            'Ocp-Apim-Subscription-Region': location,
            'Content-type': 'application/json',
            'X-ClientTraceId': uuidv4().toString()
        },
        params: {
            'api-version': '3.0',
            'from': 'en',
            'to': 'fr,zu'
        },
        data: [{
            'text': 'I would really like to drive your car around the block a few times!'
        }],
        responseType: 'json'
    }).then(function(response){
        console.log(JSON.stringify(response.data, null, 4));
    })
```

The above code is the main logic that you need in your app, you can build the frontend as per your liking.

Since this is a simple demo application, I did not want it to be complicated and added the following 5 components:

* A header
    
* An text box to take input from users
    
* A drop down menu to choose the language to translate into
    
* A button that translates the text
    
* A form that displays the output
    

Then I proceeded to build it using HTML, TailwindCSS and some JavaScript.

Here's the code for each block:

1. **Header:**
    
    ```xml
    <div class="container">
            <h1 class="heading mb-4 text-4xl font-extrabold leading-none tracking-tight text-gray-900 md:text-5xl lg:text-6xl dark:text-white"><span class="text-blue-600 dark:text-blue-500">The AI </span> translator app.</h1>
            <p class="text-lg font-normal text-gray-500 lg:text-xl dark:text-gray-400">In a new country? Making friends from all across the world? This app will help you make conversations faster and easier.</p>
    ```
    
2. **Text box:**
    
    ```xml
    <div class="input-group">
                <label for="message" class="text-sm font-medium text-gray-900 dark:text-white">Your message</label>
                <textarea id="textInput" rows="2" class="block p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500 small-textarea" placeholder="Enter the text you want to translate"></textarea>
            </div>
    ```
    
3. **Drop-down menu:**
    
    ```xml
    <div class="input-group">
            <label for="targetLanguage" class="text-sm font-medium text-gray-900 dark:text-white">Target Language</label>
            <select id="targetLanguage" class="p-2.5 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
                <option value="fr">French</option>
                <option value="es">Spanish</option>
                <option value="hi">Hindi</option>
            </select>
        </div>
    ```
    
4. **Button:**
    
    ```xml
    <button type="button" class="text-white bg-gradient-to-br from-purple-600 to-blue-500 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-blue-300 dark:focus:ring-blue-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center me-2 mb-2" onclick="translateText()">Translate</button>
    ```
    
5. **Form:**
    
    ```xml
     <form class="small-form">
            <label for="message" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Output</label>
            <div class="w-full mb-4 border border-gray-200 rounded-lg bg-gray-50 dark:bg-gray-700 dark:border-gray-600">
                <div class="px-4 py-2 bg-white rounded-t-lg dark:bg-gray-800">
                    <label for="comment" class="sr-only">Translated Text</label>
                    <textarea id="translatedText" rows="2" class="w-full px-0 text-sm text-gray-900 bg-white border-0 dark:bg-gray-800 focus:ring-0 dark:text-white dark:placeholder-gray-400 small-textarea" readonly></textarea>
                </div>
            </div>
        </form>
    ```
    

And the final part was to stitch the frontend code with the logic to make the app work!

That's how I built my [language buddy](https://langbuddy.netlify.app/) in just a few hours!

Thank you for reading till the end. I hope this tutorial has been helpful to you. If you have interesting ideas to share with me, shoot a DM [@Haimantika](https://twitter.com/HaimantikaM)