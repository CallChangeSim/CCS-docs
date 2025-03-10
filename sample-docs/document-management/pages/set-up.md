---
title: How we create a documents site
---

[TOC]

# Setting up a new site to run using mkdocs

It's not that complicated to set up a documentation site. Mkdocs is available via python-pip, so is simple to run locally as well as in a gitlab pipeline.

## Getting started on your local machine

### Create a Python virtual environment

A likely directory structure is:

- `~/`Home directory
    - `mkdocs-directory`
        - `sitename-venv` (don't create this in advance!)
        - `sitename-site`

You may need to install venv

```bash
sudo yum install -y python3.9-venv
```
_(or whatever version you're running)_

Then create the virtual environment

```bash
cd ~/mkdocs-directory
python -m venv sitename-venv
. sitename-venv/bin/activate
```

You can come out of the venv using `deactivate` and re-activate it by sourceing the activate script again. If you're more than a beginner with Python this is all second nature

### Install mkdocs

Go into your virtual environment and then (assuming you have python-pip installed) 

```bash
pip install mkdocs
```

From here you can follow the lines at [https://www.mkdocs.org/getting-started/](https://www.mkdocs.org/getting-started/) to create a blank site and add some basic content.

By running a local server you can see what you're doing as you create pages etc. My initial `mkdocs.yml` looks something like:

```yaml
site_name: 'A Very First Test Site using mkdocs'
site_url: 'https://mkdocs.dvavasour.uk/'
site_dir: 'public'
docs_dir: 'docs'

theme:
  name: 'readthedocs'

markdown_extensions:
    - toc:
        permalink: '#'
    - admonition

```

1. (Note: by omitting a `nav:` definition mkdocs will create a default navigation pane containing all the pages)
1. (Note: we're putting the markdown hierarchy into a directory `docs` to keep things a bit clean)

## Running this in Gitlab pages using a CI/CD pipeline

We pretty quickly want to make these pages public. To do this we set up a Gitlab pipeline that generates the site and pops the pages into a location where they can be served.

We're starting with a noddy script. In due course this wants to at least be broken out into a Makefile. The bit we're interested in is the `pages` task which generates the site content and sets the `public` directory as pipeline artifacts. Through some gitlab magic this becomes the content for gitlab pages (look under settings -> pages in your repo for the URL where it's served).

```yaml
image: python:3.8-buster

before_script:
  - pip install -r requirements.txt

test:
  stage: test
  script:
  - mkdocs build --strict --verbose --site-dir test
  artifacts:
    paths:
    - test
  rules:
    - if: $CI_COMMIT_REF_NAME != $CI_DEFAULT_BRANCH

pages:
  stage: deploy
  script:
  - mkdocs build --strict --verbose
  artifacts:
    paths:
    - public
  rules:
    - if: $CI_COMMIT_REF_NAME == $CI_DEFAULT_BRANCH
```
The requirements file is simply:

```
# Documentation static site generator & deployment tool
mkdocs>=1.1.2
```

Finally, we add a .gitignore file where we exclude the `public` directory from git, so that when we run `mkdocs` on our local instance we don't commit and push the output. We also add the default `site` directory that would be generated if `site-dir` weren't set in `mkdocs.yml`

## Changes to render Mermaid blocks

In order to render Mermaid blocks we make the following changes:

In mkdocs.yml we add the following code. Note, the `extra_javascript` statement results in every page loading a megabyte into the browser in order to render a mermaid block. _It should be possible to use front matter in the YAML block at the top of pages to trigger the addition of this content_

```yaml
plugins:
  - mermaid2

extra_javascript:
    - https://unpkg.com/mermaid/dist/mermaid.min.js
```
In `requirements.txt` we add the mermaid plugin, which is loaded into the runner by python-pip.

```
# Documentation static site generator & deployment tool
mkdocs>=1.1.2
mkdocs-mermaid2-plugin
```