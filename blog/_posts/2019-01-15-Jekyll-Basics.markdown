---
layout: post
title:  "Basic Jekyll Intro"
date:   2019-01-15 12:42:30 -0501
categories: blog
author: Stavros
---
<script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/clipboard@1/dist/clipboard.min.js"></script>

{% if page.content contains "code" %}
<script>
<!-- clipboard.js code -->
</script>
{% endif %}

<script>
    // get all <code> elements
var allCodeBlocksElements = $( "code" );

allCodeBlocksElements.each(function(i) {
 	// add different id for each code block

	// target	
  var currentId = "codeblock" + (i + 1);
  $(this).attr('id', currentId);
     
  //trigger
  var clipButton = '<button class="btn" data-clipboard-target="#' + currentId + '"><img src="https://clipboardjs.com/assets/images/clippy.svg" width="13" alt="Copy to clipboard"></button>';
     $(this).after(clipButton);
  });
 
  new Clipboard('.btn');
</script>
Jekyll is a static website bulder that allows you to build blogs and other sorts of static sites, primarily using markdown.

Jekyll can be installed locally or in a docker container and you can easily host your site using Github Pages.

## Resources

- [Jekyll site](https://jekyllrb.com)
- [Themes](https://rubygems.org/)
- [Step by Step tutorial](https://jekyllrb.com/docs/step-by-step/01-setup/)

## Local Installation

[Instructions](https://jekyllrb.com/docs/)

```powershell
# On system with Ruby installed
gem install jekyll bundler
```
{: #code-example-1}

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

## Additional resources

- [Jekyll Homepage](https://jekyllrb.com)
- [Step by Step tutorial](https://jekyllrb.com/docs/step-by-step/01-setup/)
- [Jekyll and Docker](Jekyll-and-Docker)
- [Docker, Windows, and 0.0.0.0](https://tonyho.net/jekyll-docker-windows-and-0-0-0-0/)
- [Jekyll Assets Documentation](https://github.com/envygeeks/jekyll-assets/blob/master/README.md)
- [Adding a code snippet copy button](https://stackoverflow.com/questions/48078199/jekyll-code-snippet-copy-to-clipboard-button)
- [Introduction to Jekyll Assets](http://ixti.net/software/2012/12/30/unleash-mr-hyde-introduction-of-jekyll-assets.html)