---
layout: post
title:  "Web Service 처리과정"
subtitle: "how to work web services in java"
categories: devlog
tags: web network java middleware
---

Web Service
===
웹 서비스는 서로 다른 컴퓨팅 환경에서 사용되는 모든 Application들이 직접 소통하고 실행될 수 있도록 동적 시스템 환경을 구현해 주는 소프트웨어 컴포넌트라고 볼 수 있습니다.  
우리는 서로 다른 OS(Windows, Linux, MAC)를 사용하는데 웹 브라우저를 통해서 동일한 Web Application을 사용할 수 있죠.  
웹 서비스는 논리적 응용 프로그램의 단위로 데이터와 서비스를 다른 응용 프로그램에 제공하고, HTTP, XML, SOAP와 같은 표준화된 웹 프로토콜과 데이터 형식을 사용하여 위와 같은 서로 다른 OS플랫폼에서 원활한 데이터의 흐름을 보장해 줍니다. 거의 모든 메시징처리에 XML이 사용되어 상호운용성 또한 높습니다.  

### Web Service 를 이루는 요소
- SOAP(Simple Object Access Protocol)  
 분산 환경에서의 정보 교환을 목적으로 하는 경량의 XML기반 프로토콜입니다. 즉 웹 서비스에 사용되는 메시지의 데이터 포맷과 메시지의 처리룰을 정한 표준 통신 규약이라고 볼 수 있습니다.  
 SOAP를 지원하는 서버가 대중화 되면서 대부부분의 서버가 Web 접근이 가능해 졌습니다.   
 ![SOAP](http://api.epeople.go.kr/guide/images/soap_str.jpg)

 - SOAP의 장점  
    1. SOAP은 기본적으로 HTTP 기반 위에서 동작하기 때문에, HTTP와 같이 프록시와 방화벽에 구애받지 않고 쉽게 통신이 가능하다.
    2. SOAP는 표준 트랜스포트 프로토콜인 HTTP 이외의 다른 트랜스포트 프로토콜들(SMTP)을 사용할 수 있다.
    3. 플랫폼 및 프로그래밍 언어에 독립적이다.
    4. 간단하고 확장 가능하며, (멀티파트 MIME 구조를 사용하여) 첨부를 통합하는 SOAP XML 메시지를 지원한다.

 - SOAP의 단점  
    1. XML 포맷의 형태로 보내기 때문에 다른 기술과 비교해서 상대적으로 느리다. (최근 네트워크 속도의 비약적인 발전과 성능 최적화 기술의 발전으로 많은 부분이 해결되고 있다.)

 - SOAP Message Example  
    ![Example](http://api.epeople.go.kr/guide/images/soap_message_example.jpg)

- WSDL(Web Service Description Language)
 웹 서비스를 설명하는언어, WSDL을 통해 서비스를 구성하는 오퍼레이션과 요청/응답 메시지 구조, 데이터 타입 등을 인지할 수 있습니다. 때문에 WSDL이 어디에 있는지만 안다면 바로 적용해서 서비스를 사용할 수 있기 때문에, 적용이 편리합니다.  

- UDDI(Universal Description Discovery & Integration)
 WSDL이 위치하는 곳을 UDDI라고 설명하는 경우가 있습니다. 쉽게 생각해서 UDDI는 웹 서비스 공개저장소라고 생각하면 이해가 쉽겠습니다. WSDL을 찾을 수 있는 마켓이라고 판단하면 되겠네요. UDDI는 여러 WSDL을 가지고 있는 마켓이기 때문에 이 곳에서 원하는 WSDL을 이름별, 카테고리별로 검색하고 사용하는 공간입니다. 원하는 서비스를 찾아서 적용한다는 의미가 이것입니다. 하지만, UDDI가 아니더라도 네트워크상으로 접근이 가능한 곳에 위치한다면 웹서비스는 성립됩니다. 따라서 WSDL이 UDDI에 존재하는 것은 아닙니다.  

- REST(Representational State Transfer)
 REST는 월드 와이드 웹과 같은 분산 하이퍼미디어 시스템을 위한 소프트웨어 아키텍처의 한 형식으로, 로이 필딩(Roy Fielding)의 2000년 박사학위 논문에서 소개되었습니다. 로이 필딩은 HTTP의 주요 저자 중 한 사람으로 
 발표 당시는 대규모의 네트워크 시스템을 위한 방법이라는 뜻이었지만 최근 이용되고 있는 REST는 HTTP와 XML을 이용하여 데이터를 주고 받는 웹 서비스를 이용하는 것으로 쓰이고 있습니다.  
 ![REST](http://api.epeople.go.kr/guide/images/rest_str.jpg)  
위 그림과 같이 URL을 통해서 데이터를 요청하고 있으며 그 결과는 XML형태로 반환됩니다. 각각의 요청과 반환되는 XML은 아래와 같은 구조로 이뤄집니다.  
![REST_XML](http://api.epeople.go.kr/guide/images/rest_sample.jpg)
![SOAP_VS_REST](http://comtech2.com/wp-content/uploads/2016/08/web-services-a-practical-approach-7-638.jpg)


SOAP와 REST에 대한 자세한 내용은 추후 포스팅으로 다루도록 하겠습니다.

Web Service Flow (Overview)
===

### Browser부터 Server까지 Network의 큰 흐름  
[http://asfirstalways.tistory.com/297?category=685177](http://asfirstalways.tistory.com/297?category=685177)  


Web Service Flow (Server/Middleware View)
===
Web Service를 기본적으로 설명하라고 하면 아마도 기본적으로는 이렇게 생각할 것 입니다.  
![How does a Web Map Service work](http://www.e-cartouche.ch/content_reg/cartouche/webservice/en/image/wms_small.jpg)

Client는 Web browser를 사용해서 server에 정보를 request하게 되고 server는 database에 정보를 request하여 원하는 정보를 얻게 됩니다. 처리가 완료되면 browser를 통해서 client에게 반환합니다.  

간단한 flow로 본다면 이렇습니다. 실제로 Web Application은 3-tier로 나뉘어집니다. 정적인 이미지 파일이나 동영상 파일과 같은 `보여지는`영역이 있는 웹 서버 구간 `Presentation Layer`와 실제로 어플리케이션이 동작하는 Web Application 구간인 `Application Layer`, Database영역인 `Data Layer`이렇게 3가지로 구분되는데요. 아래의 그림과 같은 Architecture가 Enterprise환경에서 주로 쓰이는 구조라고 볼 수 있죠.  
![Architecture for an Enterprise Web-based Application](http://www.woodger.ca/images/harchgen.gif)  
소규모의 환경에서는 Presentation Layer와 Application Layer를 통합하여 사용하는 경우도 있습니다. 하지만 이럴 경우에는 페이지마다 이미지, 영상과 같은 크기가 큰 파일들을 새로 받아와야 하기 때문에 네트워크 부하량이 증가하게 되고 결국 성능저하의 요인이 됩니다. 모바일 환경이라면 데이터를 엄청나게 잡아먹겠죠.  
따라서 Web/WAS서버를 분리하는 이유가 여기 있겠습니다. 보통 Web개발을 할 때, Local개발환경에선 Tomcat과 같은 WAS서버를 띄우고 사용하죠. 이런 경우가 아래와 같은 Presentation Layer와 Application Layer를 통합한 환경이라고 볼 수 있겠습니다.  
![Architecture Alternative for a Smaller Web-based Application](http://www.woodger.ca/images/harcgens.gif)  

실제로 동작하는 내용은 아래를 참고합니다.  
![3tier](http://cfile3.uf.tistory.com/image/272B5A3554C26E4011B3D4)  
위의 그림과는 조금 다른 명칭으로 표현되긴 했지만 근본적인 내용은 동일합니다. 예시를 들어서 웹 서비스 순서를 알아보면 사용자는 웹 브라우저를 실행시키고 http://www.example.com 사이트에 접속합니다. 비즈니스 로직의 웹 서버는 파일 시스템에서 코드를 로드하고 구문해석과 실행을 위해 스크립트 엔진으로 코드를 전달합니다. 전달된 코드는 어플리케이션 계층에 있는 어플리케이션 서버의 관련 API를 호출하고 어플리케이션 서버는 데이터베이스 커넥터(jdbc, odbc)를 이용해 데이터베이스 계층의 데이터베이스에 연결을 하고 SQL 구문을 데이터베이스에 실행합니다. 데이터 베이스는 다시 데이터베이스 커넥터에 결과 값을 반환하고, 어플리케이션 서버는 결과를 보내기 전에 비즈니스 로직 또는 관련 환경에 맞게 구현합니다.  

이후 웹 서버는 사용자 인터페이스의 사용자 웹 브라우저에 HTML 코드로 전달하기 전 마지막 작업을 수행합니다. 사용자의 웹 브라우저는 전달된 HTML 코드를 실행하고 사용자에게 시각적으로 보여줍니다.

좀 더 자세하게 알아보면 browser로 부터 get request를 받게 되면 웹 서버의 httpd가 해당 request를 접수하고 static contents인지, dynamic contents인지 web server설정에 따라 redirect나 rewrite를 이용하여 forwarding하고 web server로부터 request받은 것을 was server의 thread가 처리합니다. thread는 db server에 해당하는 정보를 요청하고, dbms에서는 was서버로부터 받은 요청에 대한 처리로 처음에 db cache에 등록이 되어 있는지 확인한 뒤 없다면 disk를 읽어온 뒤 cache에 등록한 후 해당 thread로 전달합니다. db서버로부터 정보를 얻은 thread는 web서버로 응답을 한뒤 thread를 종료하고 web서버는 browser에 html code로 전달합니다.  

Application 관점에서의 Web Service Architecture는 Front-end, Back-end Framework에 따라 다르므로 별도 포스팅으로 정리하도록 하겠습니다.  

 참고URL   
 - http://api.epeople.go.kr/guide/contents/wstype_soap.html
 - http://api.epeople.go.kr/guide/contents/wstype_rest.html
 - http://www.nextree.co.kr/p11842/
 - http://www.e-cartouche.ch/content_reg/cartouche/webservice/en/html/wms_learningObject1.html
 - http://asfirstalways.tistory.com/297?category=685177
 - http://hack4profit.tistory.com/28