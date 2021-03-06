---
layout: post
category: Other
title: Request  Response Header
tagline: by TinyBread
tags: [Other]
---


<!--more-->



  
# Request / Response Header  

### Request
 
**If-Modified-Since: Sat, 16 Apr 2016 10:41:14 GMT**   

헤더의 값으로 주어진 날짜 이후 수정이 되었다면 URI데이터를 보낸다는 것을 명시한다. 이것은 클라이언트 측 캐시에 대해 유용하다. 만약 문서가 수정되지 않았다면 서버는 304코드를 반환하여 클라이언트에게 로컬에 있는 사본을 보여준다. 단 지정한 날짜는 Date헤더 아래에 설명된 형식을 따라야 한다.

**If-None-Match: W/"041947fcc97d11:0"**

Contents가 수정된 것이 없다면 304응답 코드를 받고 새로 받아오지 않는다.

**Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8**

클라이언트가 우선적으로 받아들이는 미디어 형을 명시한다. 여러 개의 미디어 형을 쉼표로 구분해서 나열할 수있다. 옵션인 qbalue는 받는 형태의 수준 순서로써 0 에서 1까지 나타낸다.

**Accept-Encoding: gzip, deflate, sdch**

compress 또는 gzip, deflate, sdch 과 같은 클라이언트가 받아들일 수 있는 인코딩 방식을 지정한다. 여러 개의 인코딩 방식을 쉼표로 구분하여 나열한다. 만약 인코딩 형태를 지정하지 않으면 어떤 형태도 클라이언트에게 받아들여지지 않는다.

**Accept-Language: ko-KR,ko;q=0.8,en-US;q=0.6,en;q=0.4**

클라이언트가 우선적으로 지원하는 언어를 지정한다. 쉼표로 구분해서 여러 개의 언어를 지정할 수 있다. 옵션인 qvalue는 우선하지 않는 언어 순서로 0에서 1까지로 나타낸다.  언어는 두문자로 축약해서 쓴다.

**User-Agent: Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/49.0.2623.112 Safari/537.36**

클라이언트 프로그램에 대한 식별 가능한 정보를 준다.

**Cookie: ~**
URL을 위해 저장된 정보의 이름=값을 포함한다. 여러 개의 쿠키는 세미콜론으로 구분하여 나열된다. 

**Upgrade-Insecure-Requests: 1**

Http->https로 변경하여 처리 가능하다는 것을 서버에게 알려준다.

**Connection: keep-alive**

options에서는 연결을 위해 지정하는데 프록시 서버에 의한 연결은 포함하지 않는다. close연결 옵션은 클라이언트나 서버 둘 중 하나가 연결을 해제하기를 원한다는 것을 알린다.

**Host:  HYPERLINK "http://www.auction.co.kr" www.auction.co.kr**

호스트의 이름과 URI의 port번호를 지정한다. 클라이언트는 HTTP1.1에 반드시 이정보를 공급해야 하는데 이것은 여러 개의 호스트명이 갖는 애매한 URL을 쉽게 구별하는 데 도움이 된다.

### Response

**Cache-Control: public**

쉼표로 구분된 목록에 캐싱 지시문을 지정

**캐시 요청 지시문**

	* no-cache : 캐시 하지 않는다.
	* no-store : 신속히 넘긴 후에 정보를 제거한다.
	* max-age = seconds : seconds에 지정한 것보다 오래된 응답은 보내지 않는다.
	* max-stale [=seconds] 만료된 데이터를 보낸다. 만약 seconds가 지정되어 있다면 지정한 숫자보다 적은 만료된 데이터를 보낸다.
	* min-fresh = seconds : 명시된 seconds의 수 이후의 변경된 새 데이터만 보낸다.
	* only-if-cached: 새로운 데이터를 검색하지 않고 캐시에 있는 데이터만 반환한다.

**캐시 응답 지시문**

	* public : 어떠한 캐시라도 캐시할수 있다.
	* private : 공유된 캐시는 캐시하지 않는다.
	* no-cache : 캐시하지 않는다.
	* no-transform : 데이터를 변환하지 않는다.
	* must-revalidate : 클라이언트는 데이터를 재확인 해야 한다.
	* proxy-revalidate : 개인적인 클라이언트 캐시를 제외하고 데이터를 재확인 해야한다.
	* max-age=seconds : 문서는 지정된 seconds만큼만 변화가 없는 상태라고 생각


**Date: Sat, 16 Apr 2016 10:41:49 GMT**

현재의 날짜와 시간을 표시한다.

**Vary: Accept-Encoding**

엔티티가 다중 자원을 가지고 있으므로 요청한 헤더를 지정한 목록이 상황에 따라 변할 수 있다는것을 지정한다. 여러 개의 헤더는 세미콜론으로 구분하여 나열한다.

**Cookie:**

Request와 상동

**Content-Encoding: gzip**

엔티티 몸체를 전송할 때 사용할 인코딩 체계(scheme)를 지정한다. 값으로는gzip(또는 x-gzip)과 compress(또는 x-compress)를 사용할 수 있다. 만약 여러 개의 인코딩 체계(쉼표로 구별한 목록 안에)가 지정되어 있다면 소스 데이터에 적용한 명령을 나열해야 한다.

**Content-Length: 11490**

이 헤더는 전송된 엔티티 몸체가 가진 데이터의 길이(byte 단위로)를 지정한다. 어떤요청은 동적인 성질을 가질 수 있기 때문에 컨텐츠의 길이를 알 수 없을 경우도 있고, 이 경우에는 이 헤더를 제거한다.

**Content-Type: text/html**

엔티티 몸체의 미디어 형과 부미디어 형을 설명한다. 이는 같은 값을 클라이언트의Accept 헤더로 사용하며, 서버는 그 클라이언트가 우선적으로 지원하는 포맷 양식에 따르는 미디어 형을 반환해야 한다.

**ETag: W/"80aafb7ecc97d11:0"**

If-Match와 If-None-Match 요청 헤더를 위한 태그를 정의한다.

**Accept-Ranges: bytes**

URI를 위한 요청 범위의 승인을 나타내며 또는 받아들인 요청의 범위가 없을 경우 none을 지정한다. 범위의 단위는 byte이다.

**Server: Microsoft-IIS/7.5**

서버의 이름과 버전 번호를 포함한다.

**X-Powered-By: ASP.NET**

웹 어플리케이션을 구성하는 기술정보(e.g. ASP.NET, PHP, JBoss)


### 참고
[http://smilekey.blogspot.kr/2009/09/http-header-%EC%A0%95%EB%A6%AC.html](http://smilekey.blogspot.kr/2009/09/http-header-%EC%A0%95%EB%A6%AC.html)
