---
layout: post
title: "jekyll quick start"
description: ""
category: 
tags: []
---

jekyll quick start

### Create a Post
Create posts easily via rake task:

```
$ rake post title="Hello World"
```

### Create a Page
Create pages easily via rake task:

```
$ rake page name="about.md"
```

Create a nested page:

```
$ rake page name="pages/about.md"

```
Create a page with a "pretty" path:

```
$ rake page name="pages/about"
```

this will create the file: ./pages/about/index.html
The rake task automatically creates a page file with properly formatted filename and YAML Front Matter as well as includes the Jekyll Bootstrap "setup" file.
