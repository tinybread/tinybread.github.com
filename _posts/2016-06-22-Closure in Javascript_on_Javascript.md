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
        

### 참조
- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures)
- [https://blog.outsider.ne.kr/506](https://blog.outsider.ne.kr/506)