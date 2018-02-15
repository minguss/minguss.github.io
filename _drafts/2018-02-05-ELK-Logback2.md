---
layout: post
title:  "Logback Connect with ELK Stack - 2"
categories: devlog
tags: rancher docker kibana elasticsearch logstash logback kafka filebeat
---

지난번 포스팅에 이어서 Logstash에서 수집한 자료를 어떻게 Kibana에서 보여줄 것 인지 설정해보도록 하겠습니다.  
ELK 스택과 Logback-Logstash 연동은 지난 포스트를 확인해 주시기 바랍니다.  
[Logback Connect with ELK Stack - 1](/2018-01-29-ELK-Logback)

먼저 앞서 포스팅했던 내용에서 단순히 ELK스택을 활용하여 구축했는데, 생각보다 많은 서버에서 Logdata를 수집해야 했고, Logback에서 Filelog로 저장하는 데이터들도 분석해야하기 때문에 이 부분도 추가하고자 `filebeat`와 `kafka` stack을 추가 하려고 합니다.  

![ELK-kafka](https://www.elastic.co/assets/bltedd75a8728fc83a2/scaling_ha1.png)

최종적으로는 위와 같은 Architecture로 Logviewer를 구축할 것 입니다.  

kafka는 원래 링크드인이 개발한 것으로, 2011년 초에 최종적으로 오픈 소스화 된 Apache 프로젝트 인데요, 이 프로젝트는 실시간 처리를 위한 분산 메시징 시스템에 쓰입니다. Linkedin, Twitter, Netflix, Tumblr 등 대용량을 다루는 업체들이 주로 kafka를 쓰고있다고 하네요.  
