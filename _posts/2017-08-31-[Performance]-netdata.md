---
layout: post
title:  "netdata"
date:   2017-08-31 08:00:00
author: minguss
categories: devlog
tags: performance
---

최근 DevOps Korea Community에 올라온 글 중 모니터링 도구와 관련해서 추천 글이 올라왔습니다.  
NetData라는 파이선 기반 웹 모니터링 도구인데, 오픈소스임에도 불구하고 정말 많은 기능들과 깔끔한 UI, Report형식으로 되어 있다보니 보고서 작성시에요 용이해보입니다.

링크 : [netdata](https://github.com/firehol/netdata)

모니터링할 수 있는 정보 목록은 다음과 같습니다.
- CPU
- Memory
- Disks
- sda
- Network interfaces
- IPv4 networking
- IPv6 networking
- Interprocess Communication - IPC
- netfilter / iptables Linux firewall
- Linux DDoS protection
- fping latencies
- Processes
- Entropy
- NFS file servers and clients
- Network QoS
- Linux Control Groups
- Applications
- Apache and lighttpd web servers
- Nginx web servers
- Tomcat
- web server log files
- mySQL databases
- Postgres databases
- Redis databases
- mongodb
- memcached databases
- elasticsearch
- ISC Bind name servers
- NSD name servers
- Postfix email servers
- exim email servers
- Dovecot POP3/IMAP servers
- ISC dhcpd
- IPFS
- Squid proxy servers
- HAproxy
- varnish
- OpenVPN
- Hardware sensors
- NUT and APC UPSes
- PHP-FPM
- hddtemp
- smartd
- disk S.M.A.R.T. values
- SNMP devices
- statsd

---
## netdata Inforgraphic
![netdata inforgraphic](https://cloud.githubusercontent.com/assets/2662304/26529478/104652ac-43c9-11e7-903f-edb9bb2ced24.png)

---
설치 방법은 쉘 스크립트를 사용한 자동 설치 방법과 직접 다운로드 하여 수동으로 설치하는 방법 두가지가 있습니다. 집에서 운영중인 서버는 automatic installation방법을 설치했는데, 단독망 혹은 폐쇄망에서는 직접 수동 설치가 필요할 것 같아서 정리해 봤습니다. 

먼저 netdata설치 전에 선행되어 깔려야 할 Package들을 설치합니다.
> RHEL/CentOS는 EPEL Repository설정이 필요합니다. [EPEL링크](https://www.tecmint.com/how-to-enable-epel-repository-for-rhel-centos-6-5/)

``` bash
# Debian / Ubuntu
apt-get install zlib1g-dev uuid-dev libmnl-dev gcc make git autoconf autoconf-archive autogen automake pkg-config curl

# Fedora
dnf install zlib-devel libuuid-devel libmnl-devel gcc make git autoconf autoconf-archive autogen automake pkgconfig curl findutils

# CentOS / Red Hat Enterprise Linux
yum install autoconf automake curl gcc git libmnl-devel libuuid-devel lm-sensors make MySQL-python nc pkgconfig python python-psycopg2 PyYAML zlib-devel
```

다음으로 netdata를 설치합니다.
``` bash
# download it - the directory 'netdata' will be created
git clone https://github.com/firehol/netdata.git --depth=1
cd netdata

# run script with root privileges to build, install, start netdata
./netdata-installer.sh
```
만약 한번에 설치되는 과정을 직접 수동으로 제어하고 싶다면 `--dont-start-it` 옵션을 추가해줍니다.  
또한 기본 디렉토리에 설치되는 것을 원치 않는다면 
``` 
./netdata-installer.sh --install /opt 
``` 
위와 같이 입력하면 `opt/netdata` 경로에 netdata가 설치됩니다.

