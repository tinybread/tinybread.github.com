---
layout: post
category: Java
title: Overview of java.io's Input and Output Streams
tagline: by TinyBread
tags: [Java]
---

<!--more-->


# Input/Output
  
## Representing File Paths  
* `java.io.File`는 파일의 paths를 관리하기 위한 클래스이다  
	* path는 실제 디스크의 파일 혹은 디렉토리를 가르킬 수도 있고 가르키지 않을 수도 있다  
	* File 클래스의 메서드들을 통하여 path를 다루고 파일 시스템의 작업을 수행할 수 있다  
	* File 클래스는 실제 파일의 데이터를 읽거나 쓰는데 사용되진 않는다  
  
* File 클래스의 생성자는 오버로드 되어 있기 때문에 다음 형태로 객체를 생성할 수 있다  
	* path를 나타내는 문자열 
	* 상위 디렉토리를 나타내는 문자열 혹은 File 객체 그리고 하위 디렉토리를 나타내는 문자열 혹은 File 객체  
  
* File 객체를 생성하기 위하여 사용되는 path는 절대 경로와 상대 경로 모든 형태로 나타낼 수 있다  
  
* String 객체처럼 File 객체 또한 변경할 수 없다. (불변)
	* 한번 객체를 생성하면 path를 변경 할 수 없다  
  
* 자바는 어떻게 플랫폼마다 다른 디렉토리 구분자를 다룰까 ? 
	* 기본 구분자는 `file.separator`라는 시스템 속성에 의하여 정의된다  
	* 위 속성에 의하여 정의된 구분자는 File 클래스의 public static 타입인 `separator`와 `separatorChar`를 통하여 사용 가능하다  
  
{% highlight java %}  
// WinNTFileSystem.class
class WinNTFileSystem extends FileSystem {
	
	private final char slash;
	private final char altSlash;
	private final char semicolon;

	public WinNTFileSystem() {
		slash = AccessController.doPrivileged(new GetPropertyAction("file.separator")).charAt(0);
		
		semicolon = AccessController.doPrivileged(new GetPropertyAction("path.separator")).charAt(0);

		altSlash = (this.slash == '\\') ? '/' : '\\';
	}
	// 생략
}
{% endhighlight %}  
  
  
## Managing File Paths  
* File객체는 파일 시스템 작업을 수행하고 조회하기 위한 메서드들을 제공한다  
	* 파일 시스템을 조회하기 위한 메서드들..
		* canRead(),canWrite(), exists(), isDirectory(), isFile(), isHidden(), getAbsolutePath(), lastModified(), length(), listFiles(), listRoots()  
	* 파일 시스템을 변경하기 위한 메서드들..  
		* createNewFile(), mkdir(), renameTo(), delete(), deleteOnExit(), setReadOnly(), setLastModified()  
  
  
{% highlight java %}  
package io;

import java.io.File;

public class Remove {
	public static void main(String[] args) {
		if (args.length == 0) {
			System.err.println("Usage: Remove <file|directory>...");
		}
		for (int i = 0; i < args.length; i++) {
			remove(new File(args[i]));
		}
	}

	private static void remove(File file) {  
		if (!file.exists()) {  
			System.err.println("No such file or directory: " +  file.getAbsolutePath());
		} else if (file.isDirectory() && file.list().length > 0) {
			System.err.println("Directory contains files: " + file.getAbsolutePath());
		} else if (!file.delete()) {
			System.err.println("Could not remove: " + file.getAbsolutePath());
		}
	}
}
{% endhighlight %}  
  
  
## Input/Output Class Hierarchy  
* `java.io`패키지는 입력(input)과 출력(output)을 위한 많은 클래스들을 포함하고 있다  
	* 4개의 추상클래스(InputStream, OutputStream, Reader, Writer)가 대부분의 주요한 기능을 제공하고 있다  
		* **Byte**(8bit) 형태의 데이터 처리를 위한 `InputStream` 그리고 `OutputStream`  
		* **Character**(16bit) 형태의 데이터 처리를 위한 `Reader` 그리고 `Writer`  
	* 데이터를 읽기위한 InputStream과 Reader의 메서드들은 매우 유사하고 데이터를 쓰기 위한 OutputStream 그리고 Writer의 메서드들 또한 매우 유사하다  
	* 많은 I/O 클래스들은 클래스에 동적으로 추가적인 역할을 부여하기 위하여 **Decorator Pattern** 형태로 디자인 되었다  
		* 대부분의 I/O 클래스들은 Reader, Writer 혹은 InputStream, OutputStream을 파라미터로 받는 생성자를 가지고 있다  
		* 각각의 클래스들은 buffering, compression.. 등과 같은 추가적인 기능들을 제공한다  
		* I/O 클래스 계층에서 Decorator Pattern의 중요한 점은 다형성(polymorphism)이다  
			* FileReader **is-a** Reader  
			* BufferedReader **is-a** Reader  
			* LineNumberReader **is-a** Reader  
  
  
