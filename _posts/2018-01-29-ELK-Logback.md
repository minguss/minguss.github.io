---
layout: post
title:  "Logback Connect with ELK Stack - 1"
date:   2018-01-29 08:00:00
author: minguss
categories: devlog
tags: rancher docker kibana elasticsearch logstash logback
---

작년부터 APM을 구축하면서 Logviewer도 만들면 좋겠다 라는 생각을 했습니다.  
Test단계에 이르면서 수 많은 Error들을 확인하는데, Logback에서 떨어트리는 log text파일의 저장공간도 부담이고, 일일이 확인하는데 어려움이 있어 Logviewer를 구축하는게 좋겠다 라는 생각을 하게 되었죠.  

도구를 검색하던 중 생각보다 Logviewer 프로그램이 많았습니다.  
그 중에서 최근 데이터 통계분석에서 많이 사용되고 있는 ELK(Elasticsearch, Logstash, Kibana) Stack을 활용한 로그분석 시스템을 구축해보고 싶어서 ELK Stack을 선정했습니다.

추가적으로 현재 프로젝트가 온프레미스 환경이지만 수집해야 할 서버가 많았고, ELK스택을 구축할 서버도 자원이 넉넉한 편이 아니라서 분산 환경을 구축하고자 `Docker`를 이용하여 인프라 환경을 구축하기로 했습니다.

