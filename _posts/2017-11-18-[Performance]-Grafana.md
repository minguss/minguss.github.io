---
layout: post
title:  "Grafana"
date:   2017-11-18 08:00:00
author: minguss
categories: devlog
tags: performance
---

Grafana는 Opensource Dashboard 도구입니다. APM 도구의 효율성 증가를 위해 이 대시보드를 이용해서 원하는 데이터를 Dashboard형태로 모두가 공유하려고 합니다.  
사실 개인적으로 Netdata가 좋긴 하지만, AIX환경에서의 설치가 현재로선 어려워 Grafana를 이용한 모니터링 환경 구축을 하게 되었습니다.

먼저 설치방법은 아래 Step을 따라 설치한다.

**설치는 CentOS7 기준이다.**
1. Yum Repo를 `/etc/yum.repos.d/grafana.repo`를 생성해서 아래와 같이 내용을 추가해준다.
    ~~~ bash
    [grafana]
    name=grafana
    baseurl=https://packagecloud.io/grafana/stable/el/6/$basearch  
    
    repo_gpgcheck=1
    enabled=1
    gpgcheck=1
    gpgkey=https://packagecloud.io/gpg.key https://grafanarel.s3.amazonaws.com/RPM-GPG-KEY-grafana
    sslverify=1
    sslcacert=/etc/pki/tls/certs/ca-bundle.crt
    ~~~  

2. yum으로 설치를 진행한다.  

    ~~~ bash
    sudo yum install grafana
    ### if yum download not working
    sudo yum install https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana-4.6.2-1.x86_64.rpm
    ~~~  

3. 설치 완료 후 Grafana를 실행한다.  

    ~~~ bash
    sudo service grafana-server start
    ~~~

4. 실행 시 자동으로 동작하도록 설정한다.  

    ~~~ bash
    sudo /sbin/chkconfig --add grafana-server
    ~~~ 

5. 설정은 `/etc/grafana/grafana.ini`파일을 수정한다.  

6. browser를 켠 후 localhost:3000으로 접속한 후 admin/admin으로 로그인한다.  
    ![grafana](https://mblogthumb-phinf.pstatic.net/MjAxNzAyMjNfMjIg/MDAxNDg3ODM5NzQ3MjI0.TXagBpqNGfHB8kiNY1uBvXqbkf1GP_qWew_iaHucHYAg.9fxDjvqF6a9H5A1F53rqy8HjCE3v03ILeAFacQiFZnAg.PNG.ships95/Grafana002.png?type=w2)