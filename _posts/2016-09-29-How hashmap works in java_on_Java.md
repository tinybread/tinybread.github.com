---
layout: post
category: Java
title:  How hashmap works in java
tagline: by TinyBread
tags: [Java]
---
<!--more-->

### Java HashMap은 어떻게 동작하는가?

HashMap은 java Collections Framework에 속한 구현체 클래스이다. Map인터페이스 자체는 Java5에서 Generic이 적용된 것외에 처음 선보인 이후 변화가 없지만, HashMap 구현체는 성능을 향상시키기 위해 지속적으로 변화해 왔다.

어떤방식으로 HashMap 구현체의 성능을 향상시키고 충돌 가능성을 줄이는지에 대해 알아보았다.


### HashMap VS HashTable

HashMap과 HashTable은 Java의 API 이름이다. HashTable이란 JDK 1.0부터 있던 Java의 API이고, HashMap은 Java 2에서 처음 선보인 Java Collections Framework에 속한 API다. 
HashTable 또한 Map 인터페이스를 구현하고 있기 때문에 HashMap과 HashTable이 제공하는 기능은 같다. 

다만,HashMap은 보조 해시 함수(Additional Hash Function)를 사용하기 때문에 HashTable에 비해 충돌이 덜 발생하여 성능상의 이점이 있다.
또한, HashTable의 경우는 구현에 거의 변화가 없는 반면 HashMap은 지속정으로 개선되고있다.(HashTable은 JRE1.0,1.11 환경을 대상으로 구현되어있는 App과의 호환성을 제공하는 것에 가치를 둠)


** HashMap과 HashTable을 정의한다면, '키에 대한 해시 값을 사용하여 값을 저장하고 조회하며, 키-값 쌍의 개수에 따라 동적으로 크기가 증가하는 associate array'라고 할 수 있다. **
이 associate array를 지칭하는 다른 용어가 있는데, 대표적으로 Map, Dictionary, Symbol Table 등이다.
		
		
	public class 8ccce55530bc3477c678dd9921b60f3e.gifHashtable<K,V> extends Dictionary<K,V>  implements Map<K,V>, Cloneable, java.io.Serializable {
		...
		}

	public class 928b3cc3fe40d69cd06cbe7f5f3767f8.gifHashMap<K,V> extends AbstractMap<K,V>  implements Map<K,V>, Cloneable, Serializable {
				...
		}
		
		* HashTable과 HashMap의 선언부


key와 value를 지칭하기위해 HashTable에는 Dictionary라는 이름을 사용하고, HashMap에서는 그 명칭이 그대로 말하듯이 Map이라는 용어를 사용하고 있다.



### 해시 분포와 해시 충돌

동일하지 않은 어떤 객체 X와 Y가 있을 때, 즉 X.equals(Y)가 '거짓'일 때 X.hashCode() != Y.hashCode()가 같지 않다면, 이때 사용하는 해시 함수는 완전한 해시 함수(perfect hash functions)라고 한다

Boolean같이 서로 구별되는 객체의 종류가 적거나, Integer, Long, Double 같은 Number 객체는 객체가 나타내려는 값 자체를 해시 값으로 사용할 수 있기 때문에 완전한 해시 함수 대상으로 삼을 수 있다. 하지만 String이나 POJO(plain old java object)에 대하여 완전한 해시 함수를 제작하는 것은 사실상 불가능하다.


HashMap은 기본적으로 각 객체의 hashCode() 메서드가 반환하는 값을 사용하고, 결과 자료형은 int다.
32비트 정수 자료형으로는 완전한 자료 해시 함수를 만들 수 없다. 
논리적으로 생성 가능한 객체의 수가 2^32보다 많을 수 있기 때문이며, 또한 모든 HashMap 객체에서 O(1)을 보장하기 위해 랜덤 접근이 가능하게 하려면 원소가 2^32인 배열을 모든 HashMap이 가지고 있어야 하기 때문이다.

따라서 해시함수를 이용하는 associative array구현체에서 메모리를 절약하기 위해 실제 해시함수의 표현범위보다 작은 M개의 원소가 있는 배열만 사용한다. 따라서 아래와 같이 해시버킷 인덱스값을 나머지 값을 이용하여 M개의 원소가 있는 배열만 사용한다.
		int index = X.hashCode() % M;  

이와 같은 방식을 사용하면 서로다른 객체가 1/M확률로 버킷을 사용하게 된다.또한 이 해시함수가 충돌이 발생했을때, key-value 데이터를 잘 저장하고 조회할수 있는 대표적인 방식은 ** Open Addressing,  Separate Chaining이 있다.  **


* Open Addressing
	데이터를 삽입하려는 해시 버킷이 이미 사용 중인 경우 다른 해시 버킷에 해당 데이터를 삽입하는 방식이다. 데이터를 저장/조회할 해시 버킷을 찾을 때에는 Linear Probing, Quadratic Probing 등의 방법을 사용한다.

