---
layout: post
title:  "Rancher"
date:   2018-01-02 08:00:00
author: minguss
categories: rancher docker devops
tags: docker rancher
share-img: https://minguss821.github.io/assets/img/rancher.jpg
---

Monitoring, Logviewer 환경을 구축하면서 많은 S/W와 그것들을 운용할 Node가 늘어나면서 Docker를 설정하게 되었습니다.  
이전에도 Docker를 사용하여 성능이점을 얻고자 했지만, 현재 Proj에서는 Docker를 사용함으로 얻어내는 성능상 이점이 적었습니다.  
그렇게 많은 프로그램이 돌아가지도 않고, VM으로 적절하게 서비스를 나눌 수 있었기 떄문이죠.  
하지만 서비스가 늘어남에 따라서 VM을 추가적으로 증설을 해야하는 경우가 발생하는데, GuestOS영역으로 늘어나는 불필요한 Resource의 증가와 전가상화를 사용하는 프로젝트에서 발생하는 퍼포먼스 저하로 인해서 Docker를 사용하게 되었습니다.  

Docker를 이전에는 CLI로 사용하면서 관련된 Commands를 익혔는데, 최근에는 GUI도구들이 많이 생겨난 것 같습니다. 그중에서도 Rancher라는 WebUI도구를 알게 되었는데, 매우 손쉽게 Docker를 Control할 수 있었습니다. 다만, 국내에서는 많이 사용을 하지 않아서 그런지 관련된 자료가 적어서 이참에 정리를 해봅니다.

---

### Rancher

Rancher는 Docker와 Kubernetes를 운영하고 관리할 수 있는 오픈소스 플랫폼 입니다. Rancher하나로 Docker 컨테이너를 관리하는데 필요한 전체 소프트웨어 스택을 얻을 수 있게 됩니다.

Rancher 소프트웨어는 네 가지 구성요소로 이뤄져 있습니다.

### Infra Orchestration
Rancher는 Linux 호스트 형태로 원시 컴퓨팅 리소스를 가져옵니다. 각 Linux 호스트는 가상 시스템 또는 물리적 시스템이 될 수 있습니다. Rancher는 각 호스트의 CPU, Memory, Disk Storage, Network Connection 이외에 다른 것들은 예측하지 않습니다. Rancher관점에서는 VM instance인지 bare metal 서버인지 구분하지 않습니다.

### Container Orchestartion and scheduling
다수의 사용자가 컨테이너 오케스트레이션 및 스케줄링 프레임워크를 사용하여 컨테이너화 된 응용 프로그램을 실행하도록 선택합니다. Rancher는 Docker Swarm, Kubernetes, Mesos를 포함한 모든 인기있는 컨테이너 오케스트레이션 및 스케줄링 프레임워크의 배포를 포함합니다. 이 외에도 Cattle이라는 컨테이너 오케스트레이션 및 스케줄링 프레임워크를 지원합니다. Rancher는 Swarm, Kubernetes 및 Mesos클러스터를 설정, 관리 및 업그레이드하는게 광범위하게 사용됩니다.

### Application Catalog
Rancher 사용자는 버튼을 한번 클릭하는 것으로 간단하게 어플리케이션을 실행할 수 있습니다. 또한 자주 사용하는 어플리케이션을 카탈로그 형태로 등록할 수도 있습니다. 등록된 어플리케이션은 자동으로 실행되고 업그레이드 됩니다. 이미 다양한 종류의 어플리케이션 카탈로그들이 존재합니다.

### Enterprise-grade control
Active Directory, LDAP, Github등의 인증시스템 그리고 Rancher에서 제공하는 RBAC(Role-Based Access Control)을 이용해서, 유저와 그룹의 권한을 설정할 수 있다.

아래 그림을 통해서 Rancher의 주요 컴포넌트들과 기능들을 확인할 수 있다.  
![Rancher](https://rancher.com/docs/img/rancher/rancher_overview_2.png)
