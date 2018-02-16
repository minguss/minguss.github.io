---
layout: post
title:  "RewriteRule"
date:   2017-08-31 08:00:00
author: minguss
categories: devlog web middleware
tags: web
---

references : http://panocafe.tistory.com/entry/Apache-modrewrite-Introduction-%EC%95%84%ED%8C%8C%EC%B9%98-modrewrite-%EC%86%8C%EA%B0%9C

mod_rewrite 는 규칙 기반으로 URL 을 동적으로 전환(redirecting) 및 재작성(rewriting)할 수 있는 아파치 확장 모듈입니다. 이것을 사용해서 웹 서비스 운영시 유용하게 사용할 수 있습니다.

먼저 Rewrite Rule을 사용하기 위해서 RewriteEngine을 사용해야 합니다. Oracle Webtier 기준으로 mod_wl_ohs.conf에 Module내부에 RewriteEngine Flag를 On하여 Engine을 사용합니다.

잘 쓰면 유용하지만, 잘못쓰면 독이 되는 구문중에 하나입니다.  

mod_rewrite는 Perl Compatible Regular Expression 어휘를 사용한다.  어휘는 아래와 같다.

| Character (문자) |                                                                                           Meaning (의미)                                                                                          |                                                                                                Example (예제)                                                                                               |
|:----------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
|         .        |                                                                      Matches any single character (아무 단일 문자에 상응한다)                                                                     |                                                                   c.t will match cat, cot, cut, etc. (c.t는 cat, cot, cut 등에 상응한다.)                                                                   |
|         +        |                                                    Repeats the previous match one or more times. (문자가 한 번 또는 그 이상 상응함을 나타낸다.)                                                   |                                                                          a+ matches a, aa, aaa, etc (a는 a, aa, aaa등에 상응한다.)                                                                          |
|         *        |                                                     Repeats the previous match zero or more times (문자가 0번 또는 그 이상 상응함을 나타낸다)                                                     |                             a* matches all the same things a+ matches, but will also match an empty string. (a*는 a+에 상응하는 모든 문자에 상응하고 또한 빈 문자에도 상응한다.)                            |
|         ?        |                                                           Makes the match optional. (선택적 상응을 나타낸다. 즉, 있어도 되고 없어도 됨)                                                           |                                                                 colou?r will match color and colour. (colou?r는 color와 colour에 상응한다.)                                                                 |
|         ^        |                                                 Called an anchor, matches the beginning of the string (앵커라고 불리우며 스트링의 시작을 나타낸다)                                                |                                                                 ^a matches a string that begins with a (^a는 a로 시작하는 스트링에 상응한다)                                                                |
|         $        |                                                    The other anchor, this matches the end of the string. (스트링의 끝을 나타내는 또 다른 앵커)                                                    |                                                                  a$ matches a string that ends with a. (a$는 a로 끝나는 스트링에 상응한다)                                                                  |
|        ( )       | Groups several characters into a single unit, and captures a match for use in a backreference. (다수의 문자들을 하나의 유닛으로 묶는다. 역참조시 매칭되는 부분을 Substition(대체) 변수에 담긴다.) | (ab)+ matches ababab - that is, the + applies to the group. For more on backreferences see below. ( (ab)+는 ababab에 상응한다. 이 말은 +가 묶음에 적용된다는 뜻이다. backreference에 대한 설명은 아래 참고) |
|        [ ]       |                           A character class - matches one of the characters (문자 클래스 - 문자들 중 하나에 상응한다. 즉, []안에 있는 문자들 중 하나라도 있으면 상응함)                           |                                                                   c[uoa]t matches cut, cot or cat. ( c[uoa]t 는 cut, cot, cat에 상응한다)                                                                   |
|       [^ ]       |                      Negative character class - matches any character not specified (부정 문자 클래스 - 설정되지 않은 문자에 상응한다. 즉, ^뒤에 지정된 문자가 아니면 상응함)                     |                                                      c[^/]t matches cat or c=t but not c/t ( c[^/]는 cat이나 c=t에 상응하지만 c/t에는 상으하지 않는다)                                                      |

    
mod_rewrite에서 !문자는 정규식 앞에 붙이면 반대된다는 의미입니다. 즉, 스트링이 정규식에 상응하지 않을 때 정규식 앞에 !을 붙여서 상응되도록 합니다.  

---
###Regex Back-Reference Availability
Pattern이나 CondPattern에서 둥근 괄호를 사용할 때, $N, %N 스트링과 함께 사용될 수 있는 역 참조가 내부적으로 생성된다.  
![The back-reference flow through a rule](http://httpd.apache.org/docs/2.4/en/images/rewrite_backreferences.png)  

---
###RewriteRules Basics
1. 패턴(Pattern) : 룰에 의해서 변경되어질 입력 URL
2. 대체(Substitution) : 어떻게 재작성될지에 대한 정의
3. 신호[flags] : Rewriten Request에 영향을 행사하는 옵션  

![RewriteRule 작성법](http://cfile27.uf.tistory.com/image/01579E335072C7F1270469)