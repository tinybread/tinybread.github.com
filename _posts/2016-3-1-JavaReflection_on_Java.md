---
layout: post
category: Java
title: Java Reflection
tagline: by TinyBread
tags: [Java]
---



<!--more-->

#  Java Reflection  

### Reflection? 
Java는 인터프리터와 컴파일방식으로 두가지로 동작한다. reflection은 인터프리터에서 발달한 개념이다. 

컴파일방식으로 동작하는 C를 예를들면, 컴파일 후 실행하면 메모리에는 객체의 정보를 갖고 있는 것이 아니라 그 객체를 주소값을 갖고있으므로 실행 시점에서 그 객체를 동적으로 생성할 수 없다.

그러나 java는 컴파일방식과 인터프리터 방식으로 동작하기에 Class Loader와 class의 정보를 갖고있는 java.lang.Class의 instance를 통해 메모리에 클래스정보를 객체로 갖고있다. 때문에 Class객체에선 클래스의 이름, 접근자, 메소드 등의 정보들을 파악할수 있게된다.

### Uses of Reflection 

* Extensibility Features
어플리케이션은 정규화된 이름(Fully-qualified)을 사용하여 확장 개체의 인스턴스를 생성하여 외부 사용자 정의 클래스를 사용할수 있다. 

* Class Browsers and Visual Development Environments


* Debuggers and Test Tools
* 




### Example  

<img src="/assets/themes/Snail/img/DataStructure/Treap/example1.png" alt="">  
  
 


# Code
* [네이버](http://www.naver.com)
  
<br>  

# 참고
* Head first

* [reflection tutorial](https://docs.oracle.com/javase/tutorial/reflect/)  

* [http://m.blog.naver.com/eugene82kr/30017317760](http://m.blog.naver.com/eugene82kr/30017317760)

http://www.hanbit.co.kr/network/view.html?bi_id=1370