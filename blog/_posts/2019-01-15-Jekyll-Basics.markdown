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
# Example, this time mapping port 4000 in the container to 5000 on localhost
docker container run -idt --name "Jekyll" -v C:\_Jekyll_Local\:/jekyll -p 5000:4000 jekyll/jekyll bash
```
## Create new site

```powershell
jekyll new myblog
```

## Host site at http://localhost:4000 (or custom port, if specified)

```powershell
cd myblog
bundle exec jekyll serve
```

**NOTE** - The additional switch ```--host 0.0.0.0```, is required when hosting a site from a Docker container in order to make the URL available on the local machine


```powershell
bundle exec jekyll serve --host 0.0.0.0
```
## Stop server

CTRL C

## Sources

- https://jekyllrb.com
- https://davemateer.com/2018/01/25/Jekyll-and-Docker
- https://tonyho.net/jekyll-docker-windows-and-0-0-0-0/