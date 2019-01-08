# Chapter 10. 클래스 변수와 클래스 메소드

## 10-1. static 변수(클래스 변수)

### static 변수

- 인스턴스의 생성과 상관없이 초기화되는 변수

- 하나만 선언되는 변수

- public으로 선언되면 누구나 어디서든 접근 가능!

```java
class InstCnt
{
  static int instNum=0;   // 클래스 변수
  // ★ 'instNum이라는 변수는 InstCnt라는 클래스를 기반으로해서 생성된 모든 인스턴스가 공유하게끔 하겠다'라는 의미를 지님.
  
  /*
  어디서든 접근할 수 있는 변수가 '자리를 빌려서 들어왔다'고 생각하자.
  
  자바는 클래스로 시작해서 클래스로 끝나는 프로그램이므로, 외관상으로 봤을 때 모든 것들이 클래스로 존재해야한다.
  그러므로 static 변수 instNum은 InstCnt라는 집(클래스)에 전세를 내고 들어온 것이라 생각하자.
  static 변수 instNum은 InstCnt라는 집주인과 '계약 관계'에 의해서 들어온 것이지 일부가 될 수는 없다.
  
  우리가 전세로 들어갈 때 대가를 지불하는데, static 변수 instNum 또한 private와 public과 같이 '접근을 제어할 수 있는 권한'을 넘겨준다.
  */
  
  public InstCnt()
  {
      instNum++;
      System.out.println("인스턴스 생성 : "+instNum);
  }
}

class ClassVar
{
  public static void main(String[] args)
  {
      InstCnt cnt1=new InstCnt();
      InstCnt cnt2=new InstCnt();
      InstCnt cnt3=new InstCnt();
  }
}
```

```
// 실행결과

인스턴스 생성 : 1
인스턴스 생성 : 2
인스턴스 생성 : 3

// 실행 결과를 통해서 변수가 공유되고 있음을 확인할 수 있다.
// instNum이 static이 아닌 인스턴스 변수였다면 모두 1 이 출력된다.
```

<br>

### static 변수의 접근방법

- 어떠한 형태로 접근을 하건, 접근의 내용에는 차이가 없다. 다만 접근하는 위치에 따라서 접근의 형태가 달라질 수 있다.

```java
class AccessWay
{
    static int num=0;   //default 선언이므로 동일 패키지 내에서 접근이 가능
    AccessWay()
    {
        incrCnt();
    }
    public void incrCnt() { num++; }    // ① 클래스 내부 접근방법
}

// 패키지 선언이 안된 클래스들끼리도 하나의 패키지를 구성한다 → default 패키지
// AccessWay 클래스가 static 변수를 public으로 선언하면 외부에서도 접근이 가능하지만 private로 선언하면 외부 접근이 불가능하다.

class ClassVarAccess
{
    public static void main(String[] args)
    {
        AccessWay way=new AccessWay();
        way.num++;                                  // ② '인스턴스의 이름'을 이용한 접근방법.
        AccessWay.num++;
        System.out.println("num="+AccessWay.num);   // AccessWay.num → ③ '클래스의 이름'을 이용한 접근방법이 더 좋은 접근 방법이다.
    }
}
```

> `way.num++;` 부가 설명

way라는 참조변수가 가르키는 인스턴스의 클래스 안에 정의되어 있는 num이라는 static 변수에 접근

way라는 참주변수가 가르키는 인스턴스에게 num이라는 변수의 접근을 '요청'하는 것

static 변수의 접근인지 인스턴스 변수의 접근인지 잘 인식이 안되기 때문에 좋은 방법은 아니다.


> `AccessWay.num` 부가 설명

클래스 이름만 가지고 접근이 가능한 것은 분명 멤버 변수는 아니다.

멤버 변수는 인스턴스가 만들어지고 그 인스턴스를 지칭해야하니깐.

클래스 AccessWay안에 선언되어 있는 변수 num에 접근한다 → static 변수라는 의미

★ static 변수는 인스턴스 안에 존재하는 것이 아니다라는 것을 확실히 이해한다!

<br>

### static 변수의 초기화 시점

- JVM은 실행과정에서 필요한 클래스의 정보를 메모리에 로딩한다.

- 바로 이 Loading 시점에서 static 변수가 초기화 된다.

<br>

