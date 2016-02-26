---
layout: post
category: CleanCode
title: CleanCode 오류처리
tagline: by TinyBread
tags: [CleanCode]
---


<!--more-->

  
# 오류처리

코드를 짜다보면 여기저기 오류처리 하는 부분이 발생하게 된다. 때문에 항상 고민해 왔던부분은 어떻게 예외를 처리해야 할까라는 고민이었다. 어디서 try-catch-finally를 해야하며, Exception 처리는 어디서 해야하는지, 커스텀 익샙션은 Checked로 만들어야 하는지 Unchecked로 만들어야 하는지...


일관성 없는 오류 처리는 코드의 가독성을 떨어뜨려 주고 프로그램의 논리를 이해하기에 어렵다. 즉, 깨끗한 코드를 만들려면 예외처리 하는부분을 반드시 신경써야 한다.

 따라서 몇가지 고려사항을 통해 가독성 높은 코드를 작성해야한다.

### 고려사항  

* **오류코드보다 예외를 사용한다.**
		
	* 오류 코드를 반환하는 함수가 있다면 그 함수를 호출한 즉시 오류를 확인해야 한다는 문제가 발생한다. 그러나 예외를 던진다면 그 예외를 즉시 처리 하지 않고 한꺼번에 처리 할수 있다.(즉, 알고리즘과 오류 처리하는 부분을 분리해 낸다, 오류 부분을 모아서 처리할수 있게 된다.)<br>

			
* Try-Catch-Finally문 부터 작성한다.

	* try 블록에서 무슨일이 생기든지 catch 블록은 프로그램 상태를 일관성 있게 유지해야 한다. 그러므로 예외가 발생할 코드를 짤때는 try-catch-finally 문부터 시작하는 편이 낫다. 또한 Test case를 작성하여 테스트를 통과하게 하는 코드를 작서한다. 그렇게 되면 try블럭안의 부분부터 구현하게 되므로 범위내에서 트랜잭션을 만족하기 때문이다.<br>


* unchecked 예외를 사용한다.(RuntimeException)
	
	* checked 예외는 OCP(Opend Closed Principle :  확장에 열려있고 변경에 닫혀있다.)를 위반한다. 만약 메서드에 checked예외를 던졌는데 catch블럭이 세단계 위에 있다면 그 사이 메서드 모든 메서드 선언부에 해당예외를 정의해야 한다. 즉, 하위 단계코드에 예외를 변경해야 한다면 관련메소드의 해당부분을 모드 변경해야 한다는 것이다.  결과적으로 연쇄적인 수정이 발생하기 때문이다.<br>

* 예외에 의미를 제공한다.

	* 예외를 던질때 오류메시지를 통해 정보를 전달한다면, 오류 발생원인과 위치 찾는것에 도움이 된다.(예외에 메시지를 전달하자! 만약 커스텀 예외를 생성해 던진다면 그 이름을 명확하게 하자.)<br>

* 호출자를 고려해 예외 클래스를 정의한다.
	
	* 외부 라이브러리를 호출하고 그 외부 라이브러리가 예외를 던진다면, 호출하는 라이브러리 API를 감싸서 예외 유형을 반환한다.(예외에 대응하는 방식이 거의 동일할때, 아래의 코드로 설명)   <br>   
	
	
			  ACMEPort port = new ACMEPort(12);
			  
			  try {
			    port.open();
			  } catch (DeviceResponseException e) {
			    reportPortError(e);
			    logger.log("Device response exception", e);
			  } catch (ATM1212UnlockedException e) {
			    reportPortError(e);
			    logger.log("Unlock exception", e);
			  } catch (GMXError e) {
			    reportPortError(e);
			    logger.log("Device response exception");
			  } finally {
			    ...
			  }
	
		위의 방법은 예외 코드를 외부 라이브러리를 호출하는 try-catch문에서 전부 하나씩 처리함으로써 중복코드 발생
			
			  LocalPort port = new LocalPort(12);
			  
			  try {
			    port.open();
			  } catch (PortDeviceFailure e) {
			    reportError(e);
			    logger.log(e.getMessage(), e);
			  } finally {
			    ...
			  }
			
			  public class LocalPort {
			    private ACMEPort innerPort;
			    public LocalPort(int portNumber) {
			      innerPort = new ACMEPort(portNumber);
			    }
			
			    public void open() {
			      try {
			        innerPort.open();
			      } catch (DeviceResponseException e) {
			        throw new PortDeviceFailure(e);
			      } catch (ATM1212UnlockedException e) {
			        throw new PortDeviceFailure(e);
			      } catch (GMXError e) {
			        throw new PortDeviceFailure(e);
			      }
			    }
			    ...
			  }
	
		아래의 코드는 예외를 처리하는 wrapper클래스를 통해 예외를 던짐으로써 외부 라이브러리 사이의 의존성 또한 줄일수 있게 된다. 또한 테스트를 함에 있어서도 쉬워진다.<br>


* 정상 흐름을 정의한다.

	* 예외를 통해 중단된 일을 처리하는건 대게는 좋은 방식이나, 때로는 이러한 방법이 적절하지 못할때가 존재한다. 아래의 예제를 통하여 알아보도록 하자.<br>

		
			  try {
			    MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
			    m_total += expenses.getTotal();
			  } catch(MealExpensesNotFound e) {
			    m_total += getMealPerDiem();
			  }	
			
		위의 코드는 예외가 논리를 따라가기에 어렵게 만든다. 따라서 아래에서 객체를 반환함으로써 예외가 발생하지 않게 코드를 변경할 수 있다.
			
			  MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
			  m_total += expenses.getTotal();
			
			  public class PerDiemMealExpenses implements MealExpenses {
			    public int getTotal() {
			      // return the per diem default
			    }
			  }

	*  SPECIAL CASE PATTERN : 클래스를 만들거나 객체를 조작해 특수 사례를 처리하는 방식. 클래스나 객체가 예외적인 상황을 캡슐화해서 처리하기 때문에 클라이언트 코드가 예외적인 상황을 처리할 필요가 없다.


* null을 반환하지 마라
	
	* 어떤 메소드에서 null을 반환하는 것보다 예외를 발생 시키자. null을 반환한다면 반드시 확인을 해주어야 하고 만일 null을 체크하는 부분을 누락한다면 NullPointerException이 발생할 수 있기 때문이다. <br>
* null을 전달하지 마라

	* null을 전달하게 되면 NullPointerException이 발생하거나 이 예외를 잡기위해 새로운 커스텀 예외를 발생해야 한다. null을 정상적인 인수로 기대하는 것이 아니라면 null을 전달하는 코드를 피하도록한다.<br>


# 참고  
* CleanCode (인사이트/로버트 C. 마틴(박재호 이해영 옮김)) 