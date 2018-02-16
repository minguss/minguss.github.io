---
layout: post
title:  "Multiple-Gateway"
date:   2017-07-15 08:00:00
author: minguss
categories: os network
tags: network
---

서로 다른 망에 다중 연결해야 하는 경우가 발생할 수 있다.  
하지만 이런경우 Default Gateway에서 문제가 발생할 수 있다. Default로 설정된 대역대만 ping이 오가게되고 추가된 네트워크는 연결이 안될 수 있다. 따라서 이런 경우 별도로 설정을 해줘야 한다.

## Windows 환경

먼저 Route Table의 상태를 print 해본다.  
cmd화면에서 다음과 같이 명령어를 입력한다.
``` bash
route print
```
'route print'명령어를 사용하여 I/F목록을 확인할 수 있다.  
제일 왼쪽에 I/F 번호가 나와있는데, 추가할 I/F카드의 번호를 파악해 둔다.  
그리고 활성경로에는 네트워크 대상과 게이트웨이가 나와 있는데 아마 게이트웨이는 하나로 설정되어 있을 것이다.  

추가할 IP대역대와 Gateway 정보를 확인한 뒤 아래의 명령어를 Typing한다.

1. _192.168.0.0_ mask _255.255.255.255_ : __192.168.0.1 ~ 192.168.0.255__
1. 192.168.0.1 : B노드의 Gateway
1. if 22 : routing table의 인터페이스 목록 중, B노드가 연결된 랜카드 번호
1. -p : 영구적으로 기록(Reboot후에도 Routing table 유지)
``` bash
route add 192.168.0.0 mask 255.255.255.0 192.168.0.1 if 22 -p
```


## Linux 환경
먼저 Route Table의 상태를 print 해본다.  
Terminal화면에서 다음과 같이 명령어를 입력한다.
``` bash
route -n
```

Windows환경과 동일하게 i/f 정보를 확인하고 사용할 IP대역, Gateway정보를 숙지한 후 아래와 같이 Command를 입력한다.

``` bash
//추가
route add -net 192.168.0.0 netmask 255.255.255.0 dev eth2
//삭제
route del -net 192.168.0.0 dev eth2
```
