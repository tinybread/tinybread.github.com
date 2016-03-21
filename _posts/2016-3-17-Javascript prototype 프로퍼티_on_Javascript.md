---
layout: post
category: Javascript
title: Javascript prototype
tagline: by TinyBread
tags: [Javascript]
---


<!--more-->



  
# Javascript prototype 프로퍼티  


### Javascript prototype 프로퍼티?

자바스크립트는 Class가 존재 하지 않는다. 때문에 객체의 원형인 프로토타입을 이용하여 클로닝과 객체특성을 확장해 나가는 방식을 통해 새로운 객체를 생성해 낸다.

자바스크립트에서 프로토타입은 다른 객체의 기반이 되는 객체이다.
자바스크립트에서 함수 Function 이 중요한 이유는 사용자가 정의할 수 있는 프로토타입 속성 prototype property를 제공하는 유일한 객체이기 때문이다.

### Javascript prototype 프로퍼티를 쓰는이유? 
 
* 객체지향적이고 상속을 사용한다.

* 프로토타입 객체를 사용하면 객체가 프로토타입의 프로퍼티를 상당수 상속할 수 있어서 각 객체에 필요한 메모리의 양을 대폭 줄일 수 있다.




### prototype property
		function f(){}; 	//object
		var obj={};		    //undifined
		var array=[];		//undefined


* function이 사용자가 정의 할 수 있는 prototype property를 제공하는 유일한 객체

<br>

		f.prototype = {
			constructor : function f(){},
			__proto__ : Object		//Object.prototype
		}

contructor 에는 f함수가 생성될 때 사용된 함수 객체가 담겨 있다. __ proto __는 현재 프로토타입에 연결된 부모 객체이다.
파이어폭스, 크롬, 사파리는 이 속성을 __ proto __라는 이름으로 노출하여 개발자가 사용가능하나 다른 브라우저는 스크립트 이 property를 접근 할수 없다.


위의경우 Object.prototype을 상속받은 f는 아래의 메소드를 사용할 수 있다.

		f.toString();
		f.hasOwnProperty('');

### prototype chain 도식화  




### 참고
* [http://codingnuri.com/javascript-tutorial/prototypes.html](http://codingnuri.com/javascript-tutorial/prototypes.html)

* [http://beizix.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-Function-%EC%9D%98-%EB%B9%84%EB%B0%80-prototype-%EA%B3%BC-new](http://beizix.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-Function-%EC%9D%98-%EB%B9%84%EB%B0%80-prototype-%EA%B3%BC-new)