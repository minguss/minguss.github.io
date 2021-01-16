---
layout: post
title:  "[kubernetes]-ExitCode"
author: minguss
categories: devlog
tags: go
---

최근에 Pod의 문제가 있어서 해결 하던 중 생각하지 못했던 포인트에서 문제를 찾을 수 있었다.  

Kubernetes운영을 할 때, 대부분 발생했던 문제는 `OOM Killed`나 `Exit Code 0`가 유발되는 코드로 Restart가 계속해서 발생하는 등의 문제였다.  
대부분 Kubernetes에 서비스가 Node또는 Java로 개발된 서비스들이 올라가다 보니 위 두가지 케이스를 자주 접할 수 있었다.  

대부분의 에러가 이렇다보니 Last Status를 확인하지 않고 무작정 클러스터의 상태만 보기에 바빳다.  
예를들면, `kubectl get events`라던지, Grafana의 Graph주기라던지 Resource영역에서 의심을 하기에 바빳다.  

그런데 이번 문제는 `kubectl describe po <podname>`을 통해 Last Status에 `Exit Code 2`를 확인했고, 의심되는 코드들이 발견되어 해결했었다.  

유심히 확인하지 않았던 Exit Code에 대해서 정리를 해보고자 한다.



## Check Exit Code
---
Exit Code는 `kubectl describe`명령어를 통해서 확인할 수 있다.  
특히 장애가 발생했던 경우 `Last Status`항목에서 확인할 수 있다.  

---
1. Exit Code 0
    - 이 종료 코드는 지정된 컨테이너 명령이 '성공적으로'완료되었지만 Kubernetes가 작동하는 것으로 받아들이기에는 너무 자주 완료되었음을 의미한다.  
    - 주로 일회성 BatchJob으로 만들어진 Pod을 Deployment(ReplicaSet, ReplicationController)로 만든 경우 이런 현상이 발생한다.  
    ***********
    **Runbook**
    - `kubectl describe -n [NAMESPACE_NAME] pod [POD_NAME] > /tmp/runbooks_describe_pod.txt`명령으로 단서가 될 만한 내용들을 찾아본다.  

---
2. Exit Code 1
    - 컨테이너가 명령을 성공적으로 실행하지 못했고 종료 코드 1을 반환, 이것은 시작된 프로세스 내의 `응용 프로그램 오류`이지만 얼마 후에 실패한 종료 코드와 함께 반환됩니다.
    - Pod과 연결되는 서비스의 부하로 인해서 Connection timeout이 발생하는 경우 (ex, DB)
    - Network단절로 인한 Timeout으로 Application내부에서 Error를 발생하는 경우
    ***********
    **Runbook**
    - `kubectl get nodes -o wide`로 노드의 상태를 확인해본다. 노드의 장애 상황일 수도 있다.
    - `kubectl describe -n [NAMESPACE_NAME] pod [POD_NAME] > /tmp/runbooks_describe_pod.txt`명령으로 단서가 될 만한 내용들을 찾아본다.

---
3. Exit Code 2
    - `응용 프로그램 오류`또는 `misuse of a shell builtin`
    ***********
    **Runbook**
    - Local에서 해당 이미지를 실행해보고 확인해봐야 한다. application문제이다.

---
4. Exit Code 128
    - 컨테이너를 실행할 수 없음을 나타낸다. `LastState` `Reason`이 `ContainerConnotRun`으로 나타난다. 

---
5. Exit Code 137
    - 컨테이너가 `kill -9` Signal을 받아서 Pod이 Terminated된 상태
    - *Case 1. Container 메모리 부족*
        - 이는 Application에 허용된 Resource보다 많은 양을 사용하기 때문에 발생하는 문제로 Pod의 Memory Limit을 늘린다.
    - *Case 2. OOM Killer가 Container를 Terminated*
    - *Case 3. Liveness Probe failure*
        ```
        Warning  Unhealthy  13s (x3 over 23s)  kubelet, dali      Liveness probe failed: cat: can't open '/tmp/healthy': No such file or directory
        ```

---
## Lessons Learned
예외 처리가 중요하다.  
Pod에서는 이러한 예외처리에 실패할 경우 Terminated되고 저절로 Restart하게 된다.  
사실 Kubernetes에 배포한 뒤 CrashLoopBackOff가 아닌이상 잘 지켜보지 않는다.  
하지만 저렇게 예외처리를 하지 않는다면, Restart가 빈번하게 발생할 것이고, Operator입장에서는 장애가 있는 Pod으로 인식할 수 있다.  
사실 UnitTest나 e2e테스트를 아무리 추가해도 실제 UT단계에서 어떤 예외가 나올지 예상하기가 힘들다.  
가능하면 테스트를 많이 하는게 중요하다 생각이 들었다.

