---
layout: post
title:  "Microservice Architecture"
date:   2017-11-19 08:00:00
author: minguss
categories: devlog
tags: microservice
---

### Microservice 도입 배경
1. 뭔가 바꾸는게 두려워진다.
1. 개발자들이 과거 기술의 족쇄에서 벗어나지 못하고, 기술 격차는 계속 벌어진다.
1. 모든것은 '차세대'가 해결해야 줄 것이다.

### 그럼 MicroService는 무엇인가?
- 작다(Small)
- API형태로 다른 서비스와 연계하며 (communicate with APIs)
- 자율적이며 (autonomous)
- 한 가지 일을 잘하는데 초점을 맞춘 서비스 (focused on doing one thing well)
- RestURI에서는 Resource가 하나의 단위인데, 이 리소스 단위로 보는 경우도있고, 정의하기 나름인 것 같다.
- 중요한건 Microservice는 단일로 돌아가는 단위이며, 어떤 시스템을 어떤 단위로 만들것이냐에 따라서 달라지는 것이다.

### Microservice의 장점은?
- Technology Heterogeneity
- Resilience 
- Scaling 
- Ease of Deployment
- Organizational Alignment
- Composability
- Replaceability

### Microservice의 단점은?
- Complexity
- Multiple Database & Transaction Management
- Complicated Test
- Require Automation for Deploy/Operation
- Hard to develop features span multiple services

### SOA(Service Oriented Architecture)와 같지 않나?
- 비슷하나 다르다.
- SOA/ESB 서비스는 예를들어 레스토랑이라고 생각했을때, 고객(Client)과 주방(Service)사이에 웨이터가 각 Service에 정보를 변환하여 전달하는 방식
- MSA는 Client가 주문 시스템을 이용하는 방식과 같이 중간단에 웨이터가 존재하는 방식과는 다름

### Microservice 정리
- 작은단위로 개발단위를 쪼개다보니 개인이 생각보다 많은 SkillSet을 부담해야하는 일이 있다.
- 하지만 Dependency로 부터 자유로워질 수 있다.
- 변경 및 유지보수가 용이해진다.
- 다만 아키텍처가 너무 어려워지고, 고려해야할 요소가 많다.