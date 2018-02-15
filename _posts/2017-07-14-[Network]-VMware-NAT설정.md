---
layout: post
title:  "VMware player - NAT Settings"
subtitle:   "VMware player - NAT Settingsr"
categories: devlog
tags: network
---


 VMWare Network을 설정하기 전에 다음과 같은 프로그램들을 준비합니다.

1. VMware Workstation 12 Player
1. Virtual Network Editor

Virtual Network Editor는 VMware Workstation 6 이후로는 기본으로 지원이 안된다고 합니다. 따라서 별도로 다운로드 받거나 다른 방법을 찾아보도록 합니다. 필자는 별도 첨부파일을 이용하여 설치했습니다.

설치에 앞서 NAT가 무엇인지 알아보도록 합니다.

---

### Network Address Translation(네트워크 주소변환)

>참조 : [위키백과_NAT](https://ko.wikipedia.org/wiki/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC_%EC%A3%BC%EC%86%8C_%EB%B3%80%ED%99%98 "위키")

네트워크 주소 변환(영어: network address translation, 줄여서 NAT)은 컴퓨터 네트워킹에서 쓰이는 용어로서, IP 패킷의 TCP/UDP 포트 숫자와 소스 및 목적지의 IP 주소 등을 재기록하면서 라우터를 통해 네트워크 트래픽을 주고 받는 기술을 말한다. 패킷에 변화가 생기기 때문에 IP나 TCP/UDP의 체크섬(checksum)도 다시 계산되어 재기록해야 한다. NAT를 이용하는 이유는 대개 사설 네트워크에 속한 여러 개의 호스트가 하나의 공인 IP 주소를 사용하여 인터넷에 접속하기 위함이다. 많은 네트워크 관리자들이 NAT를 편리한 기법이라고 보고 널리 사용하고 있다. NAT가 호스트 간의 통신에 있어서 복잡성을 증가시킬 수 있으므로 네트워크 성능에 영향을 줄 수 있는 것은 당연하다.

파일 호스팅 서비스에서 전용 업로드 프로그램과 함께 배포되는 NAT Service 프로그램도 네트워크 주소 변환의 일종이다.

---

이 포스팅에서 NAT는 간략하게 로컬PC와 VM사이의 네트워크 연결구조라고 볼 수 있다. 하지만 아무런 설정 없이 로컬 PC에서 cmd로 VM의 IP으로 ping을 날려보면 넘어가지 않는다. 따라서 다음과 같은 절차를 거쳐서 설정을 수행합니다.

(RedHat설치는 [Redhat-Installation](https://ko.wikipedia.org/wiki/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC_%EC%A3%BC%EC%86%8C_%EB%B3%80%ED%99%98 "Redhat-Install") Post를 확인합니다.)

1. Virtual Network Editor를 '관리자 권한'으로 실행합니다.

    ![Virtual NE](/assets/img/upload/vmware/6.png )
    - VMnet8을 선택한 후에 `Subnet IP`는 사용하고자 하는 `Private IP`로 설정하고 `SubnetMask`도 설정합니다. `NAT설정`은 기본적으로 x.x.x.2로 설정되어 있는데 별도로 설정하지 않습니다.

1. 다음은 Windows OS에서 Network Adapter 설정으로 이동합니다.
    
    ![NetworkAdapter](/assets/img/upload/vmware/7.png )
    - VMnet8 Adapter의 IP주소를 변경합니다. 해당하는 Adapter를 선택하고 IP설정창으로 이동합니다. 

1. IP주소를 `자동`으로 설정한 후 저장합니다. 만약 IP주소를 변경하고자 한다면 별도의 설정이 필요로 하는데 해당하는 부분은 아래의 URL을 참고하시기 바랍니다.

    ![IPSettings](/assets/img/upload/vmware/8.png )

1. 다음으로 Virtual Machine의 Network설정을 합니다. VM을 실행한 뒤 Network Configuration으로 이동합니다.

    ![VMIPSettings1](/assets/img/upload/vmware/9.png )
    ![VMIPSettings2](/assets/img/upload/vmware/10.png )
    ![VMIPSettings3](/assets/img/upload/vmware/11.png )
    ![VMIPSettings4](/assets/img/upload/vmware/14.png )
    - Host 네트워크 설정과 동일하게 `Automatic`으로 설정합니다. 이후에 Details에서 자동으로 부여된 IP정보를 확인할 수 있습니다.

1. Host에서 VM으로 정상적으로 신호가 가는지 확인해봅니다. cmd를 열어서 ping을 확인해봅니다.
    ![PingTest](/assets/img/upload/vmware/15.png )

1. VM에서 Terminal을 실행하여 ssh를 활성화 합니다. ssh를 활성화 하기 위해서 sshd를 실행 합니다.

            service sshd start

    ![sshd_start](/assets/img/upload/vmware/13.png )

1. 마지막으로 ssh로 접근을 시도해보도록 한다.
    ![putty](/assets/img/upload/vmware/16.png )
    ![putty_access](/assets/img/upload/vmware/17.png )