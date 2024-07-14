---
title: "How to build your own GitHub roast app"
seoTitle: "Creating the GitHub Roast App"
seoDescription: "Guide to building a GitHub roast app using OpenAI and GitHub APIs. Fun project with simple HTML, CSS, and JavaScript"
datePublished: Sun Jul 14 2024 13:59:55 GMT+0000 (Coordinated Universal Time)
cuid: clylmhc3h000009ldhqb0bz2j
slug: how-to-build-your-own-github-roast-app
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1720897423209/f7396fca-a8f2-461e-a610-61f013bb9292.jpeg
tags: ai, web-development, opensource, openai

---

A few weeks ago, I built a [GitHub roast app](https://github-roast.up.railway.app/home) that kind of blew up, and today I am finally writing about how I built it.

![A screenshot from a website that roasts GitHub profiles. The text box contains the name "Haimantika" and a blue "Roast!" button below it. The roast text reads, "Well, well, well, look at Haimantika Mitra with 123 repositories and 234 followers on GitHub. So many projects, yet so little time for anyone to notice. It's like opening a fridge full of variety, but all you find is expired mayo and leftovers from last year. Keep pushing those commits, Haimantika, maybe one day your follower count will catch up to your repo count - slowly, but surely!" There's also a link to the roast creator and a blue "Save" button.](https://cdn.hashnode.com/res/hashnode/image/upload/v1720895549058/1d7ef621-b8e6-438d-ac1f-fd4a3294ce11.png align="center")

***But first, how does the application work?***

The flow is simple - you enter your GitHub username, and OpenAI generates a roast based on your `public repositories` and `number of followers`.

The prompt we send to OpenAI to generate the roast👇

```javascript
const messages = [
            { role: "system", content: "You are a witty assistant asked to create a light-hearted roast." },
            { role: "user", content: `Tell me a roast about a GitHub user named ${profileData.name || username} who has ${profileData.public_repos} repositories and ${profileData.followers} followers.` },
        ];
```

My biggest takeaway from this fun weekend project was, that you do not build a fancy UI to have users. *Easy onboarding is the key!*

Here's some screenshots attached as proof:

![A social media post by Haimantika Mitra, titled "Built this GitHub roast application last night, and I think it is brutal!" The post includes a link to the application and mentions it is built using OpenAI API, GitHub API, HTML, CSS, and Node.js. It is also noted to be open-source and open for contributions. The post has multiple reactions, comments, and reposts. An image snippet of the app interface is included below the text.](https://cdn.hashnode.com/res/hashnode/image/upload/v1720895671983/1f3c0013-d716-4a56-b15e-e365cd6af061.png align="center")

![Dashboard view of an application named "Ghroast" on GitHub. The application is owned by "Haimantika." Options visible include "Transfer ownership," "List this application in the Marketplace," and "Revoke all user tokens." The application has 1,031 users.](https://cdn.hashnode.com/res/hashnode/image/upload/v1720895678586/bf76f44b-beb2-44bc-ad26-45dfce72265b.png align="center")

*Note: The GitHub authentication was added 12 hours after I posted this project online, so my guess is, there were at least 500+ more users, since I had to renew my OpenAI twice before this.*

---

## Get started building

If you know basic HTML, CSS, JavaScript and know how to work with APIs, this walk through will be easy to follow.

Before diving right into the code, let's understand the parts we have in this application:

* **Login page**\- You need to login through GitHub, this was added later to prevent the GitHub API from exceeding. You can [read the documentation](https://docs.github.com/en/rest?apiVersion=2022-11-28) for reference.
    
* **Homepage** - A simple UI built with HTML and CSS, where you enter your GitHub username and it generates a roast for you by fetching profile details from the GitHub API and prompting the OpenAI API to generate a roast.
    

A brief architecture diagram of the application:

![A flowchart describing a process: 1. "Login through GitHub".2. Navigate to "Homepage with a textbox to enter your GitHub username".3. Press "Enter".4. Click "Save button".5. GitHub API fetches "profile information such as name, followers, & public repo".6. "The information gathered is now sent to OpenAI as a prompt to generate the roast".](https://cdn.hashnode.com/res/hashnode/image/upload/v1720965224866/9eae05e5-61ca-49a4-a812-c2668d7e3fd9.png align="center")

### Code breakdown

You can probably build a better UI than me, so I'll skip the HTML part. If you need to check it out, I've linked the repository below.

Now, let's dive into how we handle GitHub authentication and generate the roast 👇

### Code to generate the roast and save the roast as image to share on social media:

In this code, we have two functions:

* The `getRoast` function retrieves a roast associated with a GitHub username from a server. It checks if a username is entered, requests the roast, and displays it on the webpage; if unsuccessful, it informs the user of the failure.
    
* The `saveRoast` function allows the user to save this roast as an image by capturing the part of the webpage showing the roast and initiating a download of the image.
    

```javascript
async function getRoast() {
  const username = document.getElementById("username").value;
  if (!username) {
    alert("Please enter a GitHub username.");
    return;
  }

  try {
    const response = await fetch(`/roast/${username}`);
    const data = await response.json();
    document.getElementById("roastOutput").textContent =
      data.roast || "No roast available.";
  } catch (error) {
    console.error("Failed to fetch roast:", error);
    document.getElementById("roastOutput").textContent =
      "Failed to fetch roast. Please try again.";
  }
}

function saveRoast() {
  html2canvas(document.getElementById("roastWrapper")).then(function (canvas) {
    var link = document.createElement("a");
    link.href = canvas.toDataURL("image/png");
    link.download = "roast.png";
    link.click();
  });
}
```

### Code that handles the GitHub login and uses the GitHub and OpenAI API to generate the roast

In this block, we set up a web server that handles user logins through GitHub and generates roasts about GitHub users. It uses Express for server operations and Passport.js for GitHub authentication. Users are redirected to log in via GitHub, and once authenticated, can access a homepage. The server also includes an endpoint that uses the OpenAI API to create roasts based on a user's GitHub profile. It handles errors gracefully and logs server activity.

```javascript
require('dotenv').config();
const express = require('express');
const axios = require('axios');
const passport = require('passport');
const GitHubStrategy = require('passport-github').Strategy;
const session = require('express-session');
const path = require('path');
const OpenAI = require('openai');

const app = express();
app.use(express.json());
app.use(express.static('public'));
app.use(session({
    secret: 'replace_this_with_a_secure_secret',
    resave: false,
    saveUninitialized: true,
}));

// Passport setup
passport.serializeUser((user, done) => done(null, user));
passport.deserializeUser((obj, done) => done(null, obj));

passport.use(new GitHubStrategy({
    clientID: process.env.GITHUB_CLIENT_ID,
    clientSecret: process.env.GITHUB_CLIENT_SECRET,
    callbackURL: "https://github-roast.up.railway.app/auth/github/callback"
}, (accessToken, refreshToken, profile, done) => done(null, profile)));

app.use(passport.initialize());
app.use(passport.session());

// Route to serve the login page
app.get('/', (req, res) => {
    if (req.isAuthenticated()) {
        res.redirect('/home');
    } else {
        res.sendFile(path.join(__dirname, 'public', 'login.html'));
    }
});

// Route to handle GitHub authentication
app.get('/auth/github', passport.authenticate('github'));

// GitHub callback route
app.get('/auth/github/callback', 
    passport.authenticate('github', { failureRedirect: '/' }),
    (req, res) => res.redirect('/home'));

// Protected route to serve the homepage after login
app.get('/home', (req, res) => {
    if (req.isAuthenticated()) {
        res.sendFile(path.join(__dirname, 'public', 'home.html'));
    } else {
        res.redirect('/');
    }
});

const openai = new OpenAI({
    apiKey: process.env.OPENAI_API_KEY_NEW4,
});

// Endpoint to generate a roast
app.get('/roast/:username', async (req, res) => {
    const { username } = req.params;
    try {
        const githubResponse = await axios.get(`https://api.github.com/users/${username}`);
        const profileData = githubResponse.data;

        if (!profileData) return res.status(404).json({ error: "GitHub user not found" });

        const messages = [
            { role: "system", content: "You are a witty assistant asked to create a light-hearted roast." },
            { role: "user", content: `Tell me a roast about a GitHub user named ${profileData.name || username} who has ${profileData.public_repos} repositories and ${profileData.followers} followers.` },
        ];

        const completion = await openai.chat.completions.create({
            messages: messages,
            model: "gpt-3.5-turbo",
        });

        if (completion.choices && completion.choices.length > 0) {
            const roast = completion.choices[0].message.content;
            res.json({ roast });
        } else {
            res.status(500).json({ error: "Failed to generate a roast from AI" });
        }
    } catch (error) {
        console.error('Error:', error);
        res.status(500).json({ error: 'Failed to fetch data or generate roast' });
    }
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server is running on http://localhost:${PORT}`));
```

---

And that's a wrap! You can find the code [on GitHub](https://github.com/Haimantika/GitHub-roast). If you liked it, feel free to star the repo. If you would like to share more cool project ideas, feel free to DM me [@HaimantikaM](https://x.com/HaimantikaM).