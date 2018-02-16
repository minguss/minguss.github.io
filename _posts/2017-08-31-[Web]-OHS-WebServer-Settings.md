---
layout: post
title:  "Connect ohs to weblogic"
date:   2017-08-31 08:00:00
author: minguss
categories: devlog middleware web
tags: web
---

OHS (Oracle Http Server)와 WLS(WebLogic)서버 연동 시, OHS Configuration을 수정해야한다.

먼저 `mod_wl_ohs.conf`를 수정해야한다. (${ORACLE_HOME}/ohs/modules/mod_wl_ohs.so)

```
LoadModule weblogic_module

<IfModule weblogic_module>
        SetHandler weblogic-handler
        WebLogicHost localhost
        WeblogicPort 7001
        Debug ON
        MatchExpression *.*
</IfModule>
```

만약 WAS서버가 클러스터링으로 구성되어 있다면 다음과 같이 수정해야 한다.
```
LoadModule weblogic_module

<IfModule weblogic_module>
        SetHandler weblogic-handler
        WebLogicHost localhost
        WeblogicPort 7001
        Debug ON
        MatchExpression *.*
    <Location /app>
        SetHandler weblogic-handler
        WebLogicCluster localhost:8002,localhost:8003
    </Location>
</IfModule>
```


