---
layout: post
title:  "Sonarqube-Installation"
date:   2017-08-27 08:00:00
author: minguss
categories: quality
tags: codequality
header-img: "https://www.sonarqube.org/assets/logo-31ad3115b1b4b120f3d1efd63e6b13ac9f1f89437f0cf6881cc4d8b5603a52b4.svg"
---

Sonarqube Installation
===

reference : https://medium.com/@cgpink/%EC%86%8C%EC%8A%A4-%EC%A0%95%EC%A0%81-%EB%B6%84%EC%84%9D%EB%8F%84%EA%B5%AC-sonarqube-%EB%A6%AC%EC%84%9C%EC%B9%AD-9d48fc62b01f  
http://egloos.zum.com/dryang/v/4005366

Sonarqube는 코드 품질을 지속적으로 검사할 수 있는 오픈소스 플랫폼이다.  
개인적으로 시각화나 UI가 상당히 괜찮아서 가능하면 정적분석, 동적분석 결과를 Sonarqube에 Publish하는것을 선호한다.  
![Sonarqube1](https://www.sonarqube.org/index/clean-code.png)
Sonarqube의 가장 큰 장점은 DashBoard에 한번에 표현될 수 있어 관리적인 측면에서 효과적이라고 보여진다.  
또한 오픈소스 툴이라서 그런지 각종 Plug-in들이 많은 편이고 가능하다면 Plug-in도 직접 개발할 수 있어 매우 좋다고 보여진다. 

설치를 진행하기 전에 Sonarqube사이트로 이동한다.  
https://www.sonarqube.org/

Download Tab으로 이동하여 최신 버전의 Sonarqube.zip파일을 다운로드를 받은 후  설치 할 경로에 압축을 풀어 놓는다. 

Sonar는 DBMS와 연동해야한다. 이를 위해서 MySQL을 설치하도록 한다. MySQL 설치는 http://withcoding.com/26 블로그를 참조하여 설치했다. 

DBMS 계정을 생성한다.
``` SQL
CREATE DATABASE sonar CHARACTER SET utf8 COLLATE utf8_bin; 
GRANT ALL PRIVILEGES ON sonar.* TO 'sonar'@'localhost' IDENTIFIED BY 'sonar';
```

Sonarqube 설정을 편집한다.
``` xml
<!-- conf/sonar.properties -->
sonar.jdbc.username=sonar
sonar.jdbc.password=sonar

sonar.jdbc.url=jdbc:mysql://localhost:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance

## 앞에 웹서버를 reverse proxy 로 사용할 경우 아래 context 는 우회하도록 설정 필요
sonar.web.context=/sonarqube
## PHP-FPM 사용시 포트 조정. 9001 은 sonarQube 내부의 elastic search 가 사용하는 포트이므로 주의
sonar.web.port=9003
```



