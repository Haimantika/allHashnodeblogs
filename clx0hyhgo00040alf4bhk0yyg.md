---
title: "Building a chatbot using Microsoft Copilot - the no code way"
seoTitle: "No-Code Chatbot Creation with Microsoft Copilot"
seoDescription: "Learn to build a chatbot using Microsoft Copilot without coding. Enhance productivity with AI-powered tools in Microsoft 365"
datePublished: Tue Jun 04 2024 14:30:25 GMT+0000 (Coordinated Universal Time)
cuid: clx0hyhgo00040alf4bhk0yyg
slug: building-a-chatbot-using-microsoft-copilot-the-no-code-way
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1717501474866/9156d313-e12b-4862-a686-4d0eea0d2333.png
tags: ai, beginners, copilotstudio

---

By now, you must have seen chatbots on almost every business website and even forums. With generative AI, the chatbots are now faster and better. You no longer have to worry about outdated information, training your chatbots for a long time, or creating a knowledge base from scratch.

In this article, I will introduce you to Microsoft Copilot and show how you can build a chatbot that fetches data directly from a website.

## What is Microsoft 365 copilot?

Microsoft Copilot is an AI-powered tool that works with Microsoft 365 apps to help users be more productive and creative. It combines large language models (LLMs) with an organization's data to provide real-time assistance.

It includes features like:

* Chat with text, voice, and image capabilities
    
* Image creation in Designer
    
* Web grounding
    
* Use of plugins and Copilot GPTs
    

## Preparing to build the chatbot

