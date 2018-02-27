---
layout: post
title:  "[Oracle]ORA-27102: Out of memory"
date:   2018-02-18 23:09:59
author: minguss
categories: devlog
tags: dbms
---

>ORA-27102: out of memory Linux-x86_64 Error: 12: Cannot allocate memory on startup.

Oracle 설치 중 oracle init.ora 파라미터를 "lock_sga=true"로 설정 하면 인스턴스가 시작되지 않고 ORA-27102 에러가 발생합니다.  
이것은 "ulimit -l"매개 변수가 잠겨 지도록 요청 된 메모리 크기 (sga 크기)를 허용하도록 설정되지 않아 발생하는 에러입니다.  

``` bash
# ulimit -a
core file size (blocks, -c) 0
data seg size (kbytes, -d) unlimited
scheduling priority (-e) 20
file size (blocks, -f) unlimited
pending signals (-i) 16382
max locked memory (kbytes, -l) 64
max memory size (kbytes, -m) unlimited
open files (-n) 1024
pipe size (512 bytes, -p) 8
POSIX message queues (bytes, -q) 819200
real-time priority (-r) 0
stack size (kbytes, -s) 8192
cpu time (seconds, -t) unlimited
max user processes (-u) unlimited
virtual memory (kbytes, -v) unlimited
file locks (-x) unlimited
```  

Oracle Instance를 시작하기 전에 "ulimit -l unlimited"를 수행해야 겠습니다.

``` bash
ulimit -l unlimited
```
