---
title: "Create a Personal Blog using Hugo"
author: "Dunstan Vavasour"
date: "2022/06/16"
draft: true
---

# Create a Personal Blog using Hugo

## What is the purpose of this exercise?

This is primarily an exercise in Continuous Integration: their pages will exist in a code repo that they edit on their end user device and which publishes updated pages using a Gitlab CI pipeline

## What technologies will be used?

- Git: the use of branches, staging, commits, remotes
- Markdown: pages are written in standard markdown language, the lingua franca of low level technical documentation
- Hugo: _aka: use a bit of technology you've never seen before_
- Gitlab CI: set up a processing pipeline, and work out how to fix it when things break
- DNS: Add CNAME and TXT records
- Certificates: Using a very simple LetsEncrypt certificate to provide https verification

## What is visible at the end?

- A static website using a vanity domain
- README documentation of how it's made

## What resources are needed?

- Computacenter laptop with admin rights to install necessary software
- Gitlab account (a free account will suffice)
- A domain and somewhere to manage it (this will cost about £10 per head, and will probably be used for multiple )
