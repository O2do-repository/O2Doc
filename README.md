# O2Doc

This sites uses [Just the Docs](https://just-the-docs.github.io/just-the-docs/) and Github Pages.  

It is deployed at: [https://o2do-repository.github.io/O2Doc/](https://o2do-repository.github.io/O2Doc/)

### Minimal contribution

Just clone the repo locally, contribute in a branch dedicated to your change, and submit the pull request. 

### Local installation

If you want to visualize the final rendering before you publish your change: 
1. Make sure you have [Jekyll](https://jekyllrb.com) requirements installed on your machine. [See here](https://jekyllrb.com/docs/installation/#requirements). 
1. Clone the repo in a local folder and `cd` into it. 
1. Install jekyll: `gem install jekyll bundler`
1. Install Just-the-doc dependencies: `bundle install`
1. Compile and run the website locally: `bundle exec jekyll serve`.

### Github Codespace

You can also use Github codespace.  Jekyll requirements are automatically met, but if you start a new code space, you first need to proceed with local installation as described above. 

## Understand Just-the-docs basics

Ideally you should get familiar with [Just-the-docs](https://just-the-docs.com) and read its documentation. 

### Structure of the project

```py
index.md # homepage
docs/
  # project content
```

All Markdown fils in `docs` will create new pages on the site, no matter how they are structured into folders or not. 

Files need to start with the following headers at least: 
```
---
title: My New Page 
nav_order: 2
parent: My Parent Page
last_modified_date: 26-02-2025
---

The content of the file... 
```

Only the `title` is mandatory: it defines the name of the page and how it will appear in the navigation pane on the left. 

`nav_order` will setup the ordering on the navigation pane. 

Pages may have child pages, up to infinite levels.  In this case, from the child page, you need to refer to the `parent` page.  

For all the rest, please refer to [Just-the-docs](https://just-the-docs.com). 