---
layout: post
category: DataStructure
title: Trie
tagline: by TinyBread
tags: [DataStructure]
---


<!--more-->



  
# Trie

인터넷 검색을 하다보면 자동완성기능을 볼수 있을 것이다. 이러한 자동완성 기능은 어떠한 자료구조를 이용하는 것일까? 라는 고민을 하게되었고 trie 자료구조를 알게되었다.

### Trie


여러개의 문자열을 비교할때 일일히 비교할수 있지만 이렇게 되면 매우 비효율적인 검색을 하게 된다.
개수가n이고 최대길이가 m인 어떤 문자열들의 집합에서 어떻나 문자열이 포함되있는지 비교하기 한다면 최대 O(mn)의 비교횟수가 필요하기 때문이다. 또한 정렬을 시킨뒤 이진탐색을 하여도 시간복잡도가 단축될수 있으나 정렬하는데 그만큼의 시간을 소비하게된다. 이러한 문제를 해결하기 위해 검색에 효울적인 trie구조를 사용한다. 

사전을 생각해보면 trie구조를 쉽게 이해 할수있다. apple라는 단어를 찾으려면 가장먼저 a를 찾을것이고 p p l e순으로 단어를 찾을것이다. 이것을 논리적인 자료구조로 만든 것이 trie구조이다.

### 특징  
* reTRIEval 에 유용하다고 하여 Fredkin이 명명하였다. 

* trie구조는 정렬된 트리 자료구조를 의미한다.

* 주로 스트링을 키로 가지는 동적집합 이나 연관 배열을 저장하는데 사용된다. 

* 이진 검색 트리와는 달리, 노드는 키를 갖지 않는 대신, 그 위치가 키 역할을 한다.

* 또한 root 노드는 값을 비어있는 형태이다.

* 특정 노드의 모든 자식노드는 그 노드의 동일한  prefix를 갖는다. 

* **trie 구조의 가장 큰 특징으로는 마지막 노드에 최종 데이터가 존재한다는 것이다.**

* **이러한 트라이구조는 찾고자 하는 문자열을 공간을 많이 사용하는 대신, 그 문자열의 길이의 속도만큼 초고속 탐색이 가능하다.**

* 문자열 집합의 개수와 상관없이 찾고자 하는 문자열의 길이가 시간복잡도가 된다.


### 구조


<img src="/assets/themes/Snail/img/DataStructure/Trie/trieExample.jpg" alt="">



* root 노드는 빈 노드이다.
* 간선은 prefix가 붙는 것을 의미한다.
* tree의 가장 마지막 노드가 의미있는 데이터가 된다.
* 부모노드와 간선문자를 이용해 자식노드를 나타낸다.

### Example

http://m.blog.naver.com/javaking75/140211950640
참고해서 코드만들기.


### 참고
* [https://namu.wiki/w/%ED%8A%B8%EB%9D%BC%EC%9D%B4 ](https://namu.wiki/w/%ED%8A%B8%EB%9D%BC%EC%9D%B4)

* [http://clojure.or.kr/wiki/doku.php?id=study:algorithms:trie](http://clojure.or.kr/wiki/doku.php?id=study:algorithms:trie)

* [http://m.blog.naver.com/javaking75/140211950640](http://m.blog.naver.com/javaking75/140211950640)