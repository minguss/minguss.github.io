---
layout: post
title:  "[centos]-Failed to mount/sysroot"
author: minguss
categories: devlog
tags: esxi centos
---

요즘 ESXi를 재부팅할 일이 많아졌는데, VM을 부팅할 때 마다 평소에는 나타나지 않았던 에러메세지가 자주 보이기 시작했다.  
아무래도 하드디스크가 슬슬 맛이가기 시작한 모양이다.  

아쉽게도 화면 캡처는 하지 못했지만, 아래와 같은 메세지가 `빨간색`으로 표시된다.
```
Failed to mount /sysroot
```

이 메세지 위로 몇 줄 올라가서 로그를 살펴보면, sysroot를 mount를 시도하고, xfs파일시스템 문제가 발생했다고 뭐라뭐라 한다.  
이 부분을 보고 아무래도 스토리지에 문제가 있다고 판단.. 이미 혹사 시키고 있는 녀석이기에 배드섹터가 있을 수 있는데, 문제가 생기면 그냥 보내주려고 한다.  

뭐 여튼 저런 오류 메세지가 나타나면 아래의 명령어를 입력하여 파일시스템을 재설정 해주면 된다.  

```
xfs_repair -v -L /dev/dm-0
```

command에서 알 수 있듯 xfs를 복구해주는 명령어다. 옵션값은 verify와 L은 뭔지 모르겠다. device name은 저게 다르다면 `df -h`로 확인한 뒤에 넣어주도록 하자.  

ref:
- https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/storage_administration_guide/xfsrepair
- https://m.blog.naver.com/yexx/221995836137

