---
layout: post
title:  "httpd-permission-error"
date:   2017-10-17 08:00:00
author: minguss
categories: devlog web
tags: web
---

webtier 서버 동작 중 permission 80포트에 대하여 permission error가 발생했습니다.  
발생하는 원인은 root로 web서버를 동작하거나 소유권자 혹은 그룹권한등이 변경되어 발생하는 현상들이라고 추측됩니다.  

먼저 해결방법으로는 webserver 기동 스크립트 혹은 웹 서버에서 참고하는 httpd경로를 찾아서 이동합니다.  
그리고 httpd의 권한을 확인합니다.  

확인을 해보면 httpd의 소유권자가 사용자이거나 그룹권한이 지정이 안되어 있거나, 사용권한이 없는 경우 3가지 유형이 있을 것 입니다.  
따라서 아래와 같은 command로 설정을 변경해주도록 합니다.
``` bash
su - 
chown root:<group> httpd
chmod 4775 httpd
```

이렇게 변경해준 뒤 script를 실행하면 정상적으로 동작하는 것을 확인할 수 있습니다.