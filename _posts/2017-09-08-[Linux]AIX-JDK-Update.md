---
layout: post
title:  "JDK Update On AIX Machine"
date:   2017-09-08 08:00:00
author: minguss
categories: devlog
tags: linux
---

Redhat환경과 AIX환경에서 똑같은 웹개발을 하고 있는 프로젝트를 진행하던 도중 Linux환경에서는 잘 되는 기능이 AIX에서는 Error가 발생하여 관련된 Reference를 찾아보고 있었다.  
`JDK8`을 사용하고 있는데, Oracle에서 배포하는 JDK버전과 AIX에서 제공하는 버전의 세부 버전 차이가 발생하는 것들을 확인하게 되었다.  
그리고 해당하는 Error로그로는 JDK Internal Error로 확인이 되어 JDK8의 세부버전의 차이가 영향을 미친 것이라 판단하여 AIX JDK를 업데이트 하기로 결정했다.


Windiws나 Linux같은 경우 GUI로 설치하거나 압축파일을 풀고 Path를 지정하면 되는데, AIX같은 경우에는 IBM사에서 sdk같은 파일로 제공됩니다.

따라서 IBM은 조금 다른 방식으로 설치를 해야하는데 이번에 설치하면서 수행했던 내용을 로그로 남겨봅니다.

먼저 IBM 공식홈페이지의 Guidance에는 다음과 같은 방법들을 제시합니다.  
1. Command line installation
2. Follow these instructions while using the SMIT utility  

저는 시간도 급하고 해서 smit utility를 사용했는데, Command Line installation도 많이 사용하는 것 같습니다.

자세한 내용은 http://www-01.ibm.com/support/docview.wss?uid=isg3T1022693 URL을 참고하면 좋겠습니다.

먼저 설치 할 jdk버전을 다운 받습니다. 대략 2개 파일을 받아야 할 것입니다.  
jdk 파일과 jre파일을 받을텐데요 이 파일을 받고 적당한 위치에 압축을 풀어 넣습니다.

그리고 해당하는 디렉토리로 이동해서 아래와 같이 명령어를 입력합니다.

``` ksh
smittty install_all
```

그렇다면 SMIT Utility가 실행이 될텐데 첫번째 메뉴에서는 `Option:[] INPUT device / directory for software` 이라는 항목이 나올 겁니다. directory를 지정하는 부분인데, 이미 현재 디렉토리가 설치파일이 있는 위치라면 `./`를 입력합니다. 이후에 `Enter`를 누르게 되면 두번째 메뉴가 나오게 됩니다.

여기에는 `Option: [] SOFTWARE to install` 항목에 내용을 채워줘야 하는데 먼저 `F4`버튼을 눌러서 해당하는 디렉토리에 설치 가능 파일들이 무엇들이 있는지 확인해봅니다. 설치 목록 파일들이 있다면 취소 하고 내용에 `*`를 입력해 줍니다. 그리고 `Option: ACCEPT new license agreements?` 옵션에서 `Tab`키를 눌러서 Accept한 뒤 `Enter`버튼을 눌러서 설치를 진행합니다.

이후에 `/usr/java8_64`가 생성된 것을 확인하고 `/usr/java8_64/bin/java -version`을 입력하여 설치를 확인합니다.

----
2018-03-12 추가 포스팅
Smitty을 이용한 Java 설치 이외에 다른 방법이 있는 점을 확인했습니다.  
방법은 **installp -a -Y -d '경로' '파일명'**으로 설치하는 방법인데요.  
-a (-u)는 설치(삭제)옵션이며, -Y는 설치 동의 옵션입니다. -d는 설치 파일 디렉토리 정의 옵션입니다.  
``` bash
su -
installp -a -Y -d '/tmp/java7' Java7_64.sdk
```
위와 같은 command로 바로 설치할 수 있겠습니다.