* Separate Chaining
	각 배열의 인자는 인덱스가 같은 해시 버킷을 연결한 링크드 리스트의 첫 부분(head)이다.



위의 두가지 방법다 Worst Case O(M)이다.
Open Addressing은 연속된 공간에 데이터를 저장하기 때문에 Separate Chaining에 비하여 캐시 효율이 높다.( 데이터 개수가 충분히 적다면 Open Addressing이 Separate Chaining보다 더 성능이 좋다.)
하지만 배열의 크기가 커질수록(M 값이 커질수록) 캐시 효율이라는 Open Addressing의 장점은 사라진다. 배열의 크기가 커지면, L1, L2 캐시 적중률(hit ratio)이 낮아지기 때문이다.

Java의 HashMap에서 사용하는 방식은  Separate Channing이다 
HashMap의 경우 remove()메소드가 빈번하게 호출되기 때문이다.(Open Addressing은 데이터를 삭제할 때 효율적 처리가 어려움)
또한 key-value 값이 많아지면, Open Addressing의 경우 해시 버킷을 채운 밀도가 높아지기에 worst case발생 빈도가 높아진다.
Separate Chaining 방식의 경우 해시 충돌이 잘 발생하지 않도록 '조정'한다면 Worst case가 줄어들 수 있다.



### Java 8 HashMap에서의 Separate chaining

Java8의 경우 이전버전에 과 달리 HashMap의 알고리즘이 많이 달라졌다.

Java8 이전에는 객체의 해시 함수값이 균등분포라 할때, get()메소드 호출에 대한 기댓값은  E(N/M)이었다면 8에서는 이보다 더 나은 E(logN/M)을 보장한다.
데이터의 개수가 많아지면 Separate Chaining에서는 링크드 리스트 대신 트리를 사용하기 때문이다.

링크드 리스트를 사용할 것인가 , 트리를 사용할 것인가에 대한 기준은 해시 버킷에 key-value 값이 8개에 이르면 트리로 변경한다.
만약 삭제하여 6개에 이르면 다시 링크드 리스트로 변경한다. 


트리는 링크드 리스트보다 메모리 사용량이 많고, 데이터의 개수가 적을 때 트리와 링크드 리스트의 Worst Case 수행 시간 차이 비교는 의미가 없기 때문이다. 또한 8,6으로 차이를 둔것은,
차이가 두 수의 차이가 1일경우 key-vaule 쌍이 반복되어 삽입/삭제되는경우 불필요하게 변경되는것을 방지하기 위함이다.(성능저하)


또한 Java8에서는 Enntry클래스 대신 Node클래스를 사용한다. Node는 Java7에서 Entry클래스와 같지만 트리를 사용하기 위해 Node를 사용한것이다.
이때 사용되는 트리는 Red-Black Tree로 TreeMap과 구현이 거의 같다. 트리 순회시 사용하는 대소 판단 기준은 해시 함수이다. 해시 값을 대소 판단 기준으로 사용하면 Total Ordering에 문제가 생기는데, Java 8 HashMap에서는 이를 tieBreakOrder() 메서드로 해결한다. 



### 해시버킷 동적할당

해시 버킷의 수가 작고 데이터가 많다면 그만큼 충돌의 발생 확률이 높아진다. 따라서 HashMap은 데이터가 일정 개수이상이 되면 해시 버킷의 갯수를 두배로 늘린다. 이렇게 해시 버킷 개수를 늘리면 N/M만큼 충돌로 인한 성능 손실문제를 해결할 수 있다.
해시 버킷의 기본값은 16이고 데이터의 개수가 임계점에 도달하면 두배로 증가한다. 최대의 수는 2^30이다.
이렇게 두배씩 증가할때마다 발생하는 문제는 Separate Chaining을 새롭게 구성해야 하는문제가 있다. 따라서 데이터의 수가 예측 가능하다면 HashMap의 생성자에서 초기 인자로 해시버킷 개수를 지정하면 된다.

해시 버킷 크기를 두 배로 확장하는 임계점은 현재의 데이터 개수가 'load factor * 현재의 해시 버킷 개수'에 이를 때이다. 이 load factor는 0.75 즉 3/4이다. 이 load factor 또한 HashMap의 생성자에서 지정할 수 있다.
즉 기본 생성자로로 생성한 HashMap을 이용하여 많은 양의 데이터를 삽입할 때에는, 최적의 해시 버킷 개수를 지정한 것보다 약 2.5배 많이 키-값 쌍 데이터에 접근해야 한다. 이는 곧 수행 시간이 2.5배 길어진다고 할 수 있다. 
따라서 성능을 높이려면, HashMap 객체를 생성할 때 적정한 해시 버킷 개수를 지정해야 한다.


