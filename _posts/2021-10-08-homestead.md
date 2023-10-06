---
layout: post
title: "Homestead"
date: 2021-10-08T11:29:51+02:00
categories: ["Homestead"]
tags: ["hmst", "hugo", "lke"]
---

I just started a new project on GitHub:
**[hmst](https://github.com/ceesvandegriend/hmst) Homestead**.

This will be my personal website generated with [Hugo](https://gohugo.io/).
The static HTML-pages will run in a container.


# Personal website


```shell
$ cd .../website/
.../website $ hugo server -D
Start building sites …
hugo v0.88.1+extended darwin/amd64 BuildDate=unknown

                   | EN
-------------------+-----
  Pages            | 15
  Paginator pages  |  0
  Non-page files   |  0
  Static files     |  0
  Processed images |  0
  Aliases          |  4
  Sitemaps         |  1
  Cleaned          |  0

Built in 30 ms
Watching for changes in /Users/cees/prj/hmst/website/{archetypes,content,data,layouts,static,themes}
Watching for config changes in /Users/cees/prj/hmst/website/config.yml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

The workflow will be:

* Run hugo on localhost: `http://localhost:1313/`

* edit Markdown pages

* commit changes

* push changes to GitHub

* GitHub actions will build a Docker container and push this to Docker Hub

* GitHub actions will deploy the new image to LKE, Linode Kubernetes Engine.

The website with the updated content should be available almost instantly. 
