# Deploy a blog with Github Pages and Hugo


[TOC]

## REPOSITORY ON GITHUB

Create a normal repository, like "ivan-blog" as example

Create a respository, must with `<username>.github.io`




## DEPLOYMENT WITH HUGO

```shell
git clone git@github.com:ivanhan0511/ivan-blog.git

# Create local Hugo site
cd ivan-blog
hugo new site . --force

# Visit https://themes.gohugo.io/ and choose a theme and add it as a submodule into the `theme` folder
git submodule add git@github.com:kakawait/hugo-tranquilpeak-theme.git themes/hugo-tranquilpeak-theme

# Copy and use `config.toml` from the theme
cp themes/tranquilpeak/exampleSite/config.toml .

# Create your new post
hugo new post/testpage.md

# Locoal testing
hugo server -D
```
```shell
git submodule add -b master git@github.com:ivanhan0511/ivanhan0511.github.io.git public
vi deploy.sh
```

```shell
#!/bin/sh

# If a command fails then the deploy stops
set -e

printf "\033[0;32mDeploying updates to GitHub...\033[0m\n"

# Create commit message
msg="rebuilding site $(date)"
if [ -n "$*" ]; then
	msg="$*"
fi

# Build the project.
echo ""
echo ""
echo "Committing changes to $(pwd)"
hugo -D

# Go To Public folder
cd public

# Add 'public' (Github Pages repo) changes to git and commit/push.
echo ""
echo ""
echo "Committing changes to $(pwd)"
git add .
git commit -m "$msg"
git push origin main

# Add this repos changes to git and commit/push. First 'cd' out of public
cd ..
echo ""
echo ""
echo "Committing changes to $(pwd)"
git add .
git commit -m "$msg"
git push origin master
```




## CONFIGURATIONS

### Local settings

```shell
vi config.toml
```


### Configure Disqus

To enable discuss under the content, to register an account on Disqus website, 
   and get a "Short Name".  
Set the `disqusShortname` in the `config.toml`



## WRITING POSTS

### Front matter

```shell
vi archetypes/post.md
```

```md
---
title: "EXAMPLE TITLE"
date: {{ .Date }}
categories:
- Coffee
- Grinder
tags:
- Mazzer
keywords:
- coffee
- grinder
clearReading: true
#thumbnailImage: //cdn.zhzhiyu.com/uploads/photos/B.png
thumbnailImage: image-1.png
thumbnailImagePosition: bottom
autoThumbnailImage: yes
metaAlignment: center
#coverImage: //cdn.zhzhiyu.com/uploads/B.png
coverImage: image-2.png
coverCaption: "A beautiful sunrise"
coverMeta: out
coverSize: full
coverImage: image-2.png
gallery:
- image-3.jpg "New York"
- image-4.png "Paris"
- http://i.imgur.com/o9r19kD.jpg "Dubai"
- https://example.com/original.jpg https://example.com/thumbnail.jpg "Sidney"
comments: false
showTags: true
showPagination: true
showSocial: true
showDate: true
summary: "This is a custom summary and does *not* appear in the post."
---

<!--more-->
```


### Create content with Hugo

```shell
cd /path/to/project
hugo new post/hello-world.md
vi content/post/hello-world.md
```




## QUOTES

Refers to these articles:

- [A helper](https://youngkin.github.io/post/createafreeblogsite/)
- [Home page of this theme](https://tranquilpeak.kakawait.com/) for display
- [User doc](https://github.com/kakawait/hugo-tranquilpeak-theme/blob/master/docs/user.md) for learning and configuration
- When writing content, refers to all the .md files in 
  `/path/to/project/themes/hugo-tranquilpeak-theme/exampleSite/content/posts/`