이때 문제가 발생하게 되는데, 버킷개수 M이 2^a 형태가 되기 때문에,  index = X.hashCode() % M을 계산시, X.hashCode()의 하위 a개의 비트만 사용하게 된다. 
즉 해시 함수가 32비트 영역을 고르게 사용하도록 만들었다 하더라도 해시 값을 2의 승수로 나누면 해시 충돌이 발생할 수 있다.
이 때문에 보조 해시 함수가 필요하다.



### 보조 해시함수

index = X.hashCode() % M을 계산할 때 사용하는 M 값은 소수일 때 index 값 분포가 가장 균등할 수 있다.
그러나 M값이 소수가 아니기때문에 index값이 균등하지 않을수 있다. 
따라서 보조 해시 함수는 index 값을 되도록이면 균등하게 하기위해 사용된다.
Java5-7은 같은 방식의 보조 해시 함수를 사용하고 Java8부터는 새롭게 변형되었다.


	*Java7에서의 보조 해시 함수

	final int hash(Object k) {  
        // Java 7부터는 JRE를 실행할 때, 데이터 개수가 일정 이상이면
        // String 객체에 대해서 JVM에서 제공하는 별도의 옵션으로
        // 해시 함수를 사용하도록 할 수 있다.
        // 만약 이 옵션을 사용하지 않으면 hashSeed의 값은 0이다.
        int h = hashSeed;
        if (0 != h && k instanceof String) {
            return sun.misc.Hashing.stringHash32((String) k);
        }
        h ^= k.hashCode();
        // 해시 버킷의 개수가 2a이기 때문에 해시 값의 a비트 값만을 
        // 해시 버킷의 인덱스로 사용한다. 따라서 상위 비트의 값이 
        // 해시 버킷의 인덱스 값을 결정할 때 반영될 수 있도록
        // shift 연산과 XOR 연산을 사용하여, 원래의 해시 값이 a비트 내에서 
        // 최대한 값이 겹치지 않고 구별되게 한다.
        h ^= (h >>> 20) ^ (h >>> 12);
        return h ^ (h >>> 7) ^ (h >>> 4);
   	}


	*Java8에서의 보조 해시 함수
	
	static final int hash(Object key) { int h; return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16); } 


Java8에서는 좀더 간단해진 해시 코드를 사용하는데 그 이유로는 Java8에서는 해시 충돌을 방지하기 위해 링크드리스트 대신 트리를 사용하기 때문이다. 또한 최근 해시 함수는 균등분포가 잘 되게 만들어 지는 경향이 많아 이전에 사용하던 보조 해시 함수의 효과가 크지 않기 때문이다.
 


### String 객체에 대한 해시 함수

String 객체에 대한 해시 함수 수행 시간은 문자열 길이에 비례한다. 따라서 JDK1.1에서는 빠르게 해시 함수를 수행하기 위해, 일정 간격의 문자에 대해 누적한 값을 해시값으로 사용하였다.



	public int hashCode() {  
   		int hash = 0;
    		int skip = Math.max(1, length() / 8);
     		for (int i = 0; i < length(): i+= skip) 
           		hash = s[i] + (37 * hash);
    		return hash;
	}
	
	
모든 문자에 대한 해시 함수를 계산하는 것이 아니라. 문자열의 길이가 16을 넘으면 최소 하나의 문자를 건너가며 계산하고 있다.
이러한 방법의 문제점은 웹의 URL은 수십 글자이면서 앞부분의 문자열은 동일하게 구성되는 경우가 많기 때문이다.
따라서 URL의 해시 값이 같아지는 빈도가 높아질수 있어 이러한 방식은 사용되지 않는다.


현재 사용되는 방식으로는 


	public int hashCode() {  
        int h = hash;
        if (h == 0 && value.length > 0) {
            char val[] = value;

            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
    }


Horner's method를 구현한 것이다. Horner's method는 다항식을 계산하기 쉽도록 단항식으로 이루어진 식으로 표현하는 것이다. 

String 객체 해시 함수에서 31을 사용하는 이유는, 31이 소수이며 또한 어떤 수에 31을 곱하는 것은 빠르게 계산할 수 있기 때문이다. 31N=32N-N인데, 32는 25이니 어떤 수에 대한 32를 곱한 값은 shift 연산으로 쉽게 구현할 수 있다. 따라서 N에 31을 곱한 값은, (N << 5) – N과 같다. 31을 곱하는 연산은 이렇게 최적화된 머신 코드로 생성할 수 있기 때문에, String 클래스에서 해시 값을 계산할 때에는 31을 승수로 사용한다.




### 출처
- [http://d2.naver.com/helloworld/831311](http://d2.naver.com/helloworld/831311)