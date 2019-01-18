---
layout: post
title:  "Basic Jekyll Intro"
date:   2019-01-15 12:42:30 -0501
categories: blog
author: Stavros
---
Jekyll is a static website bulder that allows you to build blogs and other sorts of static sites, primarily using markdown.

Jekyll can be installed locally or in a docker container and you can easily host your site using Github Pages.

## Resources

- [Jekyll site](https://jekyllrb.com)
- [Themes](https://rubygems.org/)

## Local Installation

[Instructions](https://jekyllrb.com/docs/)

```powershell
# On system with Ruby installed
gem install jekyll bundler
```

## Jekyll in a Docker container

```powershell
# Example, this time mapping port 4000 in the container to 4001 on localhost
docker container run -idt --name "Jekyll" -v C:\_Jekyll_Local\:\Jekyll -p 4001:4000 jekyll/jekyll bash
```
## Create new site

```powershell
jekyll new myblog
```

## Build site and make it avalable on local server at 

### URL = http://localhost:4000

```powershell
cd myblog
bundle exec jekyll serve
```

## Stop server

CTRL C

## Additional Resources

https://davemateer.com/2018/01/25/Jekyll-and-Docker