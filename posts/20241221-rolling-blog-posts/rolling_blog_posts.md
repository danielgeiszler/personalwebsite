Sometimes I stumble across something while reading and I want to be able to write and post it somewhere so that I’ll be able to access it later, but that isn’t meaningful enough to make a blog post about on its own. With this setup, I can just take a note on my phone and automatically update my blog. To do this, I use a combination of Obsidian (a notes app on my phone), Quarto (the publishing system for my personal website), and Github Actions for automatic deployment. The end result is that I can take a note on my phone in markdown format and have it automatically pushed and rendered on my website in my blog.

The basic workflow here is to make changes on your device in the Obsidian app, push those changes to your (private) notes repository, copy the file containing those changes to your website repository, and render them.

You need three things to do this:
1. The Obsidian app with the Github plugin
2. A Github repository to host your Obsidian notes (it can be private so that you can make notes without others seeing them)
3. A github repository with a Quarto project

## Step 1: Setting up Obsidian
1. Download the Obsidian app from your mobile store
2. Inside Obsidian, download the Git Community plugin
3. On Github, create a [Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) linked to your account. This token will need to have read/write privileges for your repository, so the “repo” box needs to be checked when setting up your token. SAVE THIS TOKEN FOR LATER.
4. Configure your Git plugin inside Obsidian to use your username, email, and your new Personal Access Token.
5. Link your notes repository on Github to your Obsidian app.

Now, any time you make changes to your files, you can go to the bottom right hamburger menu > Open command palette > Git: to commit and push your changes to your Git repository. (Note: for these commands to appear, you need to be in read mode rather than write mode, which can be toggled by clicking the book/pencil in the top right corner.)

## Step 2: Setting up your rolling blog post
1. In Obsidian, create a new file, then commit your changes and push the file to your repository. 2. Verify that your file appears in your github repository after pushing. It should be a markdown file with the name {name}.md. I’m using a post title machine_learning_basics to do this, which appears as machine_learning_basics.md in my repository. If it’s there, everything is working!
3. In your Quarto site, create a new blog post. I have mine set up to have my blog posts contained inside individual directories (see my example [here](https://github.com/danielgeiszler/personalwebsite/tree/main/posts). To set up a new blog post, I created the folder 20241219-machine-learning-basics. Inside that folder, create the file index.qmd with the following contents (without the backslah to escape to include command}:

```
---
title: "Machine Learning Basics"
author: "Daniel Geiszler"
date: "2024-12-19"
categories: [machine learning]
---

\{{< include machine_learning_basics.md >}}
```


The first few lines are metadata for the blog post. The following statement populated the rendered blog post with the contents of another markdown file. Eventually, this markdown file will exist and be populated by your post’s content, but for now we can forget about it. However, if you want to preview your site using ``` quarto preview ``` to make sure it’s working, you’ll need to create an empty markdown file with the above file name in this directory.

## Step 3: Set up Github actions to automatically push changes to your website repository
1. Go back to your Obsidian repository and add a repository secret. This prevents your personal access token allowing read/write access from being public and is a must for security purposes. Inside your repository, go to Settings > Secrets and variables, then under Secrets add your personal access token from before as a new repository secret with the name PERSONAL_ACCESS_TOKEN.
2. create a folder called .github/workflows if it doesn’t already exist. You may need to pull these changes to your Obsidian app before pushing any subsequent changes on your device.
3. Create a new yaml file titled something like “sync_blogpost.yml” with the following contents. You’ll need to change the fields marked with comments to match your fields. (Note: The inline comments change from # to // because some are yaml comments and some are javascript comments.)

```
name: Sync Machine Learning Basics File # Change this to the name of your workflow

on:
  push:
    paths:
      - 'machine_learning_basics.md'  # Watch for changes in this specific file in your notes repo

jobs:
  update-file:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout source repo
      uses: actions/checkout@v3

    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '16'

    - name: Fetch SHA and Update File
      uses: actions/github-script@v6
      with:
        github-token: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
        script: |
          const fs = require('fs');
          const path = require('path');
          const filePath = path.join(process.env.GITHUB_WORKSPACE, 'machine_learning_basics.md'); // Change this to the name of your blog post's file
          const content = fs.readFileSync(filePath, 'utf8');
          const encodedContent = Buffer.from(content).toString('base64');

          // Fetch the current SHA of the file to be updated
          const response = await github.rest.repos.getContent({
            owner: 'danielgeiszler', // Change this to your github username
            repo: 'personalwebsite', // Change this to your website's repository
            path: 'posts/20241219-machine-learning-basics/machine_learning_basics.md', // Change this to the path inside your website repository
            ref: 'main'  // Make sure to use the correct branch here
          });

          const sha = response.data.sha;

          // Update the file with the new content and the current SHA
          const updateResponse = await github.rest.repos.createOrUpdateFileContents({
            owner: 'danielgeiszler', // Change this to your github username
            repo: 'personalwebsite', // Change this to your website's repository
            path: 'posts/20241219-machine-learning-basics/machine_learning_basics.md', // Change this to the path inside your website repository
            message: 'Update machine learning basics', // Change this to the message displayed for your action
            content: encodedContent,
            sha: sha,  // Provide the SHA fetched from the previous step
            branch: 'main',  // Make sure this is the correct branch
            committer: {
              name: process.env.GITHUB_ACTOR,
              email: `${process.env.GITHUB_ACTOR}@users.noreply.github.com`
            },
            author: {
              name: process.env.GITHUB_ACTOR,
              email: `${process.env.GITHUB_ACTOR}@users.noreply.github.com`
            }
          });

          console.log("File updated", updateResponse.data);
```

That’s it! Now you can write notes on your phone and have them automatically added to your blog post.













