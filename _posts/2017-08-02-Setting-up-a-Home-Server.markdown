---
layout: post
title:  "Setting up a Home Server"
date:   2017-08-02 08:00:00
author: minguss
categories: essay
tags: server
---

올해 초 회사 업무만으론 새로운 도구들과 트렌드를 쫓아갈 수 없겠다는 생각을 해서 개인 서버를 구축하고 필요한 공부와 연구를 하고자 조립서버를 구축했다. 
  
이미 때 늦은 서버 설치기지만 최근 전기세 때문에 서버를 다른 곳으로 이전하게 되어 한번 더 구축기를 정리할겸 글을 남겨본다.  

사실 서버는 중고로 구매했다. E5-2670대란의 영향으로 매우 저렴하게 구매할 수 있었다. 이들 중 Power Supplier와 RAM, SSD/HDD, Raid Controller는 직접 구매하게 됐다. 일단 Power는 기존 550W였다. 하지만 나는 Raid5를 구성할 예정이어서 적어도 5개는 필요한 상황이었다. HDD뿐만 아니고 CPU자체가 전력을 꽤나 소비하는 녀석이어서 Power가 550W면 문제가 발생할 수 있다고 판단했다.(전원공급기 용량이 낮으면 당연한 이야기 이지만 정상적으로 동작하지 않는다. 일부 전원공급기가 폭발하는 사례도 있다고 한다.)  
뭐 전원 공급기는 유명한 제품들이 몇개 있지만 목적은 학습용 이기 때문에 크게 돈을 들이지 않기로 했다.  
사실 파워도 중고로 구매하려고 했지만 불안한 마음에 새제품을 구매했다.  
그리고 파워도 등급이 나뉘는데 일반적으로 고급 브랜드(에너맥스, 시소닉 등)의 골드or플래티넘 라인이 효율 상 일정/준수한 그래프를 보여준다.  
주변에서 파워에 돈을 아끼지 말라고 했는데 이미 다른 부품들도 사야했기 때문에 어느정도 타협을 해야했다. 파워서플라이를 구매하는데 필수조건은 아래와 같았다.
1. 850W 이상이 일 것
2. 모듈형으로 되어 있을 것
3. Silver tier이상일 것  

소비전력은 아래 링크에서 입력하면 좋겠다. 모델명을 입력하면 알아서 전력량을 계산해줘서 도움이 되는 사이트이다.
[https://outervision.com/power-supply-calculator](https://outervision.com/power-supply-calculator)

초기 구입당시 서버 사양은 아래와 같았다.


|구분|모델명|사양|
|:-----:|:-----:|:-----:|
|M/B|Intel S2600CP2J| - |
|CPU|E5-2670| 2.6GHz(8Core, 16Thread) * 2EA
|RAM|DDR3 8G PC3-10600 ECC/REG|64GB(8GB * 8EA)
|Power|SkyDigital| 550W
|Raid controller|Adaptec asr-5805|-
|Case|Inwin 707|-


![초기 구입당시](/assets/img/upload/server/1.png)  
이후에 서버 사양은 아래와 같이 구성했다.


|       구분      |           모델명          |              사양             |
|:---------------:|:-------------------------:|:-----------------------------:|
|       M/B       |      Intel S2600CP2J      |               -               |
|       CPU       |          E5-2670          | 2.6GHz(8Core, 16Thread) * 2EA |
|       RAM       | DDR3 8G PC3-10600 ECC/REG |        128GB(8GB* 16EA)       |
|   Power Supply  |     Superflower Silver    |              850W             |
|       SSD       |       Intel 750 SSD       |          400GB * 1EA          |
|       HDD       |        Toshiba HDD        |           2TB * 5EA           |
| Raid controller |      Adaptec asr-5805     |               -               |
|       Case      |         Inwin 707         |               -               |


![업그레이드 준비](/assets/img/upload/server/2.png)
![업그레이드 준비](/assets/img/upload/server/3.jpg)

실제 추가로 구입한 물품들은 RAM, SSD, HDD, Raid Controller, Cooler등이다. 이 부품들만 해도 본체값보다 더 들었지만 꽤 만족스러운 성능을 내줄것이라 확신하고 질러버렸다. (내 카드값..)

특히 SSD는 PCIe에 연결되는 NVMe 방식의 SSD를 구매했다. 개인적으로 궁금하기도 했고, 무엇보다 SATA에 연결되는 SSD방식보다 배로 빠르다는 것에 그 속도가 궁금하여 구매하게 되었다. 

![자료출처 : micron homepage](https://www.micron.com/~/media/track-3-images/one-column-content-module/miscellaneous/sio_micron_nvme_e_022316.jpg?la=en)

실제 구축결과 일반 SSD보다 조금 더 빨라진 느낌이었다. OS설치만 하더라도 엄청나게 빨라졌으니 말이다.

Raid Controller는 보드에 내장되어 있지 않아서 별도로 구매했다. (Intel S2600CP2모델은 지원된다고 하는데 Intel S2600CP2J는 아예 비어 있었다. 아쉽..)
Raid Controller는 Adaptec이 유명하다고하여 구매했는데, 연식이 꽤 있다보니 최근 나오는 Raid카드보다 성능이 좋을 것 같진 않다. 하지만 이정도면 충분하다고 보여진다. 그리고 `Adaptec asr-5805`모델이 생각보다 발열이 엄청나다. 너무 뜨겁다. 그래서 PCIe에 장착되는 쿨러도 추가로 구매하여 Raid 카드의 발열을 잡는데 노력했다. 전반적으로 Raid카드가 들어가니 발열이 너무 심한 것 같았다.

그리고 PCIe에 장착되는 ssd와 raid카드 위치때문에 고민을 많이 했다. 두 제품 모두 발열량이 엄청나기 때문이다. 그리고 추후에 gpu카드도 설치할 예정이라 많이 고민됐다. 하지만 현재로선 2개뿐이니 넓게 간격을 벌려서 사용하기로 했다.

---
설치 중 이슈사항

- Raid카드 인식 불가: Raid카드가 정상적으로 동작하면 부팅 중 raid설정화면으로 초기에 진입한다. 그런데 진입을 하지 못하는 문제가 있었다. H/W호환 문제인건지 펌웨어 문제인건지 확인이 어려워 계속된 삽질속에 해결했다. 문제는 바로 boot mode가 quiet mode였기 때문이다. 인텔보드의 경우 raid카드가 인식하지 않는다면 이 설정을 확인해 봐야 할 것 이다.


---
OS설치는 ESXi로 설치했다.  
이것저것 실습환경이 많이 필요하고 처음 구축당시에는 OpenStack을 공부해보려고 하다보니 가상OS가 필요했다. 여튼 ESXi로 설치했지만 현재는 Windows의 Hyper-V로 바꿔볼까 고민중이다.