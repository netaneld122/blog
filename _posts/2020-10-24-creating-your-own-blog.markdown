---
layout: post
title:  "Step by step - creating your own Tech Blog using GitHub Pages and Jekyll (free & ad-free)"
date:   2020-10-24
---

![](/images/2020-10-25-21-58-25.png)

If you want the TL;DR then skip to the [Step by step](#step-by-step) section.

## Preface

This is the first post in my blog, so I thought it would be nice to walk through the process of creating a simple static blog for you - software developers and researchers out there.

Instead of spending money on boilerplate WordPress and hosting servers, you can use free and more importantly ad-free platforms that are available for everyone.

*There are several tutorials from GitHub and Jekyll themselves, but in my opinion theirs can be a bit overwhelming with many links and contradicting steps, so my main goal is to simplify that by showing the way I configured everything.*

## GitHub Pages

GitHub Pages is a feature of existing GitHub repositories, allowing you to treat the repository as a platform for creating static web pages, it hosts (and optionally generates) your HTML files and displays them from within `github.io`'s subdomains.

## GitHub Workflows

GitHub Workflows is yet another feature you can use in GitHub repositories, allowing you to automate your build, test and deployment process using `.yml` files.

In our case we'll be using it to generate our static pages.

### The github-pages gem

We are going to create a simple workflow to generate our pages, but the remote build system has [specific versions and dependencies][github-pages-versions] that may differ from your local configuration - here enters the `github-pages` library (in Ruby called a "gem").

This library makes sure that the local and remote dependencies will remain consistent and nothing unexpected occurs when deploying your website.

So what we're going to do is depend on the latest `github-pages` gem, and in turn it will depend on the correct dependency gems.

## Jekyll

Jekyll is a Ruby based gem that generates static web pages from markdown files, and optionally HTML/JS/CSS files.

If you need a refresher on how to use the markdown syntax - [this cheat sheet][cheat-sheet] can help you.

Alongside Jekyll, we will also install `bundler` - an enhancement of Ruby's package management capabilities - this is necessary because the github-pages gem relies on it.

## Step by step

1. Install [Ruby+Devkit](https://rubyinstaller.org/downloads/)
1. Install Jekyll and Bundler by running:  
    * `gem install jekyll bundler`
1. Run the following command to create your repository (enter your own repo-name):  
    * `jekyll new <repo-name>`
1. Open `<repo-name>/Gemfile`
    * Remove the line with `gem "jekyll"` in it (github-pages will get the correct dependency)
    * Uncomment the line with `gem "github-pages"` in it
1. Remove `<repo-name>/Gemfile.lock`
1. From within `<repo-name>` run
    * `bundler install`
1. And finally run
    * `bundler exec jekyll serve`
1. Browse `http://127.0.0.1:4000/`

And voilà, you have your own blog.

## Connecting to GitHub

If you haven't already configured your GitHub repository, do the following:

1. Create a new repository (must be public, unless you have a pro account)
1. Under your GitHub user settings go to `Developer settings -> Personal access tokens -> Generate new token` and grant access to `public_repo`
    * Copy the access token to your clipboard
1. Go to the GitHub repository you just created and in the repo settings go to `Secrets` and create a new one secret called `JEKYLL_PAT` with the access token from your clipboard
1. Inside the `<repo-name>` directory run:
    * `git init`
    * `git remote add origin git@github.com:<username>/<repo-name>.git`  
    * `git add .`
    * `git commit -m "Initial commit"`
1. Create a file under `<repo-name>/.github/workflows/github-pages.yml` and use [the content in the github-pages.yml section](#github-pages.yml)
1. And finally push it all using
    * `git push --all`
    * After about 3 minutes the GitHub workflow action will finish running and eventually deploy the final website files to a branch called `gh-pages`
1. Go to the repository settings and configure `gh-pages` as the source for GitHub Pages
1. Browse `https://<username>.github.io/<repo-name>`

Now your blog is accessible to everyone.

### github-pages.yml

{% raw %}
```yml
name: Build and deploy Jekyll site to GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  github-pages:
    runs-on: ubuntu-16.04
    steps:
      - uses: actions/checkout@v2
      - uses: helaili/jekyll-action@2.0.1
        env:
          JEKYLL_PAT: ${{ secrets.JEKYLL_PAT }}
```
{% endraw %}

## Some extras

If you're using VS code as your IDE, then consider the following:
* Add the **Code Spell Checker** extension - this can save you from unnecessary typos as it verifies markdown files
* Add the **Paste Image** extension - to easily embed images from your clipboard (use `WinKey + Shift + S` and then `Ctrl + Alt + V` to paste)
* Create a file called `<repo-name>/.vscode/tasks.json` with the content mentioned below, this will allow you to serve the website locally using the `Ctrl + Shift + B` keys:

### tasks.json
```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Serve",
            "type": "shell",
            "command": "bundle exec jekyll serve",
            "problemMatcher": [],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```
* You can also [configure GitHub Pages to use your own domain](own-domain)


This tutorial came out a little bit tedious, but once you configure everything it is pretty convenient to create content.


[github-pages-versions]: https://pages.github.com/versions/
[cheat-sheet]: https://github.com/mnyrop/nycdh-jekyll/blob/master/docs/markdown-cheatsheet.md
[own-domain]: https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/configuring-a-custom-domain-for-your-github-pages-site