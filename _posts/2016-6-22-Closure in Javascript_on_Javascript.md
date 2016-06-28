---
layout: post
category: Javascript
title: Closure in Javascript
tagline: by TinyBread
tags: [Javascript]
---


<!--more-->

  

# Closure in Javascript


### Closure?

- 클로저는 자신의 scope 밖에 있는 변수들에 접근 할 수 있는 함수를 의미한다.
- 로컬 변수를 참조하고 있는 함수 내의 함수



### Code
        function init() {
          var name = "클로저"; // init에 있는 지역 변수 name
          function displayName() { // 내부 함수, 즉 클로저인 displayName()
            alert(name); // 부모 함수에 정의된 변수를 사용한다
          }
          displayName();
        }
        init();
        
        
- 함수 init()은 지역변수 name, 함수 displayName()을 정의하고 있다.
- displayName()은 init()안에 정의되어 있는 내부함수 이다. 따라서, 같은 scope인 name에 접근이 가능하다.
- displayName()자체에 name이라는 지역 변수를 갖고 있지 않지만, init에 정의된 변수에 접근하는 권한이 있기때문에 부모 함수에 있는 변수 name을 사용할 수 있다.
        

### closure를 이용해서 private 함수 흉내내기

        var counter = (function() {
          var privateCounter = 0;
          function changeBy(val) {
            privateCounter += val;
          }
          return {
            increment: function() {
              changeBy(1);
            },
            decrement: function() {
              changeBy(-1);
            },
            value: function() {
              return privateCounter;
            }
          }   
        })();

        console.log(counter.value()); // logs 0
        counter.increment();
        counter.increment();
        console.log(counter.value()); // logs 2
        counter.decrement();
        console.log(counter.value()); // logs 1
        
이 예제에서는 하나의 환경을 counter.increment, counter.decrement, counter.value 세 함수가 공유한다.

공유되는 환경은 정의되자마자 실행되는 익명 함수 안에서 만들어진다. 이 환경에는 두 개의 private 아이템이 존재한다. 하나는 privateCounter라는 변수이고 나머지 하나는 changeBy라는 함수이다. 이 두 아이템 모두 익명함수 외부에선 접근할 수 없다. 하지만 익명함수 안에 정의된 세개의 public 함수에서 사용되고 반환된다.

이 세개의 public 함수는 같은 환경을 공유하는 클로져이다. 자바스크립트 어휘 스코핑(lexical scoping) 덕분에 세 함수 모두 privateCounter 변수와 changeBy 함수에 접근할 수 있다.

익명 함수가 카운터를 정의하고 이것을 counter 변수에 할당한다는 걸 알아차렸을 것이다. 이 함수를 다른 변수에 저장하고 이 변수를 이용해 여러개의 카운터를 만들수도 있다.


### closure 단점 & 성능
- 메모리를 소모한다.
- scope 생성에 따른 퍼포먼스 손해가 있다.

특히, 메모리의 소비는 return하거나, timer, callback 등으로 등록했던 함수들이 메모리에 계속 남아있게 되면 해당하는 closure도 같이 메모리에 계속 남아있게 되기때문이다.
구 버전의 IE에서는 DOM의 콜백함수로 등록을 하고 콜백함수의 해제없이 바로 DOM을 삭제해버리면 메모리 누수가 생기는 단점이 있었다.
또한, 하나의 새로운 scope를 생성하여 내부의 함수에 링크를 시키기 때문에 이에 따른 퍼포먼스 손해를 감수해야 한다.

** 단점을 알고 사용하자!


### 참조
- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures)
- [https://blog.outsider.ne.kr/506](https://blog.outsider.ne.kr/506)
- [http://unikys.tistory.com/308](http://unikys.tistory.com/308)