> 아래 예제에서는 한 번도 인스턴스 생성이 진행되지 않았다.<br>즉, 인스턴스 생성과 static 변수와는 아무런 관련이 없다.

```java
class InstCnt
{
    static int instNum=100;
    public InstCnt()
    {
        instNum++;
        System.out.println("인스턴스 생성 : "+instNum);
    }
}

class StaticValNoInst
{
    public static void main(String[] args)
    {
        InstCnt.instNum-=15;
        System.out.println(InstCnt.instNum);
    }
}
```

<br>

> 왜 `static 변수`를 `클래스 변수`라고 이야기하는가?

- 클래스가 메모리 공간에 로딩될 때 초기화되는 변수이기 때문에.

- 인스턴스와 관계없이 클래스와 관계가 깊은 변수이기 때문에.

<br>

### static 변수의 활용의 예

- 동일한 클래스의 인스턴스 사이에서이 데이터 공유가 필요할 때 static 변수는 유용하게 활용된다.

- 클래스 내부, 또는 외부에서 참조의 목적으로 선언된 변수는 static final로 선언한다.

```java
class Circle
{
    static final double PI=3.1415;   // PI와 같은 상수는 인스턴스 별로 독립적으로 유지할 필요가 없다. 값의 변경도 불필요.
    private double radius;
    
    public Circle(double rad){ radius=rad; }
    public void showPerimeter()   //둘레 출력
    {
        double peri=(radius*2)*PI;
        System.out.println("둘레 : "+peri");
    }
    public void showArea()    // 넓이 출력
    {
        double area=(radius*radius)*PI;
        System.out.println("넓이 : "+area)
    }
}

class ClassVarUse
{
    public static void main(String[] args)
    {
        Circle cl=new Circle(1.2);
        c1.showPerimeter();
        c1.showArea();
    }
}
```

## 10-2. static 메소드(클래스 메소드)

### static 메소드의 정의와 호출

- static 메소드의 기본적인 특성과 접근방법은 static 변수와 동일하다.

```java
class NumberPrinter
{
    public static void showInt(int n){ System.out.println(n); }
    public static void showDouble(double n){ System.out.println(n); }
}

class ClassMethod
{
    public static void main(String[] args)
    {
        NumberPrinter.showInt(20);    // 클래스의 이름을 통한 호출
        NumberPrinter np=new NumberPrinter();
        np.showDouble(3.15);    // 인스턴스의 이름을 통한 호출
    }
}
```

> 위의 예제에서 보이듯이 인스턴스를 생성하지 않아도 static 메소드는 호출 가능하다.

<br>

### static 메소드의 활용의 예

메소드 내에서 인스턴스 변수의 참조를 필요로하지 않는다.

즉, 메소드의 기능을 완성하기 위해서 필요로 한 것이 매개변수로 전달되는 인자가 전부일 때 대부분 static 메소드를 사용한다.

<br>

### static 메소드의 인스턴스 접근은 불가능!

- static 메소드는 인스턴스에 속하지 않기 때문에 인스턴스 멤버에 접근이 불가능하다.

<br>

## 10-3. System.out.println & public static void main

### System과 out?

- System : java.lang 패키지에 묶여있는 클래스의 이름
  
  - import java.lang.*; 자동 삽입되므로 System 이란 이름을 직접 쓸 수 있음
  
- out : static 변수이되 인스턴스를 참조하는 참조변수

  - PrintStream이라는 클래스의 참조변수

```java
public class System
{
    public static final PrintStream out;
    . . . . 
    
     // static final로 선언되었으니, 인스턴스의 생성 없이 system.out이라는 이름으로 접근이 가능하다.
}
```

> Syste.out.println()은 System 클래스의 멤버 out이 참조하는 인스턴스의 println 메소드를 호출하는 문장이다.

<br>

### public static void main

> main 메소드는 static의 형태로 정의하기로 약속했으므로, 어디에 존재하든 상관없다.<br>다만 실행하는 방식에만 차이가 있을 뿐이다.

<br>

### main 메소드의 위치와 인스턴스의 관계

```java
class AAA
{
    public static void makeAAA()
    {
      AAA a1 = new AAA();
      . . . .
    }
    . . . .
}
```

> 이렇듯 모든 메소드는 자신이 속한 클래스의 인스턴스의 생성이 가능하다.<br>이는 main 메소드도 마찬가지이므로 main 메소드는 어디든 존재할 수 있다.
