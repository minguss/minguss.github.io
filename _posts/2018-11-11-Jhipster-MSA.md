---
layout: post
title:  "JHipster로 Microservice 만들어보기 #1"
date:   2018-11-11 23:09:59
author: minguss
categories: dev
tags: JHipster MSA
---

최근에 MSA(Microservice Architecture)와 관련해서 이것저것 자료들을 정리하고 있습니다.  
그러던 중 서비스를 하나 운영 해봐야겠다 라는 생각을 갖게 되었고, 쉽게 Gen할 수 있는 방법을 찾던 중 Jhipster를 사용하게 됐습니다.  
사실 JHipster는 나온지는 꽤 됐고, 관련된 스터디자료나 프로젝트 사례가 있기 때문에 JHipster를 사용했습니다.  

몇년 전 까지만해도 Front-end는 Angular가 표준이었는데, 최근 React도 기본으로 서비스하고 있습니다.  

JHipster에서의 microservice 아래와 같은 기술스택을 사용하고 있습니다.  
![msa archi jhipster](https://nljug.org/wp-content/uploads/2018/04/afbeelding-1.png)  

기본적으로 Spring boot개발에 사용되는 'Netflix OSS'와 'Spring Cloud'을 적절하게 엮어서 사용되고 있습니다. Histrix와 같은 스택은 추가되어 있지 않지만 이런 부분들은 `Istio`를 사용할 것 이므로 필요없을 것 같네요.  

예제에는 ElasticSearch를 사용하도록 추가했지만, 이 부분 역시 별도로 구성하는 편이 좋을 것 같습니다.  

### Installation
먼저 아래의 Site에서 JHipster설치방법을 참고하여 관련된 asset들이 선작업 되어 있어야 합니다.  
[https://www.jhipster.tech/installation/](https://www.jhipster.tech/installation/)  

사전 설치 목록 (yarn 기준)
* Java 8 (저는 OpenJDK8버전을 사용했습니다.)
* Node.js (LTS 64-bit버전을 사용합니다.)  
* yarn 설치 (https://yarnpkg.com/en/docs/install#windows-stable)
* `yarn global add yo`
* `yarn global add generator-jhipster`

![jhipster install](/assets/img/upload/jhipster/1.png)  


설치 이후에 4개의 폴더를 생성합니다. 예제는 2개의 microservice와 uaa, gateway로 구성됩니다.  
``` bash
mkdir uaa blog store gateway
```
![jhipster install](/assets/img/upload/jhipster/2.png)  

다음은 아래와 같이 jhipster명령어를 사용하여 각각의 microservice와 gateway, uaa를 생성합니다.  
1. uaa
![jhipster install](/assets/img/upload/jhipster/3.png)  
2. gateway
![jhipster install](/assets/img/upload/jhipster/4.png)  
3. blog
![jhipster install](/assets/img/upload/jhipster/5.png)  
4. store
![jhipster install](/assets/img/upload/jhipster/6.png)  

생성한 뒤에는 Eclipse나 IntelliJ를 이용해서 Maven Import를 하면 됩니다. 전 Visual Studio Code를 애용하는 편이지만, 사양이 좋은 데탑에서는 Eclipse를 사용합니다. 다음 포스팅으로는 Eclipse 튜닝에 관련된 내용을 업로드 해야 겠네요.  

![import](/assets/img/upload/jhipster/7.png)  
이런식으로 하나씩 import를 해옵니다.  

정상적으로 import를 해오면, boot에 내장되어 있는 embeded tomcat을 이용해서 각각의 서비스를 동작시킬 수 있습니다.  

다만 사전에 또 해야할 것이 있는데요, JHipster-registry와 mysql, mongodb를 동작해야 합니다.  
``` bash
docker-compose -f uaa/src/main/docker/mysql.yml up -d
docker-compose -f store/src/main/docker/jhipster-registry.yml up -d
docker-compose -f store/src/main/docker/mongodb.yml up -d
```

그리고 각각의 서비스를 실행합니다.
![import](/assets/img/upload/jhipster/8.png)  

정상적으로 Boot 서비스가 실행되었다면, 브라우저에서 `localhost:8080`으로 접속을 시도해서 gateway로 정상적으로 연결되는지 확인합니다.  

![excute](/assets/img/upload/jhipster/9.png)  

지금까지 Jhipster로 MSA Application만들어보기를 살펴보았는데요, 다음 포스팅에서는 Kubernetes환경으로 배포하는 방법, 그리고 Entity추가하는 방법들을 정리해보도록 하겠습니다.
