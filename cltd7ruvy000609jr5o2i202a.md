---
title: "Introduce yourself with an npm package"
datePublished: Mon Mar 04 2024 17:27:31 GMT+0000 (Coordinated Universal Time)
cuid: cltd7ruvy000609jr5o2i202a
slug: introduce-yourself-with-an-npm-package
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1709573183770/e6d06738-314a-4bde-9f30-2faf50527e8c.png
tags: npm

---

Go to your terminal and type: `npx hello-haimantika`

Watch it in action here 👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1709564329137/b5447552-3a77-4e97-bf8c-9c0c5418dd5e.gif align="center")

Looks interesting right? Building it is easy too. Whether you're a seasoned developer or just starting out, this guide will help you craft a unique way to make your introduction stand out.

But, first.

### What is an npm package?

An [npm package](https://docs.npmjs.com/about-packages-and-modules) is a reusable piece of software, a module or library, that can be downloaded from the npm registry using the Node Package Manager (npm). It can include anything from a small utility function to a complete application framework, facilitating code sharing and reuse within the JavaScript ecosystem. Packages are typically installed as dependencies in Node.js projects to extend functionality or add new features.

### Getting Started

**Step 1:** Choose a package name

**Step 2:** Setup your project directory

```javascript
mkdir my-npx-intro
cd my-npx-intro
```

**Step 3:** Initialize your package

```javascript
npm init -y
```

### Creating an executable script

Your NPX command will run a JavaScript file, which acts as the entry point. Inside your project directory, create a file named `index.js`. This file will contain the code executed when your NPX command is invoked. Feel free to customize the content of this file to best introduce yourself.

For your script to be executable through NPX, you must declare it in your `package.json` file. Add the following lines:

```json
"bin": {
  "my-npx-command": "./index.js"
}
```

Replace `"my-npx-command"` with whatever you want your command to be called.

**Making the script executable:**

Before testing, ensure your script file is executable. On Linux, use the command:

```javascript
chmod +x index.js
```

Windows users will need Git installed to run:

```javascript
git update-index --chmod=+x index.js
```

**Testing your command:**

Before publishing, test your command locally by linking your package with npm:

```javascript
npm link
```

Now, you can test it by running `npx <package-name>`. If everything goes as planned, don't forget to unlink your package:

```javascript
npm unlink -g <directory-name>
```

### Publishing your package

To share your command with the world, you'll first need an npm account. After creating one, log in via the terminal:

```javascript
npm login
```

Finally, publish your package with:

```javascript
npm publish
```

### Share your introduction

Congratulations! You've just created and published your own NPX introduction command. It's time to share it with your friends, colleagues, or at meetups (if you are presenting).

You can find the code [here](https://github.com/Haimantika/introduction-npm-package).