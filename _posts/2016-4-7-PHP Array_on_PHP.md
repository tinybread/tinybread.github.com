---
layout: post 
category: PHP
title: PHP Array
tagline: by TinyBread
tags: [PHP]  
---


<!--more-->


  

# PHP Array







### PHP 배열  
### 배열 연산 
배열의 연산에서 특이 하게 ‘같다’ 라는 의미는 두 가지 이다. 하나는 같은 원소를 갖고 있는 것이고 다른 하나는 같은 원소와 그 순서까지 동일한 경우이다.
원소(key, value)
 
<img src="/assets/themes/Snail/img/Other/phpArray/array.PNG" alt="">  
  

[배열의 연산자]
Ex)배열 비교 1
<?
	$a = array(1, 2);
	$b = array(2, 1);
	if ($a == $b) {
		Echo ‘$a와 $b는 같은 원소를 갖는다’;
	}
	else { 
		‘$a와 $b는 다른 원소를 갖는다’;
	}
>
결과 : 다른 원소를 갖는다.
배열의 원소가 같은 것이 아니라 원소의 값이 같을 뿐 키가 다르기 떄문이다.

Ex)배열 비교 2
<?
	$a = array(1, 2);
	$b = array(1 => 2, 0 => 1);
	if ($a == $b) {
		Echo ‘$a와 $b는 같은 원소를 갖는다’;
	}
	else { 
		‘$a와 $b는 다른 원소를 갖는다’;
	}
>
결과 : 같은 원소를 갖는다.
단, 인덱스가 1,0 순으로 저장 될거 같으나 실제로는 입력한 순서 그대로 저장되어ㅣ버림 따라서 ‘===’ 연산으로 비교시 동일하지 않음으로 나옴


Ex)배열 비교 2
<?
	$a = array(1, 2);
	$b = array(1 => 2, 0 => 1);
	if ($a === $b) {
		Echo ‘$a와 $b는 같은 원소를 갖는다’;
	}
	else { 
		‘$a와 $b는 다른 원소를 갖는다’;
	}
>
 결과 :  다른 원소를 갖는다. **php에서 배열의 인덱스가 반드시 순서대로 저장되는 것은 아니다.**


### 배열 정렬
 
<img src="/assets/themes/Snail/img/Other/phpArray/sort.PNG" alt="">  
  
**natural order??**
문자열 정렬을 할 떄 숫자 정렬이 제대로 되지 않는 것을 보완한 것
Ex) apple1, apple2,…apple11, apple12
sort() => apple1, apple11, apple12, apple2….
natsort() => apple1,apple2,…apple11,apple12..







