---
layout: post
title:  "Hashmap-HashTable"
subtitle:   "Hashmap-HashTable"
categories: devlog
tags: java
---


>출처 :  
[6 Difference Between HashMap And HashTable : Popular Interview Question In Java With Example](http://javahungry.blogspot.com/2014/03/hashmap-vs-hashtable-difference-with-example-java-interview-questions.html),  
[멋진태혁님 블로그](http://blog.naver.com/PostView.nhn?blogId=sthwin&logNo=220825616965&parentCategoryNo=&categoryNo=4&viewDate=&isShowPopularPosts=true&from=search)


> Synchronization or Thread Safe :  This is the most important difference between two . HashMap is non synchronized and not thread safe.On the other hand, HashTable is thread safe and synchronized.
When to use HashMap ?  answer is if your application do not require any multi-threading task, in other words hashmap is better for non-threading applications. HashTable should be used in multithreading applications. 


Synchronization or Thread Safe 은 둘 사이의 가장 중요한 차이점이다. HashMp 은 비동기화 되어 있으며 쓰레드에 안전하지 않지만 HashTable 은 쓰레드에 안전하며 동기화 되어 있다. 
그럼 언제 HashMap을 사용할까? 애플리케이션이 여러개의 쓰레드 작업을 필요로 하지 않으면 사용한다. HashMp 은 쓰레드를 이용하지 않는 애플리케이션에 사용하기에 더 낫다. HashTable 은 멀티쓰레드를 사용하는 애플리케이션용으로 사용하는 것이 좋다. 

> Null keys and null values :  Hashmap allows one null key and any number of null values, while Hashtable do not allow null keys and null values in the HashTable object.

HashMap 은 하나의 null 키와 값에는 몇번인들 null 값을 넣을 수 있다. HashTable 은 null 키와 값에 null 값을 넣을 수 없다.  

>  Iterating the values:  Hashmap object values are iterated by using iterator .HashTable is the only class other than vector which uses enumerator to iterate the values of HashTable object.


HashMap 객체들을 반복적으로 가져오기 위해 iterator를 사용하지만 HashTable 은 vector 와는 달리 enumerator를  사용하는 유일한 클래스다.

![hashtable](http://3.bp.blogspot.com/-BvvI4qSJ5gs/UymE9OXgBGI/AAAAAAAAASA/yXv2COAHm_U/s1600/difference+between+hashmap+and+hashtable.jpg)

> Fail-fast iterator  : The iterator in Hashmap is fail-fast iterator while the enumerator for Hashtable is not.
According to Oracle Docs,  if the Hashtable is structurally modified at any time after the iterator is created in any way except the iterator's own remove method , then the iterator will throw ConcurrentModification Exception. 
Structural modification means adding or removing elements from the Collection object (here hashmap or hashtable) . Thus the enumerations returned by the Hashtable keys and elements methods are not fail fast.We have already explained the difference between iterator and enumeration.

Hashmap 의 iterator 는 fail-fast iterator 라면 Hashtable 의 enumerator 는  fail-fast iterator 가 아니다. 오라클 문서에 따르면, iterator 가 생성된 이후  iterator 자신의 제거 메소드를 제외하고는 Hashtable 이 구조적으로 수정되면 iterator 는 ConcurrentModification Exception 을 던질 것이다. 구조적인 수정이란 Collection (hashtable, hashmap) 객체에서 요소들을 추가하거나 제거한다는 것을 의미한다. 그래서 Hashtable 키를 이용하여 반환받은 enumeration 이나 element 들의 메소드들은 fail-fast가 아니다. 우리는 이미 iterator 와 enumeration 의 차이를 설명했다.

> Performance :  Hashmap is much faster and uses less memory than Hashtable as former is unsynchronized . Unsynchronized objects are often much better in performance in compare to synchronized  object like Hashtable in single threaded environment.

Hashmap 이 unsynchronized 이기 때문에 Hashtable 보다 훨씬 빠르고 메모리도 조금 사용 한다. Unsynchronized 객체들이 보통 단일 쓰레드 환경에서의 Hashtable 같은 synchronized 객체에 비해 성능상 더 낫다. 

> Superclass and Legacy :  Hashtable is a subclass of Dictionary class which is now obsolete in Jdk 1.7 ,so ,it is not used anymore.
It is better off externally synchronizing a HashMap or using a ConcurrentMap implementation (e.g ConcurrentHashMap).HashMap is the subclass of the AbstractMap class. Although Hashtable and HashMap has different superclasses but they both are implementations of the "Map"  abstract data type.

Hashtable 은 현재 Jdk 1.7 에서 잘 사용되고 있지
않은 Dictionary 의 서브 클래스이다 그래서 더이상 사용 되지 않는다.
HashMap 을 명시적으로 동기화하거나 ConcurrentMap 을 구현하는 것이 더 낫다(ex) ConcurrentHashMap). HashMap 은 AbstractMap 클래스의 서브클래스다. Hashtable 과 HashMap 이 서로 다른 부모 클래스를 갖고 있지만 둘다 "Map" 이라는 추상 데이터 타입을 구현한다.


Example of HashMap and HashTable
``` java
import java.util.Hashtable;


public class HashMapHashtableExample {
    
    public static void main(String[] args) { 
 
           
  
        Hashtable<String,String> hashtableobj = new Hashtable<String, String>();
        hashtableobj.put("Alive is ", "awesome");
        hashtableobj.put("Love", "yourself");
        System.out.println("Hashtable object output :"+ hashtableobj);
 
         
 
        HashMap hashmapobj = new HashMap();
        hashmapobj.put("Alive is ", "awesome");  
        hashmapobj.put("Love", "yourself"); 
        System.out.println("HashMap object output :"+hashmapobj);   
 
 }
}
```

`Output` :  

        Hashtable object output :{Love=yourself, Alive is =awesome}
        HashMap object output :{Alive is =awesome, Love=yourself}

### Similarities Between HashMap and Hashtable

>Insertion Order :   Both HashMap and Hashtable  does not guarantee that  the order of the map will remain constant over time. Instead use LinkedHashMap, as the order remains constant over time.

HashMap 과 Hashtable 은 둘다 사용되는 동안 맵의 순서를 보장하지 않는다. 순서 보장을 원하면 LinkedHashMap 를 사용하라.

>Map interface :   Both HashMap and Hashtable implements Map interface .

둘다 Map 인터페이스를 구현한다.

>Put and get method :  Both HashMap and Hashtable provides constant time performance for put and get methods assuming that the objects are distributed uniformly across the bucket. 

객체들이 버켓에 전체적으로 고르게 분포되어 있다고  가정했을때 put 과 get 메소드들에 대해서 둘다 일정한 시간성능을 제공한다. 

>Internal working :  Both HashMap and Hashtable works on the Principle of Hashing . We have already discussed how hashmap works in java .

둘다 해싱 원리에 기반하여 동작한다. 

When to use HashMap and Hashtable?

>Single Threaded Application  
>HashMap should be preferred over Hashtable for the non-threaded applications. In simple words , use HashMap in unsynchronized or single threaded applications .

쓰레드를 사용하지 않는 애플리케이션들에 대해서는 HashMap 사용이 선호된다. 간단히 말해서 unsynchronized 이거나 단일 쓰레드 애플리케이션에서는 HashMap 일 사용해라.

>Multi Threaded Application  
We should avoid using Hashtable, as the class is now obsolete in latest Jdk 1.8 . Oracle has provided a better replacement of Hashtable named ConcurrentHashMap. For multithreaded  application prefer ConcurrentHashMap instead of Hashtable.

최신 자바 Jdk 1.8 에서는 현재 Hashtable 클래스가 구식클래스로 되어 있으므로 사용을 안하는 것이 낫다. 오라클은  ConcurrentHashMap 라는 Hashtable 의 더 나은 대체제를 제공하고 있다. 멀티 쓰레드 애플리케이션에 대해서는 Hashtable 보다 ConcurrentHashMap 을 선호한다. 

### Recap  : Difference between HashMap and Hashtable in Java
![different](http://postfiles9.naver.net/20161001_88/sthwin_1475317433468Pubvr_JPEG/ggg.JPG?type=w773)






