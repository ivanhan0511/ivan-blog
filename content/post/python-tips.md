---
title: "Python Normal"
date: 2022-05-24T16:50:44+08:00
categories:
- Python
tags:
- Python
keywords:
- python
comments: false
showTags: true
showPagination: true
showSocial: true
showDate: true
---

This is a custom summary and does *not* appear in the post.
<!--more-->---

{{< toc >}}

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
