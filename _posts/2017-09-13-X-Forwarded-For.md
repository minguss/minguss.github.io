---
layout: post
title:  "X-Forwarded-For"
subtitle:   "Proxy ip - Client ip Settings"
date:   2017-09-13 08:00:00
author: minguss
categories: devlog
tags: web
---

XFF는 HTTP Header 중 하나로 HTTP Server에 요청한 Client의 IP를 식별하기 위한 표준입니다.

최근 테스트베드를 구축하면서 발생한 이슈는 다음과 같았습니다.
 - Web서버 앞단에 L4 Switch가 존재하는데 Load Balancing역할을 수행함합니다.
 - Web/WAS서버 로그를 확인해보면 접속한 Client IP가 아닌 L4 Swtich Proxy IP가 나옵니다.
 - Client PC Browser상에서 확인해도 실제 서버IP가 아닌 Proxy IP가 나옵니다.  

위와 같이 L4 Load balancer 장비가 존재하는데, 실제 Web/WAS서버에서 Proxy Server나 장비 IP에서 접속한 것으로 인식했습니다.
그렇기 때문에 웹서버는 실제 클라이언트 IP가 아닌 앞단에 있는 Proxy서버 IP를 요청한 IP로 인식하고, Proxy장비 IP 로 웹로그를 남기게 됩니다.  

> Client IP -> Proxy 서버 및 장비 -> Web Server

이 때 웹프로그램에서는 X-Forwarded-For HTTP Hearder에 있는 클라이언트 IP를 찾아 실제 요청한 클라이언트 IP를 알 수 있고,
웹로그에도 실제 요청한 클라이언트 IP를 남길 수 있습니다.
X-Forwarded-For 는 다음과 같이 콤마를 구분자로 Client와 Proxy IP 가 들어가게 되므로 첫번째 IP 를 가져오면 클라이언트를 식별할 수 있습니다.

> `X-Forwarded-For` Format
> X-Forwarded-For: client, proxy1, proxy2

저는 웹서버를 Webtier를 사용했습니다. 따라서 Apache 도 유사한 설정일 것이라 생각합니다.

Webtier 웹 서버에서는 LogFormat 지시자나 접근 권한 체크시 Remote Address 를 사용하는데 앞에 Reverse Proxy가 있다면 의도한 대로 동작하지 않으므로 XFF 헤더를 사용하도록 수정해야 합니다.

>`Before`  
>LogFormat `“%h %l %u %t \”%r\” %>s %b”` common

>` After `  
>LogFormat `“%{X-Forwarded-For}i %l %u %t \”%r\” %>s %b”` common
 
기본적으로 XFF헤더정보는 L4 Switch가 전달해줘야 합니다.  
따라서 Client IP를 받지 못할 경우 네트워크 장비로 부터 X-F-F 헤더정보를 추가해서 웹서버가 수신할 수 있도록 해야합니다.  
일반적인 L4 장비의 경우 옵션을 제공하지만 만약에 XFF Header를 Add하는 기능이 없다면 별도 모듈을 개발해야  할 것 입니다.