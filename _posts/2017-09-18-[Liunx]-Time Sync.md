---
layout: post
title:  "Time-Sync"
date:   2017-09-18 08:00:00
author: minguss
categories: devlog
tags: web
---

`폐쇄망인데, Time Server가 없다.`

이런 경우 대상이 되는 서버를 하나 지정한 뒤 해당 서버와 시간을 동기화 하는 방법이 있겠습니다.

NTP(Network Time Protocol)을 이용하여 시간을 동기화 하는 방법이 있지만 기본적으로 ntp 서버를 어떻게 외부 시간과 동기화 할 것인지 여러가지 고민해야할 요소가 많기 때문에 위에서 제시한 바와 같이 대상 서버 하나를 정해서 시간 동기화를 하는 방법을 생각했습니다.  

먼저 동기화를 하는 방법은 아래와 같은 명령어로 동작합니다.

``` bash
/usr/bin/rdate -s time.bora.net && sbin/clock -w
```

해당하는 명령어를 스크립트로 만들어서 사용하거나 Crontab을 이용하여 주기적으로 동기화 하는 방법을 사용하면 되겠습니다.  
주기는 경우에 따라 다르겠지만 저는 1시간에 한번 동기화작업을 걸어두었습니다.
