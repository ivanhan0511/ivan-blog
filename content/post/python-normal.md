---
title: "Python Normal"
date: 2022-05-24T16:50:44+08:00
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

<!--more-->---

## Python __private(self)的轧压

`http://greybeard.iteye.com/blog/1355216`




## Publish your own Python package to PyPI

Register to https://pypi.org and https://test.pypi.org
Copy package structure in kennethreitz/setup.py on Github
Don't forget pip install twine
```shell
python setup.py sdist
twine upload dist/*
Maybe you need ~/.pypirc
```
