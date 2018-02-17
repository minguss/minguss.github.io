---
layout: post
title:  "Scouter APM 도구 설치"
subtitle:   "Setting up Scouter APM"
date:   2017-10-10 08:00:00
author: minguss
categories: devlog
tags: web
---

프로젝트에서 상용 APM을 사용하여 웹 프로젝트에 대해서 성능측정을 하고 있지만, 기대한 만큼의 기능을 사용하기가 어려웠고, Customizing이 필요한 단계여서 대안을 찾아보고 있었다.  
최근 APM도구를 찾아보던 중 'Scouter'와 'Pinpoint'두가지를 찾아볼 수 있었다.  
PinPoint는 Naver에서 개발한 Opensource Project로 Console이 WebUI로 제공된다는 점과 대용량 처리를 위해 HBase를 사용한다는점이 매력적이었다.
다만 JDK를 무려 3개나 설치해야한다는 번거로움과 Setting해야할 부분이 많다는 점에서 초기에 셋팅을 하다가 그만 두게 되었다.  

  
그래서 오늘은 다른 대안인 Scouter를 사용해보록 했다. 사실 Scouter는 요즘 읽고 잇는 책인 '실무로 배우는 시스템 성능 최적화'에서 소개된 도구인데, Eclipse Java 기반의 프로젝트로 비교적 쉽게 구축 및 운영을 할 수 있다. 다만 실시간 모니터링 용도로 Data를 저장하기 위해서는 별도로 설정을 해야하는 특징이 있다.

먼저 Scouter를 설치한다. 기본적으로 Scouter의 구조는 Agent와 Collector 구조로 이뤄져 있고 Client프로그램은 이 Collector에 연결하여 관련된 정보를 얻어오는 내용이다. 
![Scouter구조](https://github.com/scouter-project/scouter/blob/master/scouter.document/img/main/scouter-overview.png)

| Modules           | 설명                                                           |
|-------------------|----------------------------------------------------------------|
| Server(Collector) | Agent가 전송한 데이터 수집/처리                                |
| Host Agent        | OS의 CPU, Memory, Disk등의 성능 정보 전송                      |
| Java Agent        | 실시간 서비스 성능 정보, Heap Memory, Thread 등 Java 성능 정보 |
| Client(Viewer)    | 수집된 성능 정보를 확인하기 위한 Client 프로그램               |

자세한 설치는 [https://github.com/scouter-project/scouter/blob/master/scouter.document/main/Quick-Start_kr.md](https://github.com/scouter-project/scouter/blob/master/scouter.document/main/Quick-Start_kr.md)를 참고하여 설치하면 좋을 것 같다.

설치 후 APM Collector와 각종 Agent설정을 완료 한 뒤에 Client로 Collector에 접속하면 다음과 같은 화면을 확인할 수 있다.

![Scouter Client 접속화면](/assets/img/upload/scouter/1.PNG)

자세한 사용법은 또 다른 포스팅으로 적어보겠다.