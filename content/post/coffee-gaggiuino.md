---
title: "Gaggiuino For Coffee(Stranded)"
date: 2023-12-05T13:39:32+08:00
categories:
- Coffee
- Gaggiuino
tags:
- coffee
- gaggiuino
- Principle
keywords:
- coffee
- pid
- coffee
- gaggiuino
clearReading: true
#thumbnailImage: //example.com/static/A.png
thumbnailImage: image-1.png
thumbnailImagePosition: bottom
autoThumbnailImage: yes
metaAlignment: center
#coverImage: //example.com/static/B.png
coverImage: image-2.png
coverCaption: "A beautiful image"
coverMeta: out
coverSize: full
comments: false
showTags: true
showPagination: true
showSocial: true
showDate: true
---
If "Uino" is from Ladin, it means wine.

PID temprature controllation can be replaced by Gaggiuino, which is a system based on MCU(STM32)

<!--more-->

{{< toc >}}

## EXPECTATION
---
- No broken to the original box of Milesto EM-18
- No steam for Latte as the principle
- Keep the original 420cc brass boiler, but with a new PID temp detector and controled by Gaggiuino system
- Keep the original single vibrating pump(Dec 06, 2023), but controled by Gaggiuino system
- Replace the time counter with a new LCD
- Add function of pre-infusion




## Defination
---
- PID:
  - p: 比例proportion
  - i: 积分integral
  - d: 差分differential

  PID控制需要以满足闭环控制系统作为前提, 其次PID才是精髓, 即结合当前误差比例控制, 历史误差积分求和, 误差变化微分 三者的权重比例进行调节的控制方法




## PREPARE
---

Read the [doc](https://gaggiuino.github.io/#/?id=home) first

### Hardware
- Milesto EM-18
  - 420cc brass boiler
- PCB modules from Gaggiuino
- Connection lines
- Buy all of them, STM32, PCB, lines, LCD, temperature detector
- Tools like ...


### Software
[Gaggiuino Project](https://github.com/Zer0-bit/gaggiuino) in github

#### Github Gaggiuino
- Read the [doc](https://gaggiuino.github.io/#/?id=home)
- Run the project


#### C/C++
- Known the gramma of C/C++ in Gaggiuino project




## OPERATIONS
---

### Disassemble

### AC Power

### Boiler

### Bump

### Gaggiuino

### Testing