결과적으로는 아래와 같은 환경으로 전체 Logviewer 시스템을 구축하려고 합니다.  
![Logviewer Architecture](https://2.bp.blogspot.com/-rEr0RSTQwBA/WX35K0zSjnI/AAAAAAAAPJI/6L3aLUGpfgIKpmbdT_RjufZ88p4-M3C3ACLcBGAs/s640/Sending%2BLogs%2Bto%2BELK%2Bthrough%2BLogback%2B-%2BPage%2B1%2B%25281%2529.png)  


최근에 알게 된 Docker Orchestration Tool중에 `Rancher`를 알게 되었는데 굉장히 사용하기가 간편합니다. 특히 설치된 Docker Image자체를 Catalog형식으로 제공해서 손 쉽게 설정해주는 것들이 맘에 들더군요. 그리고 `Rancher` 내부적으로 `Kubernetes`와 `Docker Swarm`도 지원하여 확장 및 활용성이 좋아보입니다.  


ELK Stack 설치
===
먼저 Rancher Web Console에 접속해서 `Stack`탭을 클릭합니다.  

`browse catalog`을 클릭합니다.  

![add stack](/assets/img/upload/rancher/addstack1.png)  

ELK Stack에서 가장 먼저 설치되어야 하는 것은 Elasticsearch입니다. 먼저 Elasticsearch Catalog를 찾아서 설치하도록 합니다. (View Details버튼을 누릅니다.)

![add elasticsearch](/assets/img/upload/rancher/addelastic.png)  

View Details를 확인하면 상세 설정페이지가 표시됩니다. 세부적인 설정은 환경에 맞춰서 설정하면 되겠습니다. (Default설정 중 발생했던 에러를 공유합니다. 초기에 `Upgrade host sysctl`을 `true`로 설정하지 않으면 vm.max_map_count를 지정하라는 에러가 발생했습니다. 이 문제가 해결되지 않으면 elasticsearch docker container가 계속해서 restart되는 현상을 발견할 수 있습니다. memory set문제인 듯 합니다.)

![add stack2](/assets/img/upload/rancher/addstack2.png)  

설치가 진행된 후 Active Status를 확인합니다.  
진행이 완료되면 다음단계로 진행합니다.

![add stack3](/assets/img/upload/rancher/addstack3.png)  

다음으로 Logstash를 설치하도록 합니다. Logstash도 마찬가지로 Catalog에서 검색합니다. 그리고 Elasticsearch에서 master node와 연결합니다.  

![add logstash](/assets/img/upload/rancher/addlogstash.png)  
![check logstash](/assets/img/upload/rancher/checklogstash.png)  

마지막으로 결과를 화면으로 보여주는 Kibana를 설치합니다. Kibana도 Logstash와 동일하게 Catalog에서 검색한 뒤 es-master node와 연결하여 설치합니다.

![add kibana](/assets/img/upload/rancher/addkibana.png)  
![check kibana](/assets/img/upload/rancher/checkkibana.png)  

설치가 완료되면 kibana로 접속하여 확인합니다. 
![kibana](/assets/img/upload/rancher/kibana_main.png)  


최종적으로 Rancher에서 ELK Stack은 아래 그림과 같이 연결됩니다.  
![elk architecture](https://www.cnrancher.com/wp-content/uploads/2016/12/%E5%9B%BE3-2.jpg)  

Logback 연결
===

ELK Stack설치 이후 Logback과 연결을 합니다. 저는 현재 진행중인 개인적으로 진행중인 Spring boot환경의 프로젝트가 있어 해당 프로젝트와 연결하도록 합니다.  
Maven을 사용하는 프로젝트라면 `pom.xml`에 dependencies를 추가해야 합니다.  

`pom.xml`
``` xml
        <dependency>
            <groupId>net.logstash.logback</groupId>
            <artifactId>logstash-logback-encoder</artifactId>
        </dependency>
```

Maven install을 해서 Library를 잘 다운받았는지 확인한 뒤에 Logback설정을 하도록 합니다.
![logstash-logback](/assets/img/upload/rancher/logstash-logback-encoder.png) 

Logback 설정파일로 이동합니다.
보통 `src/main/resources/logback/logback.xml`파일이 존재할 것 입니다. 해당 파일을 열어봅니다.

`logback.xml`
``` xml
<?xml version="1.0" encoding="UTF-8"?>

<configuration scan="true">
    <include resource="org/springframework/boot/logging/logback/base.xml"/>
    <appender name="console" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <Pattern>%d %-5level [%thread] %logger{0}: %msg%n</Pattern>
        </encoder>
    </appender>

    <appender name="logstash" class="net.logstash.logback.appender.LogstashSocketAppender">
        <!-->use logstash host/port<-->
        <host>localhost</host>
        <port>5000</port>
    </appender>

    <logger name="javax.activation" level="WARN"/>
    <logger name="javax.mail" level="WARN"/>
    <logger name="javax.xml.bind" level="WARN"/>
    <logger name="ch.qos.logback" level="WARN"/>
    <logger name="com.codahale.metrics" level="WARN"/>
    <logger name="com.netflix" level="WARN"/>
    <logger name="com.netflix.discovery" level="INFO"/>
    <logger name="com.ryantenney" level="WARN"/>
    <logger name="com.sun" level="WARN"/>
    <logger name="com.zaxxer" level="WARN"/>
    <logger name="io.undertow" level="WARN"/>
    <logger name="io.undertow.websockets.jsr" level="ERROR"/>
    <logger name="org.ehcache" level="WARN"/>
    <logger name="org.apache" level="WARN"/>
    <logger name="org.apache.catalina.startup.DigesterFactory" level="OFF"/>
    <logger name="org.bson" level="WARN"/>
    <logger name="org.elasticsearch" level="WARN"/>
    <logger name="org.hibernate.validator" level="WARN"/>
    <logger name="org.hibernate" level="WARN"/>
    <logger name="org.hibernate.ejb.HibernatePersistence" level="OFF"/>
    <logger name="org.springframework" level="WARN"/>
    <logger name="org.springframework.web" level="WARN"/>
    <logger name="org.springframework.security" level="WARN"/>
    <logger name="org.springframework.cache" level="WARN"/>
    <logger name="org.thymeleaf" level="WARN"/>
    <logger name="org.xnio" level="WARN"/>
    <logger name="springfox" level="WARN"/>
    <logger name="sun.rmi" level="WARN"/>
    <logger name="liquibase" level="WARN"/>
    <logger name="LiquibaseSchemaResolver" level="INFO"/>
    <logger name="sun.rmi.transport" level="WARN"/>

    <!-- https://logback.qos.ch/manual/configuration.html#shutdownHook and https://jira.qos.ch/browse/LOGBACK-1090 -->
    <shutdownHook class="ch.qos.logback.core.hook.DelayingShutdownHook"/>

    <contextListener class="ch.qos.logback.classic.jul.LevelChangePropagator">
        <resetJUL>true</resetJUL>
    </contextListener>
    
    <root level="WARN">
        <appender-ref ref="CONSOLE"/>
    </root>
    
    <root level="INFO">
        <appender-ref ref="logstash"/>
    </root>
    <root level="WARN">
        <appender-ref ref="logstash"/>
    </root>
    <root level="DEBUG">
        <appender-ref ref="logstash"/>
    </root>
</configuration>
```

해당 설정을 끝낸 뒤 Kibana를 다시 실행시켜 보면 @timestamp를 추가할 수 있습니다. create한 뒤, dashboard로 가면 로그가 계속 들어오는걸 확인할 수 있습니다.  

자 여기까지 1차 설치 과정입니다. Dashboard설정과 수집 pattern설정은 이후 포스팅에서 정리하도록 하겠습니다.