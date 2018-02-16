---
layout: post
title:  "GCov+LCov+Cobertura"
date:   2017-08-24 08:00:00
author: minguss
categories: devlog quality devops
tags: testing
---

출처 : [조대협님 블로그](http://bcho.tistory.com/156)

## 코드 커버러지 (Code Coverage)

#### * 테스트 커버러지란?

우리가 단위 테스트나 통합 테스트와 같은 일련의 테스트 작업을 수행하였을때, 이 테스트가 전체 테스트를 해야 하는 부분중에서 얼마만큼을 테스트 했는지를 판단해야 한다.
예를 들어, 20가지의 기능을 가지고 있는 애플리케이션이 있을때, 몇가지 기능에 대해서 테스트를 했는가와 같이, 수행한 테스트가 테스트의 대상을 얼마나 커버했는지를 나타내는 것이 테스트 커버러지이다. 이 커버러지율을 기준으로 애플리케이션이 릴리즈가 가능한 수준으로 검증이 되었는가를 판단하게 된다.
위에서 예를 든것과 같이 기능에 대한 테스트 완료여부를 커버러지의 척도로 삼을 수 도 있다.
좀더 작은 범위의 테스트인 단위 테스트의 경우는 개개의 클래스나 논리적인 단위의 컴포넌트 각각을 테스트하기 때문에, 테스트에 대한 커버 범위를 각각의 클래스 또는 소스 코드의 각 라인을 척도로 삼을 수 있는데, 테스트가 전체 소스코드중에서 얼마나를 커버했는지를 나타내는것이 "코드 커버러지(Code Coverage)" 이다.

#### * 코드 커버러지 툴의 원리

코드 커버러지 툴의 주요 기능은 실행중에 해당 코드라인이 수행이 되었는가? 아닌가를 검증하는것이다. 이를 위해서 커버러지 툴은 클래스의 각 실행 라인에 커버러지 툴로 로깅을 하는 로직을 추가 하는 것이 기본 원리이다.  

예를 들어 아래와 같이 간단한 HelloWorld라는 소스가 있을 때, 소스 커버러지 툴을 거치게 되면 <그림 2> 와 같은 형태의 소스 코드를 생성해내게 된다.

``` java
public class HelloWorld(){
  public void HelloBcho(){
   System.out.println(“Hello Bcho”);
  }
  public void HelloHyunju(){
   System.out.println(“Hello Hyunju”);
  }
}
```
< 그림 1. 원본 소스 코드 >

``` java
public class HelloWorld(){
  public void HelloBcho(){
    CoverageTools.log(클래스 및 라인 관련 정보1);
  System.out.println(“Hello Bcho”);
CoverageTools.log(클래스 및 라인 관련 정보2);
}
public void HelloHyunju(){
    CoverageTools.log(클래스 및 라인 관련 정보3);
System.out.println(“Hello Hyunju”);
CoverageTools.log(클래스 및 라인 관련 정보4);
}
}
```
< 그림 2. 코드 커버러지 로그 수집이 추가된 코드>

그림 2와 같이 변형된 코드가 수행되게 되면 코드 수행에 따른 로그가 생성되게 되고, 이를 분석하여 코드중에 어느 부분이 수행되였는지를 보여주는 것이 코드 커버러지 툴의 기능이다.
이렇게 기존의 클래스에 커버러지 분석을 위한 분석 코드를 추가 하는 작업을 ‘instrument’ 라고 하고 크게 정적 기법과 동적 기법 두가지를 지원한다.
정적 기법은 애플리케이션이 수행되기 이전에 소스코드나 컴파일이 완료되어 있는 클래스 파일을 instrument하여 instrumented class들을 만든후 그것을 수행하는 방식이고, 원본 class를 가지고 애플리케이션을 수행하여 런타임시에 클래스가 로딩되는 순간에 클래스에 Instrumentation을 하는 것이 동적 방식이다.
 동적 방식은 AOP (Aspect Oriented Programming)이나, APM (Application Performance Monitoring)에서 많이 사용하는 방법이다. 그러나 이 방식의 경우 런타임에서 code instrumentation을 하는 부하가 발생하기 때문에, instrumentation양이 AOP나 APM에 비해서 압도적으로 많은 Code Coverage툴의 경우에는 정적인 instrumentation 방식이 좀더 유리 하다. 단 정적 방식의 경우 컴파일후에도 instrumentation을 한번 거쳐야 하고, coverage를 분석하기 위한 애플리케이션 묶음(JAR,WAR등)과 운영을 위한 애플리케이션 묶음이 다르기 때문에 용도에 따라서 매번 다시 배포(DEPLOY)해야 하는 번거로움이 있다.
 툴에 따라서 instrumentation 방식이 다르기 때문에 애플리케이션의 성격과 규모에 따라서 적절한 instrumentation을 사용하는 툴을 사용하기 바란다.

---

## 1. **Gcov**
Gcov는 커버리지 테스트 프로그램으로 GCC (C Compiler)에 내장되어 있어 GCC를 설치한 후 옵션을 통해서 Coverage데이터를 수집할 수 있다. gcov는 프로그램 코드의 어느 부분을 최적화 시키는 것이 가장 좋은 선택인지를 판단하기 위한 프로파일링 도구(profiling tool)로 사용될 수 있으며, gprof등과 함께 사용하면 프로그램 코드의 어느 부분이 데이터 입출력 처리를 제외한 사칙 연산 등의 실제 연산 처리 소요 시간을 가장 많이 차지하고 있는 지 알아낼 수 있다.

> **주의사항** : gcov를 사용하고자 한다면 컴파일 시에 GNU CC 최적화 옵션(Optimize Option)을 사용하지 않아야 한다. 왜냐하면, 코드를 최적화 해서 컴파일 할 경우에는 여러 개의 행들이 하나의 함수로 통합될 수 있기 때문에 프로그램 실행 시에 계산 시간이 많이 소요되는 부분을 찾기 위한 정보를 많이 얻을 수 없기 때문이다. 또한 gcov는 행을 최소 처리 단위로 통계를 산출하기 때문에 한 행에 하나의 문장을 입력하는 프로그래밍 스타일을 사용할 때 최상의 결과를 가져올 수 있다. 만약, 루프나 제어 구문으로 확장된 복잡한 형태의 매크로를 사용한 경우에는 매크로가 호출된 행만을 출력해 주는 gcov의 통계는 큰 도움이 되지 않는다. 복잡한 형태의 매크로가 마치 함수와 같이 사용될 경우에는 이러한 문제를 해결하기 위해서 매크로가 아닌 인라인 함수(inline function)의 형태로 수정할 수 있다.

### **Gcov의 사용법**

먼저 빌드 옵션에 **-fprofile-arcs -ftest-coverage**을 추가한다.

        gcov [-b] [-c] [-v] [-n] [-l] [-f] [-o 디렉토리이름] 소스파일이름


## 2. **Lcov**
LCOV는 Linux Test Project(이하 LTP)에서 GCOV의 결과를 HTML형식으로 보기 위해 제작된 GCOV확장 프로그램이다.
GCC 설치 시 기본적으로 설치되는 GCOV와는 달리 Extension Tool로 별도로 다운을 받아서 설치를 진행해야 한다.

### **LCov의 사용법**

``` bash
lcov [-c] [-d] [-o] 소스파일이름

// Example
lcov -c -d . -o coverage.lcov
lcov -r coverage.lcov "*.h" . -o coverage.lcov
lcov -a coverage.lcov -o total_coverage.lcov

//lcov -> html로 변환한다.
genhtml total_coverage.lcov
```

## 3. **Cobertura**

Cobertura는 정적 코드 커버리지(static code coverage) 툴로 뜻은 스페인어로 coverage라고 한다. 
정적 코드 커버리지는 자동화된 테스트 코드가 전체 코드중 어느 정도를 커버하고 있는지 분석해 주는 툴이다. 
코드중 테스트 비율을 바로 알수 있으므로 품질 향상에 도움을 주며 비슷한 제품으로는 상용으로는 아틀라시안의 clover 가 있다.

### **Maven과의 연동**
reporting 항목에 추가한다. (pom.xml)
``` bash xml
<reporting>
    <plugins>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>cobertura-maven-plugin</artifactId>
        <version>2.6</version>
      </plugin>
    </plugins>
  </reporting>
```

### **LCov -> Cobertura 변환**
LCov로 생성된 Coverage Report를 Cobertura로 변경하기 위해서는 별도의 작업이 필요하다.  
만약 Jenkins에서 확인하고 싶다면 Cobertura로 변경하는 과정이 필요할 것 이다.  
이럴 경우 별도의 Tool을 설치하여 사용하면 좋을 것이다.  
[lcov-to-cobertura-xml](https://github.com/eriwen/lcov-to-cobertura-xml)

사용법은 다음과 같다.

``` bash
python lcov_cobertura.py lcov-file.dat --base-dir src/dir --excludes test.lib --output build/coverage.xml --demangle
```

---

## **구축 사례**
먼저 Qt Framework 상에서 개발된 Source Code에 대해서 Coverage 측정을 했던 사례가 있다. 이런 경우 다음과 같은 과정을 통해서 Jenkins에서 확인할 수 있도록 설정했다.

### Qt Framework
Qt는 기본적으로 'QMake'를 이용하여 makefile을 생성한다. GUI환경에서 그리고 개발자 단말에서 직접 커버리지를 측정하기 위해 GCov옵션을 추가한 Binary를 생성하고자 하면 Build옵션에  -fprofile-arcs -ftest-coverage을 추가하면 되겠지만, 자동화 된 환경을 구축하고자 한다면 아래와 같은 방법을 이용하면 좋겠다.

``` bash
echo QMAKE_CFLAGS += -g -fprofile-arcs -ftest-coverage --coverage -fno-exceptions -fno-inline >> "$WORKSPACE/test.pro" //C 개발일 경우
echo QMAKE_CXXFLAGS += -g -fprofile-arcs -ftest-coverage --coverage -fno-exceptions -fno-inline >> "$WORKSPACE/test.pro" //C++ 개발일 경우
echo QMAKE_LIBS += -lgcov >> "$WORKSPACE/test.pro"
```

위와 같은 옵션을 추가하여 Gcov옵션을 추가하면 되겠다.
다만 Qt에서는 기본적으로 최적화 옵션을 주는것이 기본으로 설정되어있는 것 같다.
따라서 Qmake 이후 Makefile을 확인하면 Optimize option이 -O2로 설정되어 있는 것을 확인할 수 있다.
Manual상에서 기본적으로 Optimize옵션을 설정하지 않는것을 권장하고 있기 때문에 -O2 옵션을 -O0로 변경하는 과정이 필요하다.  (Optimization Option은 연결 페이지를 참고 한다.)

``` bash
find . -name 'Makefile' -exec sed -i 's/O2/O0/g' {} \;
```

이렇게 Makefile을 생성한 뒤 Make빌드를 수행한 후 Gcov bin와 .gcno파일과 gcov관련 파일이 생성됨을 확인한다.

이렇게 생성된 binary로 Coverage Test를 수행한 뒤 gcna파일이 생성이 된 것을 확인 한 후 커버리지를 측정 할 소스 파일에 대해서 gcov를 수행합니다. 

``` bash
gcov -b -c <filename>.o //단일 file의 경우
```

이후 lcov로 수집된 gcov 데이터를 reporting한다.

``` bash
lcov -c -d . -o test.cov
lcov -a test.cov -o total.cov
```

생성된 lcov report를 사용하여 html 형식으로 gen하거나 cobertura를 이용하여 jenkins에서 확인할 수 있도록 한다.