## Byte Streams  
* InputStream - byte를 읽기 위한 추상 클래스  
	* read()  
		* 1byte를 읽거나 바이트 배열로 복사한다  
	* skip()  
	* mark(), reset()  
	* close() 
		* finally 블럭 내에서 처리 해야 한다  
* OutputStream - byte를 쓰기 위한 추상 클래스  
	* write()  
		* 1byte를 출력하거나 바이트 배열을 출력한다  
	* flush()  
	* close()  
* 아무 파라미터가 없는 `InputStream.read()` 메서드는 int 값(읽은 byte의 수)을 리턴한다. 만약 스트림의 끝에 다다르면 -1을 리턴한다  
* 한번에 한 byte씩 읽고 쓰려면 다음과 형태로 작성해야 한다 
  
{% highlight java %}  
int b = 0;
while ((b == fis.read()) != -1) {
	System.out.write(b);
}
{% endhighlight %}  
  
* 위와는 다르게 byte 배열을 이용하여 버퍼 형태로 사용하는 것이 훨씬 효율적이다  
  
{% highlight java %}  
private static void read(File file) {
	try {
		FileInputStream fis = new FileInputStream(file);
		try {
			byte[] buffer = new byte[1024];
			int len = 0;
			while ((len = fis.read(buffer)) > 0) {
				System.out.write(buffer, 0, len);
			}
		} finally {
			fis.close();
		}
	} catch (FileNotFoundException e) {
		System.err.println("No such file exists: " + file.getAbsolutePath());
	} catch (IOException e) {
		System.err.println("Cannot read file: " + file.getAbsolutePath() + ": " + e.getMessage());
	}
}
{% endhighlight %}  
  
## Character Streams  
* Reader - character를 읽기 위한 추상 클래스  
	* read()  
		* 1 char(2 bytes)를 읽거나 char 배열로 복사한다  
	* skip()  
	* mark(), reset()  
	* close() 
		* finally 블럭 내에서 처리 해야 한다  
* Writer - character를 쓰기 위한 추상 클래스  
	* write()  
		* 1 char(2 bytes)를 출력하거나 char 배열을 출력한다  
	* flush()  
	* close()  
* 아무 파라미터가 없는 `ReaderReader.read()` 메서드는 int 값(읽은 char의 수)을 리턴한다. 만약 스트림의 끝에 다다르면 -1을 리턴한다  
* 한번에 한 byte씩 읽고 쓰려면 다음과 형태로 작성해야 한다 
  
  
{% highlight java %}  
private static void readDefault(File file) {
	try {
		FileReader reader = new FileReader(file);
		try {
			int ch;
			while ((ch = reader.read()) != -1) {
				System.out.print((char) ch);
			}
			System.out.flush();
		} finally {
			reader.close();
		}
	} catch (FileNotFoundException e) {
		System.err.println("No such file exists: " + file.getAbsolutePath());
	} catch (IOException e) {
		System.err.println("Cannot read file: " + file.getAbsolutePath() + ": " + e.getMessage());
	}
}
{% endhighlight %}  
  
  
## Exception Handling in Java I/O  
* 대부분의 I/O 클래스 생성자와 메서드들은 checked exception을 던질 수 있다  
* I/O exception들의 기본이 되는 클래스는 `java.io.IOException`이다  
	* 몇몇 메소드들은 IOException의 자식 클래스를 던진다  
	* 예를들어 FileInputStream, FileOutputStream, FileReader의 생성자들은 FileNotFoundException을 던질 수 있다  
* close() 메서드가 Exception이 발생 하였을때도 호출되는 것을 보장하기 위해서는 반드시 finally 블럭내에서 호출해야 한다 
  
  
## Java File I/O Classes  
* `java.io` 패키지에는 파일 I/O 처리를 위해 구현된 클래스들이 포함되어 있다  
	* FileInputStream, FileOutputStream, FileReader, FileWriter  
* 오버로드된 다양한 생성자들은 파일 path, File 객체와 같은 것들을 이용하여 객체를 생성 할 수 있도록 해준다   
	* 만약 생성자에서 file을 열 수 없을 경우 FileWriter를 제외한 다른 클래스들은  FileNotFoundException을 던진다  
	* FileWriter는 IOException을 던진다  
