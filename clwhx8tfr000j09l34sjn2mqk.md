---
title: "How to create a GitHub stats tool using OpenSauced, Tailwind, and JavaScript"
seoTitle: "GitHub Stats Tool with OpenSauced & Tailwind"
seoDescription: "Build a GitHub stats tool using OpenSauced APIs, Tailwind, and JavaScript. Discover trending projects by stars and forks"
datePublished: Wed May 22 2024 14:30:44 GMT+0000 (Coordinated Universal Time)
cuid: clwhx8tfr000j09l34sjn2mqk
slug: how-to-create-a-github-stats-tool-using-opensauced-tailwind-and-javascript
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716364940460/43b1a17e-50e3-4fc0-a7be-7c61801c8bc1.png
tags: github, javascript, webdev, html5, tailwind-css

---

If you're an open-source enthusiast, you've probably found yourself checking out the `trending` tab on GitHub to see which projects are hot. I did the same! Now, imagine a tool that shows you which projects got the most stars and forks in the last 24 hours and helps you discover new ones. Sounds awesome, right? Well, your imagination is now a reality because I built [projectstats.netlify.app](https://projectstats.netlify.app/).

In this article, I'll dive into how I created this tool in just a few hours using [OpenSauced APIs](https://api.opensauced.pizza/), JavaScript, and some Tailwind magic.

Let's kick things off with a quick introduction 👇

## What is OpenSauced? 🍕

[OpenSauced](https://opensauced.pizza/) is a platform designed to support maintainers and teams, redefining the concept of open source contributions.

Key features of OpenSauced:

* [**StarSearch**](https://docs.opensauced.pizza/features/star-search/) **-** StarSearch is an AI-powered feature that provides in-depth insights into contributor history and activities, enabling users to query GitHub activities and analytics through natural language.
    
* [**Workspaces**](https://docs.opensauced.pizza/features/workspaces/) **-** Workspace is a virtual environment for managing information for individual productivity, team collaboration, and company-wide operations.
    
* [**Repository Insights**](https://docs.opensauced.pizza/features/repo-insights/) **-** It provides a comprehensive view of open source project's health and contributions. OpenSauced Repository Insights helps you make data-driven decisions that align with your goals.
    
* [**Contributor Insights**](https://docs.opensauced.pizza/features/contributor-insights/) **-** This feature enables you to categorize, monitor, and analyze different groups of contributors within open source projects.
    
* [**Repository Pages**](https://docs.opensauced.pizza/features/repo-pages/) **-** Repository Pages allow you to view specific information about a repository hosted on GitHub through a detailed visual and analytical representation of the project.
    
* [**Highlights**](https://docs.opensauced.pizza/features/highlights/)**\-** The Highlights feature is the place you can display your favorite open source contributions, share the story, and inspire others to join you in your open source journey.
    
* [**Dev Card**](https://docs.opensauced.pizza/features/dev-card/) **-** The dev card has your profile picture, username, the number of pull requests you have created, the number of repositories you contributed to, and a graph icon that describes your activity rate. You can show it off in public to show your open-source contributions.
    

## Building the GitHub stats tool

Now that we know what OpenSauced is and its features, let's use it to build the GitHub stats tool.

OpenSauced has a lot of [APIs](https://api.opensauced.pizza/) that you can use to build a solution for yourself. For the tool that I built, I used the `/v2/histogram/top/stars` and `/v2/histogram/top/forks` APIs.

To understand the API response, you can either use tools like Postman or refer to the OpenSauced API documentation.

The expected response from the `/v2/histogram/top/stars` API is:

```json
[
  {
    "bucket": "2022-08-28 22:04:29.000000",
    "star_count": 4,
    "repo_name": "open-sauced/api"
  }
]
```

The expected response from the `/v2/histogram/top/forks` API is:

```json
[
  {
    "bucket": "2022-08-28 22:04:29.000000",
    "star_count": 4,
    "repo_name": "open-sauced/api"
  }
]
```

This gives us a clear idea of how the response will look when we call the API. The tool we will be building will show the repository name and the star/fork count, so we can omit the bucket and write the logic accordingly.

**Let's see how we wrote the logic for it:**

1. In my `index.html` file, which contains the code for the UI, there are two buttons with the IDs `loadStars` and `loadForks`. These buttons fetch data from the API when clicked, so we added two event listeners:
    
    ```javascript
    document.getElementById('loadStars').addEventListener('click', function() {
        fetchData('https://api.opensauced.pizza/v2/histogram/top/stars', 'starRows');
    });
    
    document.getElementById('loadForks').addEventListener('click', function() {
        fetchData('https://api.opensauced.pizza/v2/histogram/top/forks', 'forkRows');
    });
    ```
    
2. The next step is to fetch the data from the API and display it in the table format that we added in our `index.html` file.
    
    The code shown below will perform these three actions:
    
    * Clears any existing content in the HTML element specified by `elementId` to ensure it displays only new data.
        
    * Slices the data to include only the top 10 items from the response.
        
    * For each item, it constructs a table row (`<tr>`) with two columns (`<td>`): the repository name and its star or fork count, depending on the data type.
        
    
    ```javascript
    function fetchData(url, elementId) {
        fetch(url)
            .then(response => response.json())
            .then(data => {
                const tableBody = document.getElementById(elementId);
                tableBody.innerHTML = ''; // Clear existing data
    
                // Slice the data to display only the first 10 items
                data.slice(0, 10).forEach(item => {
                    const row = `<tr>
                        <td>${item.repo_name}</td>
                        <td>${item.star_count || item.fork_count}</td>
                    </tr>`;
                    tableBody.innerHTML += row;
                });
            })
            .catch(error => {
                console.error('Error fetching data:', error);
            });
    }
    ```
    

With this, you learned how to use OpenSauced APIs to build a GitHub stats tool like mine. If you want to see the frontend code, you can find the entire code [here](https://github.com/Haimantika/github-stats).

## Resources

If you are just getting to know about OpenSauced and Tailwind, these resources will help you get started:

* [OpenSauced documentation](https://docs.opensauced.pizza/)
    
* [OpenSauced API documentation](https://api.opensauced.pizza/)
    
* [OpenSauced community](https://discord.gg/n7vy36zv)
    
* [Tailwind documentation](https://tailwindcss.com/docs/installation)