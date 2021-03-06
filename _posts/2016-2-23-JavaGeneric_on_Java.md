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


자바SE5 버전에서 가장 큰 특징으로 generic이다. 

기존의 컬렉션 클래스는 자료구조를 모델링한 클래스들이다. Generic의 도입이 가장 두드러지는 곳은 컨테이너(컬렉션)를 통해 객체를 관리 하는 부분이다.  Generic을 다른 방식으로 사용할 수도 있지만 컬랙션에서 사용함으로써 중요한 점은 형 안정성을 갖춘 코드를 만들수 있게되었다. 

Generic을 도입하기 이전까진 컨테이너에서 Object 타입으로 객체를 집어넣을 수 있었기 때문에 어떠한 객체를 넣어도 컴파일러에서 신경쓰지 않았다. (즉, ArrayList라는 컬랙션에 아무 객체를 넣어도 컴파일러에서 신경쓰지 않았다는것을 의미한다.) 저장할 데어터가 어떤 클래스 타입인지를 사전에 알 수 없기때문에 모든 자바 클래스의 최상위 클래스인 Object를 기준으로 데이터 타입을 지정했기 때문이다. 

기존에 컨테이너에 데이터를 get하게 되면 반환대는 객체는 모두 Object이기 때문에 명시적 형변환을 통해 개발자가 적절하게 타입을 캐스팅 해주어야했다. 이러한 명시적 형변환은 컴파일시에 컨테이너에 저장된 타입을 확인할 수 없었고 잘못 캐스팅하면 실행시에 ClassCastException과 같은 예외가 발생하였다.

그러나 Generic이 도입되면서 타입 안정성을 확보할 수 있게 되었고 문제를 실행 도중이 아닌(RuntimeException) 컴파일 시 발견 할수 있게 되었다.


### Example  
* **Generic클래스 정의 및 사용**
<br>일반적인 클래스를 정의하는 것과 동일하나 클래스명 뒤에 <T>와같이 명시해준다.<br><br>
	GenericExample.java

		package com.tinybread.genericExample;
		
		public class GenericExample<T> {
		
			private T value;
		
			public T getValue() {
				return value;
			}
		
			public void add(T value) {
				this.value = value;
			}
		
		}



	GenericExampleTest.java
	
		package com.tinybread.genericExample;
		
		import static org.junit.Assert.*;
		
		import org.junit.Test;
		
		public class GenericExampleTest {
		
			@Test
			public void test() {
				GenericExample<String>  example = new GenericExample<String>();
				example.add("test");
				assertEquals("test", example.getValue());
			
				//example.add(1);//error String Type으로 선언했기 때문
				
			}
		
		}


**Generic Type에 사용되는 파라미터**
<br>

*  E - Element (자바의 컬렉션에서 널리 사용되고 있다.)
*  K - Key
*  N - Number
*  T - Type
*  V - Value
*  S,U,V etc. - 2nd, 3rd, 4th types

### Generic 파라미터 타입 제한
'extends'키워드를 사용한다. U는 Number의 하위 클래스이다. (Number의 하위 클래스가 아닌 것을 제한)
		
		public <U extends Number> void print(U u){
				System.out.println(u.getClass().getName());
			}
  

<br>
조건이 여러개 일때는 &를 사용한다.
		
		<U extends Number & MyInterface>
  

### 와일드 카드 문자  

* <?> - 모든 객체 자료형, 내부적으로는 Object로 인식한다.
* <? super 객체자료형> - 명시된 객체자료형과 그 상위 객체, 내부적으로는 Object로  인식한다.
* <? extends 객체자료형> - 명시된 객체 자료형과 이를 상속한 하위객체, 내부적으로는 명시된 객체 자료형으로 인식한다.




### 참고
* Head First Java (한빛미디어 / 케이시 시에라, 버트 베이츠)
* Thinking In Java (SciTech / Bruce Eckel)
* Understanding of Java Programming(이한출판사 / 조성희)
* [http://onecellboy.tistory.com/64](http://onecellboy.tistory.com/64)

