---
layout: post
category: Java
title: Java Generic
tagline: by TinyBread
tags: [Java]
---


<!--more-->



  
# Java Generic


### Generic?



**Generic?**

자바SE5 버전에서 가장 큰 특징으로 generic이다.<br>

Generic의 도입이 가장 두드러지는 곳은 컨테이너를 통해 객체를 관리 하는 부분이다. Generic을 다른 방식으로 사용할 수도 있지만 컬랙션에서 사용함으로써 중요한 점은 형 안정성을 갖춘 코드를 만들수 있게되었다. 

Generic을 도입하기 이전까진 컨테이너에서 Object 타입으로 객체를 집어넣을 수 있었기 때문에 어떠한 객체를 넣어도 컴파일러에서 신경쓰지 않았다. (즉, ArrayList라는 컬랙션에 아무 객체를 넣어도 컴파일러에서 신경쓰지 않았다는것을 의미한다.)

그러나 Generic이 도입되면서 타입 안정성을 확보할 수 있게 되었고 문제를 실행 도중이 아닌(RuntimeException) 컴파일 시 발견 할수 있게 되었다.


### 간단한 Generic
### Generic Interface
### Generic method
### 와일드 카드 문자
### Generic의 제약


### 참고
* Head First Java (한빛미디어 / 케이시 시에라, 버트 베이츠)
* Thinking In Java (SciTech / Bruce Eckel)
