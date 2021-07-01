---
layout: post
title:  "[docker]-맥북에서 Docker Resource 줄이기"
author: minguss
categories: devlog
tags: python annotation
---

python 뉴비라서 새로운 패턴을 발견할 때 마다 새롭다.  
decorator라는 함수 앞에 annotation이 붙는 경우를 봤는데, 흐름 상 전처리를 하는 역할로 보여졌다.  

마치 Java에서 @Get, @Set과 같은 Annotation과 같이 보여지는데, 공통 함수로 User defined되어 있었다.  
정확한 동작 원리가 필요하여 정리해본다.  

**일반적인 Decorator**
``` python
def deco(func):
    def decorator(*args, **kwargs):
        print("%s %s" % (func.__name__, "before"))
        result = function(*args, **kwargs)
        print("%s %s" % (func.__name__ , "after"))
        return result
    return deco

# 함수에 데코레이터를 붙여준다.
@deco
def function(x, y):
    print(x + y)
    return x + y

func(1,2)
```
위와 같이 decorator라는 함수를 선언하고, func을 호출하기 전에 decorator를 붙여주게 되는데, 실행결과는 다음과 같다.  

``` python
func before
3
func after
```

바로 decorator를 통해서 우리가 호출 할 func을 수행하기 전에 전처리 할 함수들을 선언해볼 수 있겠다.  
예를 들어서 verification과 같은 func을 추가해서 dependency나 ready여부들을 확인할 수 있는 방법들을 추가해볼 수 있겠다.  

``` python
@timeout(10)
def main_func():
    nested_func()
    while True:
        continue

@timeout(5)
def nested_func():
   print "finished doing nothing"
```
나 같은 경우 위와 같이 timeout func을 추가하여 사용해봤다.  
