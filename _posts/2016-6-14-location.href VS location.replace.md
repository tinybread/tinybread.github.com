---
layout: post
category: Javascript
title: location.href VS location.replace
tagline: by TinyBread
tags: [Javascript]
---

<!--more-->


  
# location.href VS location.replace


<img src="/assets/themes/Snail/img/Javascript/href_VS_replace/href와replace차이.JPG" alt="">

replace 와 href는 흔히 페이지를 변경하고자 할때 사용한다. 그러나 이 둘사이에는 분명한 차이점이 존재하며 이를 적절하게 알고 사용해야한다.

개념의 차이
href : 속성
<br>repalce : 메소드

기능의 차이
href : 페이지를 이동시킨다.
<br>replace : 현재 페이지를 바꿔준다 (기존페이지를 새로운 페이지로 변경)

<br>
=> href는 페이지를 이동하면서 history를 남겨준다 즉 a->b페이지 이동 후 뒤로가기를 통해 a로 돌아갈 수 있다는 것을 의미한다.
반면, replace는 history를 남기지 않기 때문에 a->b이동 후 뒤로가기를 통해 돌아갈 수 없다. 또한 replace는 캐시(인터넷 임시파일을) 사용하지 않다는 점에서 href와 다르다.





### 참고
* [http://roresy.tistory.com/1921 ](http://roresy.tistory.com/1921)

