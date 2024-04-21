---
title: "Building search functionality in your app using Azure AI: Part 1"
seoTitle: "Integrate Search in Apps with Azure AI"
seoDescription: "Learn to build and integrate Azure AI-powered search functionality into your app with this step-by-step guide"
datePublished: Sun Apr 21 2024 14:30:29 GMT+0000 (Coordinated Universal Time)
cuid: clv9ml2x7000309judmq9fypv
slug: building-search-functionality-in-your-app-using-azure-ai-part-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1713700370365/747346ad-fdb1-4f05-bf0a-3834c06603f9.png
tags: ai, azure

---

You've seen search functionality in nearly every application you use daily, from your favorite web engine to e-commerce sites and OTT platforms. Have you ever wondered how you can build one? In this article, I will provide a step-by-step walkthrough.

Before we dive into the demo, let's understand some terms to clarify the basics.

## Search functionality basics

A search service acts as an intermediary between the external data stores, where all your raw data resides, and your client application, where you submit search queries and receive responses.

On the search service side, there are two primary functions: *indexing* and *querying*.

* [**Indexing**](https://learn.microsoft.com/en-us/azure/search/search-what-is-an-index) involves processing content by breaking it down (such as converting text into tokens) and organizing it in a manner that facilitates easy searching. This processed data is stored in structures known as inverted indexes, and in cases involving vectors, these are stored in vector indexes.
    
* [**Querying**](https://learn.microsoft.com/en-us/azure/search/search-query-overview) begins once the index contains searchable content. At this stage, your application sends a search query to the search service, which then retrieves and returns the relevant data. The interaction occurs over a search index that you manage.
    
* In scenarios where AI-powered search is utilized, as in this demo, [semantic ranking](https://learn.microsoft.com/en-us/azure/search/semantic-search-overview) plays a crucial role. This feature enhances the search queries by improving language comprehension and ensuring that the most pertinent results are given priority.
    

## Building search functionality using Azure AI

### Creating an index

The first step in creating a search solution is to build an index, which you can do using the Azure portal or the CLI.

1. Using Azure portal:
    
    * Sign in to the [Azure portal](https://portal.azure.com/).
        
    * Select the [**Import data wizard**](https://learn.microsoft.com/en-us/azure/search/search-import-data-portal) **-** The wizard provides a complete workflow that creates an indexer, a data source, and a finished index, and it also loads the data. If you need a simpler option, choose **Add Index** instead.
        
        ![Screenshot of the Microsoft Azure portal interface showing the demo-search-svc page with options to "Add index" and "Import data" highlighted.](https://learn.microsoft.com/en-us/azure/search/media/search-what-is-an-index/add-index.png align="center")
        
2. Using the Azure CLI:
    
    * You need to create a resource first using the following commands:
        
        ```bash
        az login
        ```
        
    * Create a resource group
        
        ```bash
        az search service create --name my-search-demo --resource-group cognitive-search-demo-rg --sku free --partition-count 1 --replica-count 1
        ```
        
    * Get a query key that provides read access to a search service.
        
        ```bash
        az search query-key list --resource-group cognitive-search-demo-rg --service-name my-cog-search-demo-svc 
        ```
        
    * Get a search service admin API key, which provides write access to the search service.
        
        ```bash
        az search admin-key show --resource-group cognitive-search-demo-rg --service-name my-cog-search-demo-svc
        ```
        
    * Now that the service is created, to load the data you can [use a script](https://github.com/Azure-Samples/azure-search-javascript-samples/blob/main/search-website-functions-v4/bulk-insert/bulk_insert_books.js) as mentioned in Microsoft docs.
        

Great job! You have completed the first task, next up is using the Azure AI APIs in your application to use the index you just created.

## Testing the index

To check if your query meets your requirements, you can use the `Create demo app` option within the Azure portal to test the query.

![Screenshot of the Microsoft Azure portal showing the "good-books" resource with options like "Create Demo App", "Edit JSON", and "Delete". The page displays statistics such as 10,000 documents and 7.8 MB total storage.](https://cdn.hashnode.com/res/hashnode/image/upload/v1713699976534/7a60148f-d675-4fc1-908d-235c09fd54a7.png align="center")

![Screenshot of a Microsoft Azure interface for creating a demo app, showing options to customize individual search results with fields for thumbnail, title, and description.](https://cdn.hashnode.com/res/hashnode/image/upload/v1713699987513/54c9e634-6300-4630-8974-c9cbc645b0f2.png align="center")

This is how the demo will look like:

%[https://www.loom.com/share/dc78a4759bbf4642a395fd8049c63dd7?sid=5fcf11b6-be1c-4a23-9149-f051580df9d4] 

And that's all about creating an index, next up we will see how we can use Azure AI APIs to integrate this index into our web app.