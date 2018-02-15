---
layout: post
title:  "Pinpoint"
subtitle:   "Pinpoint"
categories: devlog
tags: performance
---


사용하고 있는 APM의 기능이 제한되어 Application 단에 모니터링이 불가하여 다른 APM도구를 알아보던 중 Naver에서 오픈소스 프로젝트로 개발중인 `Pinpoint`를 알게 되어 설치를 하게 되었습니다.
필요했던 부분이 WAS와 DB모니터링인데, Pinpoint에서 모두 지원이 가능하여 이 도구를 선택하게 되었습니다.

Pinpoint + netdata조합으로 우선 사용하다가 더 좋은 대안이 나타나면 또 변경해 볼 생각입니다.

먼저 Pinpoint의 Requirement를 보고 조금 당황했습니다.
일단 JDK를 3개나 설치해야 합니다.  
오늘 날짜가 2017년 9월 8일인데, 곧 JDK9이 나온다고 하면 JDK버전을 나중에는 4개나 설치 해야 하는 상황이 발생할 수 있을 것 같습니다.

저는 아래 Git을 참고했습니다.
https://github.com/naver/pinpoint

설치는 사실 이해하기가 어려웠습니다. 항목도 많고 복잡했기에 Quick Installation Guide를 찾아서 설치했습니다.
https://github.com/naver/pinpoint/blob/master/quickstart/README.md

먼저 Requirement를 확인해봅니다.

Requirements
---
In order to build Pinpoint, the following requirements must be met:

>JDK 6 installed (jdk1.6.0_45 recommended)
>JDK 7 installed (jdk1.7.0_80 recommended)
>JDK 8 installed
>JAVA_HOME environment variable set to JDK 7+ home directory.
>JAVA_6_HOME environment variable set to JDK 6 home directory.
>JAVA_7_HOME environment variable set to JDK 7 home directory.
>JAVA_8_HOME environment variable set to JDK 8 home directory.
>QuickStart supports Linux, OSX, and Windows.

저는 Linux(CentOS7)환경에 설치했습니다. Agent는 어차피 JVM상에서 확인할 것 이므로 환경의 제약 사항은 없을 것 같습니다.

먼저 JDK6와 JDK7, JDK8을 다운로드 받고 설치합니다.

그리고 환경변수에 각각 등록합니다.
``` bash
# User specific environment and startup programs
JAVA_HOME=/ide/java/jdk1.7.0_80
PINPOINT_PATH=/ide/servers/pinpoint

PATH=$PATH:$HOME/.local/bin:$HOME/bin:$JAVA_HOME/bin:$PINPOINT_PATH

export JAVA_HOME=/ide/java/jdk1.7.0_131
export JAVA_6_HOME=/ide/java/jdk1.6.0_45
export JAVA_7_HOME=/ide/java/jdk1.7.0_80
export JAVA_8_HOME=/ide/java/jdk1.8.0_131
```

등록한 후 `source ~/.bash_profile`을 입력하여 환경변수를 바로 적용합니다.

그리고 Maven 빌드를 해야합니다. Git Checkout으로 소스파일을 받거나 소스 전체를 받아서 압축을 해제한 후 빌드합니다.
pinpoint 폴더로 이동하여 
```
./mvnw clean install -Dmaven.test.skip=true
```
위 명령어를 입력하여 빌드를 수행합니다.

> Tip) 처음에 설치했을 때, annotation error가 발생한 경우가 있었는데, `JAVA_HOME`을 설정하지 않은 경우 발생하는 에러였습니다. JAVA_HOME뿐만 아니고 JDK6,7,8 모두 설정해야 합니다.

다음으로 hbase를 설치해야하는데, apache 홈페이지에서 다운로드 받으면 됩니다.`Hbase`는 나중에 따로 포스팅을 하겠지만 간단하게 하둡 플랫폼을 위한 공개 비관계형 분산 데이터 베이스 입니다. 아마도 네이버와 같이 거대한 시스템을 운영하기 위해서 많은량의 시스템을 운영하고 있을텐데, 그것들을 모니터링하기 위해서 빅 데이터 스토어를 운영하는 것 같습니다.

먼저 `Hbase`를 설치하기 위해서 [Apache download site](http://apache.mirror.cdnetworks.com/hbase/)로 이동합니다.

2017년 9월 11일 기준으로 나는 1.2.6버전을 설치했다. 호환 가능한 버전들을 참고하던 중 1.2.6이 최선일 것 같아서 설치했습니다.  
호환성 매핑표를 찾아보고 설치를 진행하면 좋겠습니다.

tar파일로 되어 있는 파일을 다운로드 받고 압축을 풉니다. 그리고 `pinpoint/quickstart/`로 이동한 뒤 아래와 같은 Process를 따라 설정합니다.

``` bash
cd pinpoint/quickstart
mkdir hbase
cd hbase
cp -R ../../hbase-1.2.6 .
mv hbase-1.2.6 hbase
```

폴더를 모두 옮겼다면 다음에는 hbase를 시작하고 table 초기화 작업을 해야합니다. 아래와 같은 명령어를 입력하세요.
``` bash
quickstart/bin/start-hbase.sh
quickstart/bin/init-hbase.sh
```

다음은 hbase와 testapp, web화면을 띄울 것 입니다. quickstart 폴더로 이동해서 아래 명령어를 순서대로 입력하세요.
```
./start-collector.sh
./start-testapp.sh
./start-web.sh
```

상태 확인은 아래 URL로 확인이 가능합니다.
- Web UI - http://localhost:28080
- TestApp - http://localhost:28081

중지하는법은 다음과 같습니다.
- Web UI - Run quickstart/bin/stop-web.sh
- TestApp - Run quickstart/bin/stop-testapp.sh
- Collector - Run quickstart/bin/stop-collector.sh
- HBase - Run quickstart/bin/stop-hbase.sh