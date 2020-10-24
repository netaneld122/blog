---
layout: post
title:  "Step by step - creating your own Tech Blog using GitHub Pages and Jekyll (free & ad-free)"
date:   2020-10-24
---

If you don't want to read the bla-bla skip to the [Step by step](#step-by-step) section.

## Preface

This is the first post in my new blog, so I thought it would be suitable to walk through the process of creating a simple static blog website for you - software developers and researchers out there.

Instead of spending money on boilerplate WordPress and hosting servers, you can use free and more importantly ad-free platforms that are available for everyone.

*There are several tutorials from GitHub and Jekyll themselves, but in my opinion they can be a bit overwhelming with configuration steps and possibilities, so my main goal is to simplify that by showing **my** way of achieving the goal.*

## GitHub Pages

GitHub Pages is a builtin feature inside existing GitHub repositories, allowing you to treat the repository as a platform for creating static web pages, it hosts (and optionally generates) your HTML files and displays them from within `github.io`'s subdomains.

## GitHub Workflows

GitHub Workflows is yet another builtin feature inside existing GitHub repositories, allowing you to automate your build, test and deployment process using `.yml` files.

In our case we'll be using it to generate our static pages.

### The github-pages gem

We are going to create a simple workflow to generate our pages, but the remote build system has [specific versions and dependencies][github-pages-versions] that may differ from your local configuration - here enters the `github-pages` library (in Ruby called a "gem").

This library makes sure that the local and remote dependencies will remain consistent and nothing unexpected occurs when deploying your website.

## Jekyll

Jekyll is a Ruby based gem that generates static web pages from markdown files, and optionally HTML/JS/CSS files.

Alongside Jekyll, we will also install `bundler` - an enhancement of Ruby's package management capabilities - this is necessary because the github-pages gem relies on it.

## Step by step

1. Install [Ruby+Devkit](https://rubyinstaller.org/downloads/)
1. Install Jekyll and Bundler by running:  
    `gem install jekyll bundler`
1. Run the following command to create your repository (enter your own name):  
    `jekyll new <repo-name>`
1. Open `<repo-name>/Gemfile`
    * Remove the line with `gem "jekyll"` in it (github-pages will get the correct dependency)
    * Uncomment the line with `gem "github-pages"` in it
1. Remove `<repo-name>/Gemfile.lock`
1. Run `bundler install`
1. Run `bundler exec jekyll serve`
1. Browse `http://127.0.0.1:4000/`

And voilà, you have your own blog.

## Connecting to GitHub

If you haven't already configured your GitHub repository, do the following:

1. Create a new repository (must be public, unless you have a pro account)
1. Go to settings


[github-pages-versions]: https://pages.github.com/versions/