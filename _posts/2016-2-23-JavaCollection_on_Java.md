---
layout: post
category: Java
title: java Collections
tagline: by TinyBread
tags: [Java]
---


<!--more-->

  
# Java Collections

### 어떤 Collection을 사용해야 하는가?

Collection을 사용할 때는 목적과 용도에 맞게 선택하여 사용해야 한다.(왜 그것을 사용하게 되었는지가 중요하다고 생각한다.)<br>
아래의 이미지는 Collection을 선정할때 도움이 될것같아 참고하게 되었다.

<img src="/assets/themes/Snail/img/Java/JavaCollection/whichDiagrom.png" alt="">
<br> 

### Collection framework       
<img src="/assets/themes/Snail/img/Java/JavaCollection/hierarchy.png" alt="">
<br> 

### Blocking Queue
아래의 참고링크를 보다 Blocking collctions이라는 것이 있어 찾아보게 되었다.

<img src="/assets/themes/Snail/img/Java/JavaCollection/blockingQueueAPI.PNG" alt="">
  
<br>
<br>
**feature**

* 병렬 컬렉션으로서 동기화된 컬랙션의 단점을 보완해주고 있다.(여러스레드 동시에 컬렉션 접근)
* 따라서 멀티스레드 환경의 Producer-Consumer type문제에 적합 (스레드 safe)

	* (Producer-Consumer 작업을 만드는 주체와 처리해주는 주체를 분리 : Producer->queue에 작업 쌓기 / consumer->queue에 쌓은 작업 처리)
	
* 비어있는 queue에 항목을 검색/ 추출시도 queue에 데이터가 준비될 때까지 wait
* 가득찬 queue에 항목을 삽입시도시 queue에 공간이 확보될때까지 wait
<br><br>

<img src="/assets/themes/Snail/img/Java/JavaCollection/blockingQueue.PNG" alt="">
  

### Example BlockingQueue


		
		 class Producer implements Runnable {
		   private final BlockingQueue queue;
		   Producer(BlockingQueue q) { queue = q; }
		   public void run() {
		     try {
		       while (true) { queue.put(produce()); }
		     } catch (InterruptedException ex) { ... handle ...}
		   }
		   Object produce() { ... }
		 }
		
		 class Consumer implements Runnable {
		   private final BlockingQueue queue;
		   Consumer(BlockingQueue q) { queue = q; }
		   public void run() {
		     try {
		       while (true) { consume(queue.take()); }
		     } catch (InterruptedException ex) { ... handle ...}
		   }
		   void consume(Object x) { ... }
		 }
		
		 class Setup {
		   void main() {
		     BlockingQueue q = new SomeQueueImplementation();
		     Producer p = new Producer(q);
		     Consumer c1 = new Consumer(q);
		     Consumer c2 = new Consumer(q);
		     new Thread(p).start();
		     new Thread(c1).start();
		     new Thread(c2).start();
		   }
		 }




### 참고
- [http://java-latte.blogspot.kr/2013/06/dont-know-which-mapcollection-to-use.html](http://java-latte.blogspot.kr/2013/06/dont-know-which-mapcollection-to-use.html)

- [PigBrain](http://pigbrain.github.io)
- [https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/BlockingQueue.html](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/BlockingQueue.html)

- [SynchronizedCollections VS ConcurrentCollections  (http://deepblue28.tistory.com/entry/JavaSynchronizedCollections-vs-ConcurrentCollections)](http://deepblue28.tistory.com/entry/Java-SynchronizedCollections-vs-ConcurrentCollections)

<br>  

