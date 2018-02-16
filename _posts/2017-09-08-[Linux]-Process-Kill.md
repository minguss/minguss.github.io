---
layout: post
title:  "프로세스 찾아서 kill하기"
date:   2017-09-08 08:00:00
author: minguss
categories: os linux
tags: linux
---

Linux에서 파일을 복사하거나 move할 때, 프로세스가 동작중이면 해당 작업이 수행이 안된다. (뭐 Windows도 마찬가지다.)  
자동화 하는 환경에서 이런 경우에 대해서는 Process를 Kill을 해야 하는 상황이 발생할 수 있는데, Process를 찾아야 할 것 입니다.  

이럴때에는 프로세스를 찾아서 kill하는 script가 필요합니다.
``` bash
ps -ef | grep <process명> | grep -v grep | awk '{print $2}' | xargs kill -9
```

이렇게 입력을 해주면 해당 프로세스를 kill하게 됩니다.