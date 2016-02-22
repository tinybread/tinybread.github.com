---
layout: post
category: DesignPattern
title: Template Callback
tagline: by TinyBread
tags: [DesignPattern]
---


<!--more-->

  
# Template Callback
File, DB를 활용하여 IO작업을 하다보면 (mybatis와 같은 sql 맵퍼 프레임워크를 사용하지 않고) 쿼리의 내용만 변하고 try-catch-finally의 변하지 않는 일정한 작업이 존재하게 된다.

이처럼 복잡하지만 바뀌지 않는 작업의 흐름과 그중 일부만 자주 바꿔서 사용하는 구조에 아래와 같은 방식을 사용할 수 있다.
전략패턴의 기본구조에 익명의내부클래스를 활용한 방식이다.
이러한 방식을 스프링에서는 템플릿/콜백 패턴이라고 부른다.


### Template Callback pattern?     

먼저, 전략패턴이란 OCP관점에서 보면 확장에 해당하는 변하는 부분을 별도의 클래스로 만들어 추상화된 인터페이스를 통해 위임하는 방식이다. 
아래의 이미지를 보면 클라이언트에서 전략을 제공하고 
컨텍스트라는 클래스를 통해서 변하지 않는 부분을 추출 후  동적으로 변하는 부분에 대해 각각의 알고리즘을 교체하여 사용하는 방식이다. 

<img src="/assets/themes/Snail/img/DesignPattern/TemplateCallback/strategyUML.jpg" alt="">
출처 : 토비의 스프링 3.1 (그림 3-3 전략 패턴에서 client의 역할)

<br><br>
템플릿/콜백이란  
이와 같은 전략패턴의 컨텍스트를 템플릿이라 부르고, 익명 내부클래스로 만들어지는 오브젝트(변하는부분)를 콜백이라 부른다.<br>

*  콜백 : 실행되는 것을 목적으로 다른 오브젝트의 메소드에 전달되는 오브젝트, 파라메터로 전달되지만 값 참조가 아닌, 로직을 전달하기 위해 사용(자바에선 메소드 자체를 파라미터로 전달할 방법이 없기 때문)

아래의 이미지는 템플릿/콜백의 일반적인 작업흐름을 나타낸다.
<img src="/assets/themes/Snail/img/DesignPattern/TemplateCallback/templateCallback.jpg" alt="">
출처 : 토비의 스프링 3.1 (그림 3-7 템플릿/콜백의 작업흐름)<br><br>
클라이언트에서 템플릿안에 실행될 로직을 담은 콜백 오브젝트를 만들고 이것을 템플릿에게 참조할 정보로 제공한다.<br>
템플릿은 정해진 작업을 진행하다 내부에서 생성된 콜백오브젝트의 메소드를 호출한다. 콜백은 클라이언트 메소드에 있는 정보와 템플릿에 제공한 정보를 이용하여 작업을 수행후 템플릿에게 결과를 돌려준다.<br>
템플릿은 콜백이 돌려준 값을 활용해 작업을 수행후 결과를 클라이언트에 전달해 준다.<br><br>

이러한 방법을 이용하여 DAO클래스(클라이언트)에서 try-catch-finally의 부분을 템플릿클래스로 만들고 호출, 익명의 내부클래스를 활용하여 쿼리문 작성(템플릿 클레스에게 전달)하여 구현할 수 있다. 

### Example       
<img src="/assets/themes/Snail/img/DesignPattern/TemplateCallback/client.PNG" alt="">
<img src="/assets/themes/Snail/img/DesignPattern/TemplateCallback/context.PNG" alt="">


<br>  

# Code  
추후 첨부  
<br>  

# 참고  
* 토비의 스프링 3.1 (에이콘/이일민) 