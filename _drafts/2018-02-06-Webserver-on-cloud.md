---
layout: post
title:  "webserver instance on cloud "
categories: devlog
tags: web network nginx
---

지난주 잘 사용하고 있던 Google Cloud Platform의 무료 크레딧 기간이 끝나서 집에 구축한 서버로 이관작업을 했습니다.  
처음에 Confluence를 구축할 때, Embedded DB를 사용해서 그런건지 Instance가 워낙 저사양으로 신청해서인지 굉장히 느렸는데 지금은 굉장히 빠릿빠릿(?) 합니다.  
여튼 이관 작업을 하면서 기존 Cloud에서 사용하던 도메인이 무쓸모가 되었는데, 이를 해결하기 위해서 여러가지 방법을 강구했습니다.  

제가 원하는 모습은 아래와 같습니다.  
``` bash
wiki.minguss.com -> wiki로 포워딩  
blog.minguss.com -> blog로 포워딩  
```  

생각하고 있는 모습은 위와 같은데, 결론적으로는 `apache`나 `nginx`같은 `웹 서버`를 구축해야 가능할 것 같습니다.  
호스팅업체나 공유기에서는 위와 같은 도메인으로 `연결`은 가능하지만, 전제가 있습니다.  
첫째, `각각의 Public IP`를 갖고 있어야 할 것, 둘째, 서비스 할 웹 서비스가 `80포트`로 연결되어 있어야 할 것 입니다.  
하지만 자체 구축한 서버는 Dynamic Public IP하나에 내부 Private IP로 되어 있어 외부에서 서비스를 접속하기 위해서는 하나의 IP만 사용할 수 있으며, 각 서비스는 공유기의 `포트포워딩`기능을 이용할 수 밖에 없었습니다.  
결국 호스팅업체나 공유기로는 원하는 모습대로 구축이 어렵고, 별도로 웹 서버를 구축해야 겠다는 생각을 했습니다.  

어느 환경에 웹 서버를 구축할까 고민을 하던 중 Cloud에 구축하기로 했습니다.  
사실 집에서 남는 Rasberry Pi가 있어서 이것을 활용 할 지 고민 했는데, 겸사겸사 Cloud공부도 할겸 추가하게 되었네요.  

개인적으로 Amazon에는 좋지 않은 경험이 있어 Azure를 쓸 생각입니다.  
