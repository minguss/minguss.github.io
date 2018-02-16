---
layout: post
title:  "Jenkins-Installation"
date:   2017-08-27 08:00:00
author: minguss
categories: devops ci
tags: devops
image: "/assets/img/jenkins-logo.png"
share-img: "/assets/img/jenkins-logo.png"
---


이전까지는 Jenkins가 Zip파일로 배포가 되었던 것 같은데 최근에는 Windows Version기준으로 .msi 설치본으로 배포가 되는 것 같다.
msi 설치본으로 제공되면서 훨씬 구축이 쉬워진 것 같다. port번호나 path의 경우에는 추후에 변경하면 될 것으로 보인다.

먼저 Jenkins 홈페이지로 이동한다.  
https://jenkins.io/

![jenkins_homepage](/assets/img/upload/jenkins/JenkinsHome.PNG)

Download 버튼을 클릭하고 OS별 설치파일 목록을 확인한다.

![jenkins_select](/assets/img/upload/jenkins/select.PNG)

### Windows Version 설치
![jenkins_windows1](/assets/img/upload/jenkins/1.PNG)  

Jenkins 설치 화면이다. Next버튼을 눌러서 설치를 진행해보자

![jenkins_windows1](/assets/img/upload/jenkins/2.PNG)  

Jenkins 설치 경로를 지정하는 화면이다. Repository 구성에 맞게 지정하도록 한다.

설치완료 후 지정한 Port로 변경할 일이 있을 수 있다. Port변경은 Jenkins를 설치한 root directory아래 `jenkins.xml`이라는 파일을 수정하면 된다.

``` xml
<arguments>-Xrs -Xmx256m -Dhudson.lifecycle=hudson.lifecycle.WindowsServiceLifecycle -jar "%BASE%\jenkins.war" --httpPort=8080</arguments>
```
이 부분에서 `--httpPort=8080`을 원하는 Port번호로 변경하면 된다.  
이후 Jenkins를 재기동 해야하는데 제어판 > 관리도구 > 서비스에 등록되어 있는 서비스를 재기동해도 되고, 관리자 권한으로 CMD을 열고 아래와 같이 순서대로 명령을 입력하여 재기동 하면 된다.

``` cmd
net stop jenkins
net start jenkins
```