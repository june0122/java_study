# Chapter 18. 예외처리(Exception Handling)

### *18-1. 예외처리에 대한 이해와 try~catch문의 기본*

***■ if문을 이용한 예외의 처리***

- 에러 → 컴파일 에러
- 예외 → 프로그램의 문법적인 오류가 아닌 논리적인 오류를 뜻한다. 프로그램의 실행 도중에 발생하는 문제의 상황.

> 예외의 예시
- 나이를 입력하라고 했는데, 0보다 작은 값이 입력되었다.
- 나눗셈을 위한 두 개의 정수를 입력 받는데, 나누는 수(제수)로 0이 입력되었다.
- 주민등록번호 13자리만 입력하라고 했는데, 중간에 -를 포함하여 14자리를 입력하였다.

```java
System.out.print("피제수 입력 : ");
int num1 = keyboard.nextInt();

System.out.print("제수 입력 : ");
int num2 = keyboard.nextInt();

/*
아래의 코드가 간단한 코드에서 일반적으로 사용해온 예외처리 방식이다.
이는 'if문이 프로그램의 주흐름인지, 아니면 예외의 처리인지 구분되지 않는다는 단점'이 있다.
주석을 달지 않는 이상 코드레벨에서 예외처리인지 아닌지 판단이 힘들다.
*/

if(num2==0)
{
  System.out.print("제수는 0이 될 수 없습니다.");
  i-=1;
  continue;
}
```
<br><br>
***■ try~catch문***

1. JVM(자바가상머신)이 예외상황의 발생을 감지
2. 표준으로 미리 정의되어 있는 예외 클래스에서 해당 예외를 표현할 수 있는 인스턴스를 생성
3. JVM에 의해 생성된 인스턴스의 참조값을 catch 블럭에 전달

ex) 0으로 나누는 예외상황을 알리기 위해서 정의된 예외 클래스 : ArithmeticException Class


```java
try
{
  // try 영역 : (프로그램의 정상적 흐름 중) 예외가 발생할 수 있는, 발생 가능한 지역
}

catch(AAA e)
{
  // catch 영역 : 예외처리 코드, catch 영역에서 예외상황이 처리된다.
}
```
<br><br>
***■ e.getMessage()***

- ArithmaticException 클래스와 같이 예외상황을 알리기 위해 정의된 클래스를 가리켜 예외 클래스라 한다.

- 모든 예외 클래스는 *Throwable 클래스*를 상속하며, 이 클래스에는 *getMessage 메소드*가 정의되어 있다.

- getMessage 메소드는 예외가 발생한 원인정보를 문자열의 형태로 반환한다.

```java
try
{
  System.out.println("나눗셈 결과의 몫 : "+(num1/num2));
  System.out.println("나눗셈 결과의 나머지 : "+(num1%num2));
}

catch(ArithmeticException e)
{
  System.out.println("나눗셈 불가능");
  System.out.println(e.getMessage());
}
System.out.println("프로그램을 종료합니다.");

/*

실행의 예

두 개의 정수 입력 : 7 0
나눗셈 불가능
/ by zero
프로그램을 종료합니다.

*/
```
<br><br>
***■ 대표적인 예외 클래스들***

- 배열의 접근에 잘못된 인덱스 값을 사용하는 예외상황 → ArrayIndexOutOfBoundsException

- 허용할 수 없는 형변환 연산을 진행하는 예외상황 → ClassCastException

- 배열선언 과정에서 배열의 크기를 음수로 지정하는 예외상황 → NegativeArraySizeException

- 참조변수가 null로 초기화 된 상황에서 메소드를 호출하는 예외상황 → NullPointerException

<br><br>
***■ try~catch의 또다른 장점***

```java
try
{
  System.out.println("피제수 입력 : ");
  int num1=keyboard.nextInt();
  
  System.out.println("제수 입력 : ");
  int num2=keyboard.nextInt();
  
  System.out.println("연산결과를 저장할 배열의 인덱스 입력 : ");
  int idx=keyboard.nextInt();
  
  arr[idx]=num1/num2;
  System.out.println("나눗셈 결과는 "+arr[idx]");
  System.out.println("저장된 위치의 인덱스는 "+idx);
}

catch(ArithmeticException e)
{
  System.out.println("제수는 0이 될 수 없습니다.");
  i-=1;
  continue;
}

catch(ArrayIndexOutOfBoundsException e)
{
  System.out.println("유효하지 않은 인덱스 값입니다.");
  i-=1;
  continue;
}
```

**하나의 try 블록에 둘 이상의 catch 블록을 구성할 수 있기 때문에 예외처리와 관련된 부분을 완전히 별도로 떼어 놓을 수 있다.**

<br><br>
***■ 항상 실행되는 finally***

- 그냥 무조건, 항상 실행되는 것이 아니라, **finally와 연결되어 있는 try 블록으로 일단 진입을 하면**, 무조건 실행되는 영역이 바로 finally 블록이다.

- 중간에 return문을 실행하더라도 finally 블록이 실행된 다음에 메소드를 빠져 나간다.


<br><br>
### *18-2. 프로그래머가 직접 정의하는 예외의 상황*

***■ 예외 클래스의 정의와 throw***

- 나이를 입력하라고 했더니, -20살을 입력했다.
- 이름 정보를 입력하라고 했더니, 나이 정보를 입력했다

이와 같은 상황은 프로그램의 논리적 예외상황이다. 즉, 프로그램의 성격에 따라 결정이 되는 예외상황이다.

따라서 **이러한 경우에는 예외 클래스를 직접 정의**해야 하고, 예외의 발생도 직접 명시해야 한다.

```java
// 예외 클래스는 Throwable의 하위 클래스인 Exception 클래스를 상속해서 정의한다.

class AgeInputException extends Exception
{
    public AgeInputException()
    {
        super("유효하지 않은 나이가 입력되었습니다.");
        
        // Exception 클래스의 생성자로 전달되는 문자열이 getMessage 메소드 호출 시 반환되는 문자열이다.
    }
}
```
<br><br>
***■ 예외를 처리하지 않을 경우***

예외가 발생은 되었는데, 처리하지 않으면 계속해서 반환이 되어 main 메소드를 호출한 가상머신(JVM)에게 전달이 된다.

메소드의 호출관계(예외의 전달 흐름)을 보고 싶을 때는 **printStackTrace** 메소드를 이용한다.
<br><br>
> 가상머신의 예외처리 방식

- 가상머신의 예외처리 1 : getMessage 메소드를 호출한다.

- 가상머신의 예외처리 2 : 예외상황이 발생해서 전달되는 과정을 출력해준다.

- 가상머신의 예외처리 3 : 프로그램을 종료한다.

<br><br>
### *18-3. 예외 클래스의 계층도*
<br><br>
![예외 클래스 간의 상속 관계](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/516/2099.png)

<br><br>
표준 java class 메소드를 호출하기 위해선 API 문서를 참조하여 어떤 예외가 있는 지 확인해야 한다.

<br><br>
***■ 처리하지 않아도 되는 RuntimeException***

- Exception 클래스의 하위 클래스이다.

- 때문에 try~catch문을 통해서 처리하기도 한다.

- 그러나 Error 클래스를 상속하는 예외 클래스와 마찬가지로, try~catch문 또는 Throws절을 명시하지 않아도 된다.

- 이들이 명시하는 예외의 상황은 프로그램의 종료로 이어지는 것이 자연스러운 경우가 대부분이기 때문이다.

<br><br>
> RuntimeException을 상속하는 대표적인 예외 클래스

- ArrayIndexOutOfBoundsException
- ClassCastException
- NegativeArraySizeException
- NullPointerException
