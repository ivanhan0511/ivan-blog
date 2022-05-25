---
title: "MySQL View"
date: 2022-05-25T15:48:48+08:00
categories:
- DB
- MySQL
tags:
- MySQL
- SQLView
keywords:
- mysql
- view
clearReading: true
thumbnailImage: //d1u9biwaxjngwg.cloudfront.net/cover-image-showcase/city-750.jpg
thumbnailImagePosition: bottom
autoThumbnailImage: yes
metaAlignment: center
coverImage: //d1u9biwaxjngwg.cloudfront.net/cover-image-showcase/city.jpg
coverCaption: "A beautiful image"
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
---

Create a SQLView to make query fast
<!--more-->

{{< toc >}}





## CHAPTER ONE

List some prerequisites:

{{< alert info >}}
Alert info
{{< /alert >}}

{{< alert success >}}
Alert success
{{< /alert >}}

{{< alert warning >}}
Alert warning
{{< /alert >}}

{{< alert danger >}}
Alert danger
{{< /alert >}}

{{< alert no-icon >}}
Alert no-icon  
So far, I don't know where to be used
{{< /alert >}}

{{< alert danger no-icon >}}
Alert danger no-icon
{{< /alert >}}




## CHAPTER TWO

### Some Content

Maybe I need HLD(High Level Design) after work-flow analysis

And then I need LLD(Low Level Design)

API file is important

- Path parameter: `/api/express/addorder/`
- Operation: `POST`
- Body:

  | Field            | Type      | Nullable | Remark                           |
  | :-------------- | :-------- | :--- | :----------------------------- |
  | id_list     | List[Int] | No   | ID list                     |
  | info_id | Int       | No   | Info ID |
  
- Response

  | Field | Type    | Remark                                                        |
  | ---- | ------- | :---------------------------------------------------------- |
  | code | int     | 0: True, the others is False |
  | msg  | string  | Response message                                                |
  | data | MyData | Response data                                                |
  
- MyData structure

  | Field                | Type     | Remark                                                         |
  | ------------------- | -------- | :----------------------------------------------------------- |
  | some_info        | String   |            |
  | some_status      | Int      |  |
  
- Response example

  ```json
  {
      "code":0,
      "data":{
          "some_info":"xxx",
          "some_status":1
      },
      "msg":"Submit Success"
  }
  ```
  {{< hl-text yellow >}}
  *Please pay attention: xxx!!!*
  {{< /hl-text >}}


### Image Gallery
Image example

{{< image classes="fancybox right clear" src="20220426_banner_example.jpg" thumbnail="http://example.com/static/B.png" group="group:travel" thumbnail-width="750px" thumbnail-height="321px" title="One Piece" >}}




## CHAPTER THREE

### Code

Example:

{{< tabbed-codeblock Gist >}}
    <!-- tab python -->
def say_hello:
    for _ in 'hello':
        print(i)
    <!-- endtab -->
    <!-- tab python -->
import datetime


def print_time:
    print(datetime.datetime.now())
    <!-- endtab -->
{{< /tabbed-codeblock >}}