* FileOutputStream과 FileWriter의 생성자는 만약 파일이 존재하지 않을 경우 OS에서 파일 생성을 금지하지 않는다면 파일을 생성한다  
* 기본적으로 파일을 쓸때는 파일의 첫 부분부터 쓰게 된다. 즉 파일에 데이터가 있을 경우 덮어쓰게 된다  
	* append 속성이 있는 생성자를 이용하여 `true`로 지정하면 파일 데이터의 끝에 이어서 쓰게 된다  
* FileWriter와 FileReader는 character변환을 위하여 기본으로 시스템의 인코딩 방식을 사용한다. 만약 이 인코딩을 바꾸고 싶다면 파일을 FileInputStream이나 FileOutputStream을 이용하여 열어야 한다. 그리고 InputStreamReader이나 OutputStreamWriter를 adapter로 이용하면서 인코딩 타입을 지정할 수 있다  
  
{% highlight java %}  
private static void readUTF8(File file) {
	try {
		FileInputStream fis = new FileInputStream(file);
		InputStreamReader reader = new InputStreamReader(fis, "UTF-8");

		try {
			OutputStreamWriter writer = new OutputStreamWriter(System.out, "UTF-8");
			
			int ch;
			while ((ch = reader.read()) != -1) {
				writer.write(ch);
			}
			writer.flush();
		} finally {
			reader.close();
		}
	} catch (FileNotFoundException e) {
		System.err.println("No such file exists: " +  file.getAbsolutePath());
	} catch (IOException e) {
		System.err.println("Cannot read file: " + file.getAbsolutePath() + ": " + e.getMessage());
	}
}
{% endhighlight %}  
  
## Easy Text Output: PrintWriter/PrintStream  
* PrintWriter와 PrintStream은 일반적인 텍스트 출력 작업을 단순화하도록 설계되었다  
	* print() 메서드는 모든 자바의 primitive 타입들에 대해 String 형태로 출력가능 하도록 오버로드되어 있고 모든 오브젝트들에 대해서는 toString() 메서드를 자동적으로 호출하여 출력한다  
	* println() 메서는 print()와 동일하지만 플랫폼에 따른 줄바꿈 문자가 추가된다  
	* format() 메서드와 printf() 메서드는 다양한 형태로 출력을 하기위한 메서드이다  
		* printf()메서드는 C언어의 근간하여 디자인 되었다  
	* 이 클래스의 메서드들은 IOException을 던지지 않는다. 대신 checkError()메서드를 통하여 예외 상황에서 내부적으로 셋팅된 플래그의 값을 검사할 수 있다  
* System.out과 System.err는 PrintStream의 인스턴스이다  
  
## Reading from the Terminal
* 어떤 역사적인(?) 이유로 System.in 오브젝트는 Writer가 아닌 InputStream의 인스턴스이다  
	* read() 메서드는 character가 아닌 1byte를 읽는다  
* java.io.InputStreamReader를 adapter로 사용할 수 있다  
	* 기본적으로 byte들을 읽고 시스템 인코딩 타입에 맞는 character 형태로 변환해준다  
	* InputStreamReader에는 다양한 생성자가 오버로드 되어 있어서 시스템 인코딩 타입 외에 다른 타입을 지정할 수 있다  
	* read()메서드는 한번에 1 character를 읽기 위해 사용 할 수 있다  
* java.io.BufferedReader 클래스는 character 스트림에서 조금 더 효율적인 처리를 위하여 character들을 버퍼로 관리한다  
	* BufferedReader 클래스가 가지고 잇는 readLine()메서드는 줄바꿈 문자를 제거하고 줄 단위로 입력된 텍스트를 String 객체 형태로 리턴해준다  
* InputStreamReader와 BufferedReader를 혼합하여 터미널에서 입력한 데이터를 편리하게 처리할 수 있다  
  
{% highlight java %}  
BufferedReader input = new BufferedReader(new InputStreamReader(System.in) );
String line = input.readLine();
{% endhighlight %}  
  
* 터미널에서 줄 단위로 입력을 받기위해서는 아래와 같이 할 수 있다  
{% highlight java %}  
package com.marakana.demo;

import java.io.*;

public class HelloYou {
	public static void main(String[] args) {
		System.out.print("What's your name? ");
		
		BufferedReader input = new BufferedReader(new InputStreamReader(System.in) );
		
		try {
			String name = input.readLine();
			System.out.println("Hello, " + name + "!");
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}
{% endhighlight %}  
  
  
# 원문   
* https://newcircle.com/bookshelf/java_fundamentals_tutorial/input_output  