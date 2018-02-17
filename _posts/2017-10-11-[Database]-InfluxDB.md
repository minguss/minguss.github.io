---
layout: post
title:  "InfluxDB"
subtitle:   "InfluxDB"
date:   2017-10-11 08:00:00
author: minguss
categories: devlog
tags: nosql influxdb
---

먼저 CentOS7 기준 설치방법은 아래와 같다.

1. Yum Repo를 추가해야한다.  

    ``` bash
    cat <<EOF | sudo tee /etc/yum.repos.d/influxdb.repo
    [influxdb]
    name = InfluxDB Repository - RHEL \$releasever
    baseurl = https://repos.influxdata.com/rhel/\$releasever/\$basearch/stable
    enabled = 1
    gpgcheck = 1
    gpgkey = https://repos.influxdata.com/influxdb.key
    EOF
    ```  

2. Yum으로 설치를 다음 명령어로 진행한다.  

    ``` bash
    sudo yum install influxdb
    sudo service influxdb start
    ```  

3. 설치 이후에는 Configuration을 한다.  

    ``` bash
    ### 새 설정파일을 생성한다.
    influxd config > influxdb.generated.conf

    ### 그리고 influxdb.generated.conf을 원하는 구성대로 편집한 후 -config 옵션을 적용하여 원하는 config로 구동한다.
    influxd -config influxdb.generated.conf

    ### config와 -config는 기본구성과 custom 구성이 결합될 수 있다. 
    influxd config -config /etc/influxdb/influxdb.partial.conf
    ```

이번에 설치한 버전은 1.4v인데, 해당 버전에서는 Web UI를 지원하지 않는 것 같다.
localhost:8086으로 접근하면 `404 Page not found`에러가 발생하는걸 봐선 package에 포함되어있지 않은 것 같다.
