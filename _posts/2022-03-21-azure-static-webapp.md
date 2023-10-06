---
layout: post
title: "Azure Static Webapp"
date: 2022-03-21T10:30:00+01:00
categories: ["Azure"]
tags: ["az", "snfl"]
---

It is quite easy to setup a static website in
[Microsoft Azure](https://portal.azure.com) with
[Visual Studio Code](https://code.visualstudio.com), if the Azure extentions are
installed.

## Create static website

First, create a repo in [Github](https://www.github.com) and commit a basic
static website. Open this project in `vscode`.

![vscode 01](/assets/images/01-vscode.png)

## Create Azure Static Webapp

Open the Azure extentions in `vscode` and go to `static web apps`.

![vscode 02](/assets/images/02-vscode.png)

Select the `+` to add a new static web app.

Fill in the following values:

- Name: swfl

- Region: West Europe

- Project structure: Other / Custom

- Location code: /src/site

- Location build output: \[Blank\]

`vscode` will do a commit to `github` and create a `github action`.

![github 03](/assets/images/03-github.png)

After the Github action is run, a new static webapp is running in Azure.

## View website

![vscode 04](/assets/images/04-vscode.png)

The name is randomly generated, but the site is accessible on the internet.
The URL is: [https://green-meadow-021d61a03.1.azurestaticapps.net](https://green-meadow-021d61a03.1.azurestaticapps.net).

Now, every time a commit to the Github repo is done, the Github action runs and
upload the new version of the website to Azure.

The advantage of running a website as a Azure Static Webapp:

- HTTPS certificate is handled by Azure.

- The [free tier](https://green-meadow-021d61a03.1.azurestaticapps.net)
  costs nothing, has 100GB bandwidth and 0.25GB storage.

- Of course, it is possible to add a custom domain (hostname) to the site.
