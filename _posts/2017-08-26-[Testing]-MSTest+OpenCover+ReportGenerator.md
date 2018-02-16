---
layout: post
title:  "MSTest+OpenCover+ReportGenerator"
date:   2017-08-26 08:00:00
author: minguss
categories: devlog quality devops
tags: testing
---

Visual Studio에는 기본적으로 Unit Test Tool을 지원한다.  
아마도 가장 많이 사용하는 Tool이 MSTest일 것이다.  
최근 프로젝트 중 MSTest로 Unit Test를 진행했지만 Branch Coverage가 나오지 않는다고 하여 정말로 나오지 않는 것인지 확인한 결과 Branch Coverage를 나올 수 있도록 하는 방법을 알게 되었고, 작업은 하는 김에 관리파트에서 원활하게 관리할 수 있도록 Jenkins에 연결시키려고 한다.  

사용한 Tool은 다음과 같다.
- [MSTest](https://docs.microsoft.com/ko-kr/dotnet/core/testing/unit-testing-with-mstest)
- [OpenCover](https://github.com/opencover/opencover/releases)
- [ReportGenerator](http://danielpalme.github.io/ReportGenerator/)
- [OpenCoverToCoberturaConverter](https://github.com/danielpalme/OpenCoverToCoberturaConverter)

MSTest는 Visual Studio에 기본적으로 탑재되어 있는 code coverage tool이다.  
도구를 알아보던 중 MSTest 이외에도 [NUnit](http://nunit.org/), [NCover](https://www.ncover.com/)가 많이 사용되는 Tool로 확인된다. 

OpenCover도 NUnit과 NCover와 같은 Opensource기반 code coverage tool이다.  
자세한 내용은 링크를 참조하자  
https://www.codeproject.com/Articles/677691/Getting-code-coverage-from-your-NET-testing-using

ReportGenerator는 OpenCover로 생성한 xml을 HTML로 변환하여 Report타입으로 변환시켜주는 Tool이다. 이전에 포스팅한 LCov와 비슷한 Tool이라고 생각하면 좋겠다.  

OpenCoverTocoberturaConverter는 OpenCover로 생성한 xml파일을 Cobertura Format으로 변경해주는 Tool이다. Jenkins에서 확인할 때, 이용할 수 있는데 취향에 맞춰 HTML Publish를 사용하거나 Cobertura Format으로 변경하여 사용하거나 선택을 하면 되겠다.  

기존에 Gcov + Lcov + Cobertura 조합을 사용했기 때문에 이번에도 역시 Cobertura로 적용할 생각이다.

먼저 좋은 Tutorial 자료가 있어서 공유한다.   
http://ziyasal.github.io/development/jenkins-opencover-nunit-code-metrics-coberture-oh-my  
https://www.youtube.com/watch?v=0UJNYrhH05w

먼저 위 프로그램들을 설치한다. (Visual Studio에서 Nuget패키지로 다운받는것 보다는 직접 설치를 하는것이 좋다. Path를 별도로 설정해줘야하는 번거로움이 있어 가급적 .msi 파일로 설치하는 것이 좋겠다.)

나는 MSTest를 사용할 것이므로 먼저 MSTest이 정상적으로 동작하는지 확인해 본다. 
``` cmd
MSTest.exe /noisolation /testcontainer:\"C:\workspace\ConsoleApplication1\bin\Debug\ConsoleApplication1.dll\" /resultsfile:C:\coverage\coverage.trx
```

이상이 없이 잘 동작한다면 OpenCover를 사용하여 coverage정보를 수집한다.  
``` cmd
OpenCover.Console.exe -register:user -target:MSTest.exe -targetargs:"/noisolation /testcontainer:\"C:\workspace\ConsoleApplication1\bin\Debug\ConsoleApplication1.dll\" /resultsfile:C:\coverage\coverage.trx" -mergebyhash -output:C:\coverage\CoverageReport.xml
```

.xml파일이 생성된 것을 확인 한 후에 ReportGenerater로 Report를 생성한다.
``` cmd
C:\ReportGenerator_2.1.8.0\bin\ReportGenerator.exe -reports:"C:\coverage\CoverageReport.xml" -targetdir:${WORKSPACE}\coverage1 -sourcedirs:${WORKSPACE}
```

Report가 잘 생성되었는지 확인한다.  

![Summary](/assets/img/upload/testing/OpenCover.PNG)  

OpenCovertoCobertura를 이용하여 Cobertura Format으로 Report를 변환한다.
``` cmd
OpenCoverToCoberturaConverter.exe -input:C:\jenkins\workspace\TestCoverage1\coverage.xml -output:C:\jenkins\workspace\TestCoverage1\Cobertura.xml -source:C:\jenkins\workspace\TestCoverage1
```

---
## Jenkins Job 등록 시 참고사항

Jenkins에 Job으로 등록하면서 발생했던 이슈사항가 있었다.  
1. MSTest Permission Error발생
2. OpenCover로 Coverage 수집 시 아래와 같은 No_result Error
3. HTML Report등록 시 404 Can`t found Error


MSTest Permission의 경우 아래와 같은 Error가 발생했었다.  
![MSTest_Error](/assets/img/upload/testing/MSTest_Error.PNG)  
해당 메세지에 대해서 검색한 결과 Jenkins Global Tool Configuration에서 MSTest Path를 Full Path로 설정해 줘야 한다는 내용이었다. 나는 MSTest.exe가 위치한 Path까지만 지정했지만 실제로 파일명까지 기재해야 한다는 내용이었다. 따라서 아래와 같이 Full Path로 지정해준다.
![MSTest_Setting](/assets/img/upload/testing/MSTest_Setting.PNG)  

OpenCover로 Coverage 수집시 No_result Issue가 발생한 사례는 아래와 같았다.  
![OpenCover_result](/assets/img/upload/testing/OpenCover_result.PNG)  
검색결과 -register Option을 변경해주면 되는 문제였는데, cmd에서 수행할 때와 Jenkins에서 수행할 때, 다른 register값을 참고한는 것 같다. cmd에서는 x64로, jenkins는 x86을 참고하는 것 같은데 어쨋든 옵션을 `-register:user` -> `-register:path32`으로 변경하면 정상적으로 동작한다. 만약 이렇게도 변경이 안된다면 Register 참조 값을 직접 입력해야한다.
``` cmd
regsvr32 x86\OpenCover.Profiler.dll 
regsvr32 x64\OpenCover.Profiler.dll
```
Reference : https://github.com/OpenCover/opencover/issues/167  
    https://github.com/OpenCover/opencover/issues/562

마지막으로 ReportGenerator로 생성한 Report가 Jenkins에서 제대로 연결해주지 못하는 상황이 있었는데, 이는 File name을 `index.html`이 아닌 `index.htm`으로 지정해주면 해결되는 문제였다. 참고하면 좋을 것 같다.



