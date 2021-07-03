---
layout: post
title:  "[kubernetes]-Garbage-collection"
author: minguss
categories: devlog
tags: kubernetes
---

Kubernetes Garbage Collection
===
제목에서 확인할 수 있듯, Kubernetes에도 Garbage Collection이 존재한다.  

## Garbage Collection?
개념적으로 간단하게 알아보면, 동적 할당 된 메모리 영역 가운데 더 이상 사용하지 않는 영역을 확인해서 자동으로 해제하는 기법이다. 더 이상 사용하지 않는 영역이란, 어떤 변수로 가리키지 않게 된 영역(reference)을 의미한다.  

주로 Java, C# 등의 Unmanaged Language에서 GC가 사용되고, Java에서는 GC가 일어난 횟수나 주기에 대해서 성능 모니터링 포인트로 잡기도 한다.  

## Kubernetes에서의 GC?
ref: 
- https://kubernetes.io/docs/concepts/workloads/controllers/garbage-collection/    
- https://da-nika.tistory.com/146  

Kubernetes에서 Garbage Collection은 Kubernetes Resource안에 Owner Reference가 더이상 존재하지 않을 때 발생한다.  

기본적으로 Kubernetes에서 Resource를 생성할 때 ownerReference자동으로 설정한다. 예를 들어 ReplicaSet을 만들 때 Kubernetes ownerReference는 ReplicaSet에있는 각 Pod의 필드를 자동으로 설정하게 된다.  

아래와 같이 RC를 만들어서 배포해보면, pod에 Owner Reference가 생성되는 것을 확인해볼 수 있다.

``` yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-repset
spec:
  replicas: 3
  selector:
    matchLabels:
      pod-is-for: garbage-collection-example
  template:
    metadata:
      labels:
        pod-is-for: garbage-collection-example
    spec:
      containers:
      - name: nginx
        image: nginx
```

``` bash
kubectl apply -f https://k8s.io/examples/controllers/replicaset.yaml
kubectl get pods --output=yaml
```

위와 같이 배포해서 Pod의 Resource를 확인해보면,

``` yaml
apiVersion: v1
kind: Pod
metadata:
  ...
  ownerReferences:
  - apiVersion: apps/v1
    controller: true
    blockOwnerDeletion: true
    kind: ReplicaSet
    name: my-repset
    uid: d9607e19-f88f-11e6-a518-42010a800195
  ...
```
위와 같이 출력되는 것을 확인할 수 있다.  

![image](https://user-images.githubusercontent.com/22410442/124345110-b427e400-dc11-11eb-885f-f432469cc8d8.png)

이렇게 owner reference가 설정된 항목들은 garbage collection이 수행될 떄, 상위 객체를 따라서 delete할지 말지를 선택할 수 있다.  
연쇄적으로 삭제하는 방식은 cascading deletion이라고 불리운다.  

이 cascading deletion에는 두가지 방법이 존재하는데, 
1. foreground cascading deletion: 종속 항목을 먼저 삭제, 상위 항목은 종속 항목이 삭제된 뒤 삭제
2. background cascading deletion: 상위 항목을 먼저 삭제, 종속항목은 백그라운드에서 알아서 삭제   

아래와 같이 replicaset을 삭제하지만 pod들은 남아있도록 해보면
``` bash
kubectl delete replicaset my-repset --cascade=false
```
위와 같이 하게 되면, pod들은 그대로 남아있고 RC는 삭제되는 것을 확인해볼 수 있다.

**참고**  
kubernetes 1.20 이전 버전에서 owner reference가 지워지지 않는 현상이 발생하여 GC가 실행이 안되고 resource가 그대로 남아있는 현상이 발생했음.  
안전하게 1.20이상 버전을 사용하길