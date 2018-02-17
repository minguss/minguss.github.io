---
layout: post
title:  "GCC Installation(Source)"
date:   2017-08-26 08:00:00
author: minguss
categories: devlog
tags: linux
image: /assets/img/linux-logo.png
share-img: /assets/img/linux-logo.png
---

최근 프로젝트에서 GCC 버전과 관련된 이슈사항이 있었다.

보통 GCC와 같은 컴파일러는 Yum이나 apt로 설치가 가능하겠지만, 단독망(폐쇄망)을 운영하는 환경에서는 해당하는 컴파일러 버전의 RPM과 Dependencies RPM을 구해서 직접 설치하는 경우가 대다수 이다. 실제로 이렇게 설치를 하는것이 안정적이라고 볼 수 있다.

하지만 RPM이 지원이 안되는 경우도 발생할 수 있다.
이번에 발생했던 이슈도 이러한 상황이었다. Fedora24 기준으로 기본으로 설치되어 있는 버전은 GCC6.1.1, 최신버전도 6.3.x이다. 하지만 프로젝트에서 사용하는 GCC는 하위버전 이었다. 이러한 경우 Fedora24에는 GCC를 설치할 때 곤란한 상황이 발생할 수 있다. 제공하는 RPM이 없기 때문이다. OS제공사에서 직접 build를 돌려서 안정적인 버전을 RPM으로 묶어서 배포를 하는데, 하위 버전은 지원하지 않을 가능성이 있기 때문이다.

따라서 이러한 경우에는 직접 소스를 Compile하는 방법을 선택해야 할 것 이다.

먼저 Source를 컴파일 하기 위해서는 사전에 아래와 같은 사전 준비작업을 해야 한다.
1. Make Build를 위해서 GCC를 먼저 설치한다. (Version 관계 없음)
2. 설치할 버전의 GCC source를 다운로드 한다.
3. 시간 여유(실제로 설치할 때, 많은 시간이 걸린다.)

준비가 완료가 되면 다음과 같은 과정으로 설치를 진행한다. 먼저 압축을 해제한다.
``` bash
tar -xvf gcc-4.8.5.tar
```

다음으로 GCC컴파일에 필요한 라이브러리를 다운로드를 다운로드 받는다.
친절하게도 GCC빌드 전 다른 패키지들을 따로 번거롭게 설치할 필요 없이 모두 다운받을 수 있도록 스크립트로 제공되어 있다.

아마 설치하는 패키지는 산술 관련 라이브러리 일 것이다. ([mpfr](http://www.mpfr.org/),  [gmp](https://gmplib.org/), [mpc](http://www.multiprecision.org/index.php?prog=mpc), [isl](http://isl.gforge.inria.fr/))
```
./contrib/download_prerequisites
```
해당 스크립트를 돌려서 설치한 이후에는 Makefile을 만들어야 한다. Makefile을 생성하기 위해서는 configure를 먼저 수행하게 된다. configure를 할 때, 옵션을 줄 수 있는데, 용도에 따라서 옵션값을 조정하면 되겠다. 나는 C/C++이 필요하여 해당 옵션만 부여하도록 한다.

```
$ ./configure --prefix=/usr/local/gcc-4.8.5 --enable-checking=release --enable-languages=c,c++ --disable-multilib
```
Makefile이 생성이 되었다면 Make를 수행한다.

만약 CPU 여유가 있다면 다음과 같이 빌드해보자.
```
make -j <CPU core 수>
```
위 과정까지 원활하게 수행이 되었다면 install을 시작한다.
```
make install
```
install까지 정상적으로 수행이 되었다면 변경된 GCC버전을 확인해보도록 한다.

`사전에 /etc/profile을 수정하거나 GCC Path를 변경된 /usr/local/gcc-4.8.5로 변경해야 한다.`

```  
vi /etc/profile

export GCC=/usr/local/gcc-4.8.5
export C_INCLUDE_PATH=$GCC/include/c++/4.8.5/
export CPLUS_INCLUDE_PATH=$GCC/include/c++/4.8.5/
export LD_LIBRARY_PATH=$GCC/lib64:$PPL/lib:$LD_LIBRARY_PATH
export PATH=$GCC/bin:$PATH
```