Building the chatbot is fairly easy using the Copilot Studio, and the only prerequisite you need is to have a Microsoft 365 subscription. If you are a developer like me, you can sign up for the [Microsoft 365 developer program](https://developer.microsoft.com/en-us/microsoft-365/dev-program) for free. Along with Copilot Studio access you also get a tonne of other benefits.

So what are we building today? A chatbot that can help you answer banking related queries. For this demo, I have used [State Bank of India](https://sbi.co.in/). Depending on your use case, you can make the required changes.

This is how the information flows 👇

![A flowchart illustrating the process where a website (sbi.co.in) fetches data and sends it to Copilot Studio, which adds descriptions and other information. The processed data is then sent to a bot, which generates a response.](https://cdn.hashnode.com/res/hashnode/image/upload/v1717499453831/ce3f2570-e5f2-4d10-a17d-c0309433cc7a.png align="center")

## Steps to build the bot

1. Sign in to [Microsoft Copilot](https://powerva.microsoft.com/) [Studio.](https://powerva.microsoft.com/)
    
2. Click on the `+Create` option from the left pane and choose `New Copilot`. Depending on your use case, you can also start with a template.
    
    ![Screenshot of the Copilot Studio interface showing options to create a new copilot or a new Microsoft Copilot action. Below these options, there are various templates available, including Safe Travels, Store Operations, Sustainability Insights, Website Q&A, Approval Manager, CV Match, Job Craft, Kudos, Wellness Check, and Status Tracker. Some templates are marked as "Coming soon."](https://cdn.hashnode.com/res/hashnode/image/upload/v1717499685863/3fbc1905-1a4b-40fb-a744-30ebb4fe6e5a.png align="center")
    
3. Once you click on `New Copilot`, you get to see a screen as shown in the image below. The click on `Skip to configure` to add details for your chatbot.
    
    ![Screenshot of the Copilot Studio interface. The screen shows a prompt saying, "Hi, I’m here to help you build a custom copilot. In a few sentences, how will your copilot assist your users?" Below the prompt is a text box labeled "Type your message" with a send button. There are buttons labeled "Skip to configure" and "Create" at the top right. The interface indicates the primary language is English.](https://cdn.hashnode.com/res/hashnode/image/upload/v1717499801415/fac46b77-4a67-455d-8d2a-062b9d1838d1.png align="center")
    
4. Add a name for the copilot, add required description, and instructions and then add the knowledge source. You will get multiple sources as options, but for this demo, choose `Public websites` and add the link to the website that will act as knowledge base for your bot.
    
    ![A screenshot of the "Add available knowledge sources" window in Copilot Studio. It lists various sources such as Public websites, SharePoint and OneDrive, Files, Dataverse, Microsoft Fabric, Enterprise websites, Azure SQL, ADLS Gen2, Salesforce, ServiceNow Knowledge, File share, MediaWiki, and SharePoint Server. There is a "Close" button at the top right and a "Cancel" button at the bottom right.](https://cdn.hashnode.com/res/hashnode/image/upload/v1717499907023/3fb82a77-20d8-409c-be74-5af7f1e449c8.png align="center")
    
5. Once you have filled in all the details, this is how it will look 👇. The click on `Create` to create the conversational chatbot.
    
    ![Screenshot of the Copilot Studio interface showing the creation of a helpdesk named "SBI helpdesk". The description states that the copilot helps users with loans, investments, and other financial services provided by SBI. Instructions are to answer in a polite and professional manner. A knowledge source URL is also provided.](https://cdn.hashnode.com/res/hashnode/image/upload/v1717499973871/628646b9-4fe8-452e-91a0-d816e7a4f8e0.png align="center")
    
6. As a final set before publishing it to various channels, test the bot with various queries and see if it responds correctly.
    
    ![A chat interface where a user asks for investment options for a 25-year-old working woman. The response suggests investing in Mutual Funds for tax-saving, retirement planning, and education, and exploring Fixed Income Investment Options (Bonds) for stable returns. There is a reference link provided for more information.](https://cdn.hashnode.com/res/hashnode/image/upload/v1717500188289/bb56cc09-8376-4ff0-ae2f-e4294e5eecfd.png align="center")
    

## Extending your Copilot

Now that we have built a chatbot, let us take it a step ahead, and ask it to collect information such as full name, email address, and phone number. To do that, you need to build a flow. Microsoft Copilot has [Power Automate](https://www.microsoft.com/en-us/power-platform/products/power-automate) inbuilt and it can be easily done with the help of Copilot.

1. Go to the `Topics` from the menu bar and click on `Add a topic`.
    
2. You have two options: `Create from blank` or `Create a description with Copilot`.
    
3. To make it simple, choose `Create a description with Copilot` and fill in the details such as:
    
    * Name a topic to: `Book a call with an agent`
        
    * Create a topic to:`Collect a user's full name, email, and phone number`
        
4. Then click on `Create` and the topic gets created for you.
    
    ![Screenshot of the Copilot Studio with the topic created](https://cdn.hashnode.com/res/hashnode/image/upload/v1717500739630/78e4511e-4f3c-4f76-80e2-10b5d645e8e9.png align="center")
    
    You can make more updates such as:
    
    * Updating the trigger phase
        
    * Summarizing the information gathered from the user. You can do this by clicking on the `Copilot` option as shown in the image below.
        
    
    ![A screenshot of the Copilot Studio interface. The left sidebar shows a list of copilots, including "SBI helpdesk" under Custom copilots, and "Copilot for Microsoft 365" and "Copilot for Sales" under Microsoft. The main section displays the "SBI helpdesk" topic with options to edit with Copilot, add comments, variables, and more. The right panel includes a chat interface for testing the copilot, with a sample message from the SBI helpdesk virtual assistant.](https://cdn.hashnode.com/res/hashnode/image/upload/v1717501111396/c97f7e6e-fb5e-4e81-a67b-95b79977f23b.png align="center")
    

## Conclusion

In this brief article, you learnt what is Microsoft Copilot and how you can build a custom copilot for your business needs using no code. In the next article, we will learn how we can build it the pro code way.

Some resources that can help you get started:

* [Create Power Platform solutions with AI and Copilot](https://learn.microsoft.com/en-us/training/paths/copilot-solutions/)
    

* [Create Copilots with Copilot Studio](https://learn.microsoft.com/en-us/training/paths/power-virtual-agents-workshop/)