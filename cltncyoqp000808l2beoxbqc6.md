---
title: "It is time to build your portfolio website"
datePublished: Mon Mar 11 2024 19:50:29 GMT+0000 (Coordinated Universal Time)
cuid: cltncyoqp000808l2beoxbqc6
slug: it-is-time-to-build-your-portfolio-website
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1710186594546/d81b25b4-14a5-43bc-9de9-a9297449b9a8.png
tags: javascript, html, portfolio, tailwind-css, portfoliowebsite

---

Answer honestly, how many times did you think of building a portfolio and kept delaying? Building and updating a portfolio is one thing that every dev wants to do, but keeps contemplating and nobody knows why.

My guesses are:

* Being very picky with templates
    
* The desire to over engineer it (You cannot deny, we want to make it look fancy)
    
* Deciding which tech stack to use
    

In my opinion, you should have a minimal portfolio and then keep updating it. With the job landscape being competitive, a portfolio becomes a must-have. In this tutorial we will see how to build a simple portfolio like this 👉 [haimantika.dev/](https://haimantika.dev/)

## Prerequisites

* Understanding of HTML, TailwindCSS, JavaScript - to build the portfolio
    
* A [Hashnode account](https://hashnode.com/) - to build the [haimantika.dev/blog](https://haimantika.dev/blog) page that fetches blogs from Hashnode.
    
* A [Vercel account](https://vercel.com/) - to host [Hashnode Headless CMS](https://hashnode.com/headless)
    
* Understanding of [Flowbite](https://flowbite.com/docs/getting-started/introduction/)
    

## Getting Started

1. The first step for me before building any website is to draft an overview of all what it will contain.
    
    This is how I structured it:
    
    ```markdown
    /portfolio
     /home
      - description
      - projects
    /blog
      - list of all blogs
    ```
    
2. The next step is deciding the components, since this is a minimal portfolio all that we needed was:
    
    * A header section
        
    * A footer section
        
    * Card to display the projects
        
3. Let's start building the header, along with the dark-light mode toggle (feel free to use your own SVG)
    
    ```xml
     <header class="sticky top-0 z-10 bg-white dark:bg-black text-black dark:text-white p-4">
            <section class="max-w-4xl mx-auto flex justify-between items-center">
                <nav class="space-x-8 text-m" aria-label="main">
                    <a href="#home" class="hover:opacity-90 text-black dark:text-white">home</a>
                    <a href="https://haimantika.dev/blog" class="hover:opacity-90 text-black dark:text-white">blog</a>
                    <a href="#projects" class="hover:opacity-90 text-black dark:text-white">projects</a>
                </nav>
                <button id="themeToggle" class="flex items-center justify-center p-2 bg-gray-300 dark:bg-gray-700 rounded-full shadow hover:bg-gray-400 dark:hover:bg-gray-600 focus:outline-none focus:ring-2 focus:ring-gray-500 focus:ring-opacity-50 transition duration-200">
                  <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6 text-white" viewBox="0 0 20 20" fill="currentColor">
                      <!-- Half Sun SVG Path -->
                      <path d="M10 2v2m4 2a4 4 0 11-8 0m0 10a4 4 0 108 0m4-2v2m2-6h2M2 10H0m2.929 4.929l1.414-1.414M17.071 5.929l-1.414 1.414M4.929 5.929l-1.414-1.414m12.728 12.728l1.414-1.414"/>
                      <!-- Semi-circle to represent the half sun -->
                      <path d="M15 10a5 5 0 01-10 0h10z" />
                  </svg>
              </button>
              
            </section>
        </header>
    ```
    
4. Now moving on to build the footer, I wanted my footer to include all social links, so this is how it looks:
    
    ```xml
    <footer id="footer" class="mt-auto bg-white dark:bg-black text-black dark:text-white">
            <section class="mx-auto flex max-w-4xl flex-col p-4 sm:flex-row sm:justify-between">
                <nav>
                    <div class="flex justify-center gap-4">
                        <a href="https://twitter.com/HaimantikaM" target="_blank" class="hover:opacity-90 text-black dark:text-white">Twitter</a>
                        <a href="https://newsletter.haimantika.com" target="_blank" class="hover:opacity-90 text-black dark:text-white">Newsletter</a>
                        <a href="https://github.com/Haimantika" target="_blank" class="hover:opacity-90 text-black dark:text-white">GitHub</a>
                        <a href="https://www.linkedin.com/in/haimantika-mitra/" target="_blank" class="hover:opacity-90 text-black dark:text-white">LinkedIn</a>
                    </div>
                </nav>
            </section>
        </footer>
    ```
    
5. Next up is to build cards for the project, for which I will be using the [card component](https://flowbite.com/docs/components/card/) from `Flowbite`. This is how it looks after customising:
    
    ```xml
    <!-- Project 2 -->
                <div class="max-w-sm p-6 my-6 bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-lg shadow">
                    <a href="#">
                        <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">Introduction npm package</h5>
                    </a>
                    <p class="mb-3 font-normal text-gray-700 dark:text-gray-400">
                        Go to your terminal and type `npx hello-haimantika`. This is an npm package that helps you build a package for yourself that you can use to introduce yourself in a cool way.
                    </p>
                    <a href="https://github.com/Haimantika/introduction-npm-package" class="inline-flex items-center px-3 py-2 text-sm font-medium text-center text-white bg-blue-700 rounded-lg hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
                        Read more
                        <svg class="rtl:rotate-180 w-3.5 h-3.5 ms-2" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 10">
                            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M1 5h12m0 0L9 1m4 4L9 9" />
                        </svg>
                    </a>
    ```
    
    ***Note: To make the blog precise, I have mentioned pseudo codes. You can find the entire code*** [***here***](https://github.com/Haimantika/haimantika-headless-portfolio)***.***
    
6. The next step is to build the [blog](https://haimantika.dev/blog) page. For my portfolio, I have used Hashnode Headless' [starter kit](https://github.com/Hashnode/starter-kit).  
    ***Note: Follow the*** [***README***](https://github.com/Hashnode/starter-kit/blob/main/README.md) ***thoroughly to setup your own blog (it literally takes just a few minutes).***
    
7. Once your blog is ready, our final step is to customise the blog and align it with our existing UI. Since my portfolio is a simple dark background, all that I had to do is use Hashnode Headless CMS' dark mode. You can do that from the `tailwind.config.js` file.
    
8. Next up is, adding the footer and header components.  
    To update the header, go to `packages/blog-starter-kit/themes/personal/components/personal-theme-header.tsx` and add the following piece of code:
    
    ```xml
    <header className="p-2 bg-white text-black dark:bg-black dark:text-white">
        <nav className="max-w-xl mx-auto flex justify-between items-center space-x-8">
            <a href="https://haimantika.dev/#home" className="hover:opacity-90">home</a>
            <a href="https://haimantika.dev/blog" className="hover:opacity-90">blog</a>
            <a href="https://haimantika.dev/#projects" className="hover:opacity-90">projects</a>
        </nav>
    ```
    
    To update the footer, go to `packages/blog-starter-kit/themes/personal/components/footer.tsx` and add the following piece of code:
    
    ```xml
    		<footer className="border-t border-neutral-700 dark:border-neutral-700 pt-10 text-sm text-neutral-900 dark:text-neutral-400 bg-white dark:bg-black">
    		<section className="mx-auto flex max-w-4xl flex-col p-4 sm:flex-row sm:justify-between">
    			<nav>
    				<div className="flex justify-center gap-4">
    					<a href="https://twitter.com/HaimantikaM" target="_blank" className="text-neutral-900 dark:text-white hover:opacity-90">Twitter</a>
    					<a href="https://newsletter.haimantika.com" target="_blank" className="text-neutral-900 dark:text-white hover:opacity-90">Newsletter</a>
    					<a href="https://github.com/Haimantika" target="_blank" className="text-neutral-900 dark:text-white hover:opacity-90">GitHub</a>
    					<a href="https://www.linkedin.com/in/haimantika-mitra/" target="_blank" className="text-neutral-900 dark:text-white hover:opacity-90">LinkedIn</a>
    				</div>
    			</nav>
    		</section>
    	</footer>
    ```
    
    And that's a wrap! You have now built a simple portfolio for yourself!
    

## Ending notes

I hope you had fun reading it and will soon be building a portfolio. For hosting my portfolio, I have used [Cloudflare](https://www.cloudflare.com/en-gb/) and the Hashnode Headless blog is running in Vercel. If you need any help with hosting or routing, feel free to drop a comment here or DM me [@HaimantikaM](https://twitter.com/HaimantikaM)