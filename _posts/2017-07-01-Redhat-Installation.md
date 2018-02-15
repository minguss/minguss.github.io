---
layout: post
title:  "Redhat Installation"
categories: devlog
tags: redhat linux
---

RedHat-Installation using VMware Player 12
===

Redhat Install중 자동으로 넘어가는 현상이 발생했다. 원인은 찾지 못했지만, 자동으로 넘어가는 현상에 대해서 임시로 넘어갈 수 있는 방법을 정리했다.

1. 먼저 Install 수행 중 install destination을 설정한다.

  ![install](../upload/vmware/1.png )
  - installation destination을 설정하지 않으면 바로 설치가 진행되는 현상이 발생했다. 다른 가상화도구에서는 발생하지 않는데, VMware Player에서는 발생하므로 설치시 주의해야한다.

2. 설치 할 Hdisk를 설정한다.
  ![install2](../upload/vmware/2.png )

3. GUI환경으로 설치하기 위해서 `'Software Selection'`을 선택한다.
  ![install3](../upload/vmware/3.png )

4. `'Server with GUI'`를 선택한 후 `'Done'`을 클릭한다.
  ![install4](../upload/vmware/4.png )

5. `'Begin Installation'`을 선택한다.
  ![install4](../upload/vmware/5.png )