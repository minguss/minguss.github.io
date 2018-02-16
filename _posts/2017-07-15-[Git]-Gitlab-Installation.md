---
layout: post
title:  "Gitlab-Installation"
date:   2017-07-15 08:00:00
author: minguss
categories: devlog devops
categories: devlog scm git
tags: git
---

gitlab은 ruby on rails framework 기반으로 개발된 github와 유사항 Web-based git repository management system입니다.

github와 달리 License는 MIT이기 때문에 활용도가 높다고 판단됩니다..

개인적으로 Git의 Configuration Management System을 좋아합니다. 그 이유에 대해서는 Git 사용법을 정리하면서 언급하도록 하겠습니다.

설치 전 Requirement를 살펴봅니다.
공식 홈페이지에서는 OS recommended를 다음과 같이 하고 있네요.

  - Ubuntu
  - Debian
  - CentOS
  - OpenSUSE
  - Raspberry Pi2

저는 CentOS를 선호하여 CentOS7에 설치하도록 했습니다. 

사전 준비사항을 알아볼까요?
먼저 필요한 Package들을 설치하도록 합니다.

## Installation
### Package Installation

```
/// System Update
sudo yum -y update

///Compiler & Linker, ETC... 
sudo yum -y groupinstall 'Development Tools'

///redis, compile library 
yum -y install vim-enhanced readline readline-devel ncurses-devel gdbm-devel glibc-devel tcl-devel openssl-devel curl-devel expat-devel db4-devel byacc sqlite-devel gcc-c++ libyaml libyaml-devel libffi libffi-devel libxml2 libxml2-devel libxslt libxslt-devel libicu libicu-devel system-config-firewall-tui redis sudo crontabs logwatch logrotate perl-Time-HiRes patch
```

### Configure redis

