---
layout: post
title:  "Pinpoint-Agent"
date:   2017-09-11 08:00:00
author: minguss
categories: devlog
tags: performance
---


Pinpoint는 설치했고, 실제 각 서버 노드마다 Agent를 설치해서 모니터링하는 설정을 해야합니다.

먼저 Tomcat 기준으로는 `/usr/share/tomcat7/bin`디렉토리로 이동해서 `catalina.sh`에 자바옵션을 추가해 줍니다.
cf)윈도우에서는 `catalina.bat`에 변경해야합니다.

```sh
JAVA_OPTS="-javaagent:/ide/pinpoint/pinpoint-agent/pinpoint-bootstrap-1.1.0.jar -Dpinpoint.agentId=sokit -Dpinpoint.applicationName=SOKIT"
```

그리고 실제로 /ide/pinpoint에 pinpoint-agent가 들어가 있도록 해당 폴더를 옮겨줘야 합니다.
`pinpoint/pinpoint/agent/target`에 있는 pinpoint-agent를 모두 /ide/.. 이하에 옮겨줍니다.
``` sh
cp -r pinpoint-agent /ide/pinpoint/
```

그리고 pinpoint-agent 들어가서 vi pinpoint.config를 열어줍니다.
profiler.collector.ip를 pinpoint 서버로 바꿔주고 포트를 29996 29995 29994로 바꿔줍니다.
그리고 중요한 부분이 29996 29995는 UDP입니다 해당 서버 방화벽 허용을 Custom TCP가 아닌 UDP로 꼭 바꿔줘야합니다. 
