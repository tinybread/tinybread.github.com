---
layout: post
category: Spring
title: Spring Web MVC Flow
tagline: by TinyBread
tags: [Spring]
---


<!--more-->



  
# Spring Web MVC Flow
Spring Web MVC framework는 model-view-controlelr의 구조를 제공한다.
또한, 유연하고 느슨하게 결합된 web applications을 개발하기 위해 준비된 컴포넌트들을 제공한다.

MVC 패턴은 이러한 요소 간의 느슨한 결합을 제공하면서, application의 서로 다른 측면 (input logic, business logic, and UI logic)으로 분리를 초래한다.


* Model : application data를 캡슐화 하고 일반적으로 POJO로 구성되어 있다.

* View : Model data를 렌더링하고, 일반적으로는 클라이언트의 브라우저가 해석할 수 있는 HTML을 생성한다.

* Controller : 적절한 Model을 통해 사용자의 요청을 처리하고 뷰에 결과를 전달한다.

### DispatcherServlet
Spring MVC framework는 모든 HTTP 요청 및 응답을 처리하는 DispatcherServlet을 중심으로 설계된다.


<img src="/assets/themes/Snail/img/Spring/SpringWebMVCFlow/springWebMVCFlow.png" alt="">


### Spring MVC Flow ###

* (HttpRequest의 요청)최초 진입은 DispatcherServlet이 담당한다.

* Handler Mapping을 통해 Controller를 찾음

* Controller 호출하여 작업 처리(Get/Post에 따라 해당메소드 실행)
 * Controller에 의해 Service실행

* Service 실행 후 ModelAndView 인스턴스 반환

* ViewResolver 참조하여 실질적인 View 생성

* 클라이언트에게 원하는 UI 제공


### Required Configuration
Spring Web MVC를 사용하기 위해 config는 아래와 같은 방법으로 할 수 있다.

1. xml을 사용하여 application context 설정을 할 수 있다.(root-context.xml, servlet-context.xml)

2.annotation을 이용해 java based의 설정을 할 수 있다.(servlet 3.0이상 가능)

* (@Configuration, @EnableWebMvc, @EnableAsync, @ComponentScan,WebApplicationInitializer ...)

### 참고
* [https://namu.wiki/w/%ED%8A%B8%EB%9D%BC%EC%9D%B4 ](https://namu.wiki/w/%ED%8A%B8%EB%9D%BC%EC%9D%B4)

* [http://clojure.or.kr/wiki/doku.php?id=study:algorithms:trie](http://clojure.or.kr/wiki/doku.php?id=study:algorithms:trie)

* [http://m.blog.naver.com/javaking75/140211950640](http://m.blog.naver.com/javaking75/140211950640)

* [http://algs4.cs.princeton.edu/lectures/52Tries.pdf](http://algs4.cs.princeton.edu/lectures/52Tries.pdf)