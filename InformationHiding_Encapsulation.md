# Chapter 09. 접근제어 지시자와 정보은닉, 그리고 캡슐화

## 09-1. 정보은닉(Information Hiding)

### 왜 정보은닉이 필요한가

```java
/*
객체지향의 관점에서 매우 좋지 않은 예제
메소드를 통해서 정의한 판매의 규칙이 완전히 무너진 코드
*/

class FruitSalesMain4
{
    public static void main(String[] args)
    {
        FruitSeller seller = new FruitSeller(0, 30, 1500);  // 메소드를 통해 정의한 판매의 규칙
        FruitBuyer buyer = new FruitBuyer(10000); // 메소드를 통해 정의한 판매의 규칙
        
        seller.myMoney += 500;
        buyer.myMoney -= 500;
        seller.numOfApple -= 20;
        buyer.numOfApple += 20;
        // 메소드를 통해 정의한 규칙을 무시하고 멤버 변수에 직접 접근함 → 정보은닉이 되지 않았을 때의 문제점
        // 정보은닉 : 인스턴스 멤버 변수에 직접 접근하는 것을 허용하지 않는 것
        
        System.out.println("과일 판매자의 현재 상황");
        seller.showSaleResult();
        System.out.println("과일 구매자의 현재 상황");
        buyer.showBuyResult();
    }
}
```

<br>

### 정보은닉, 인스턴스 변수의 private

```java
// private 선언으로 외부에서의 접근을 금지

class FruitSeller
{
    private int numOfApple;
    private int myMoney;
    private final int APPLE_PRICE;
    . . . . 
}

class FruitBuyer
{
    private int myMoney;
    private int numOfApple;
    . . . .
}
```

- private와 같은 키워드를 가리켜 접근제어 지시자라고 한다.

- 인스턴스 변수의 private 선언으로 인해서 메소드가 유일한 접근수단

<br>

## *09-2. 접근제어 지시자(Access Control Specifiers)*

### 접근제어 지시자의 관계

> `접근 허용도` : public > protected > default > private

|지시자|클래스 내부|동일 패키지|상속받은 클래스|이외의 영역(남남)|
|------|:------:|:------:|:------:|:------:|
|private|●|×|×|×|
|default|●|●|×|×|
|protected|●|●|●|×|
|public|●|●|●|●|

<br>

### private와 public

- private : 클래스 내부(메소드)에서만 접근 가능

- public : 어디서든 접근 가능(접근을 제한하지 않는다.)

```java
class AAA
{
    private int num;
    public void setNum(int n){ num=n; }
    public int getNum() { return num; }
    . . . .
}
```

```java
class BBB
{
    public accessAAA(AAA inst)
    {
        inst.num=20;  // num은 private 멤버이므로 컴파일 불가, 컴파일 타임에 에러 발생
        inst.setNum(20);
        System.out.println(inst.getNum());  // setNum, getNum은 public이므로 호출 가능
    }
    . . . .
}
```

<br>

### 접근제어 지시자를 선언하지 않는 경우(default)

- 접근제어 지시자 선언하지 않는 경우 : default 선언

- default 선언은 동일 패키지 내에서의 접근 허용

```java
package orange;

class AAA // package orange로 묶인다.
{
    int num;  // default 선언!
    . . . .
}

class BBB // package orange로 묶인다 
{
    public init(AAA a) { a.num=20; }  // BBB는 AAA와 동일 패키지로 선언되었으므로 접근 가능
    . . . .
}
```

<br>

### protected

- protected : 상속 관계에 놓여 있어도 접근을 허용

- default 선언으로 접근 가능한 영역 접근 가능

- 그리고 상속 관계에서도 접근 가능

```java
class AAA
{
    protected int num;
    . . . . 
}

class BBB extends AAA
{
  protected int num;  // 상속된 인스턴스 변수
  
  public init(int n) { num=n; } // 상속된 변수 num의 접근!
  
  /*
  '인스턴스' 개념이 아닌 "클래스" 개념! 선언될 당시 클래스의 어디에 속해 있느냐가 중요.
  
  BBB 클래스 안에 존재하는 init 메소드가 AAA 클래스에 존재하는 num에 접근
  */
  . . . .
}
```

`접근제어 지시자는 인스턴스 관점이 아닌 반드시 클래스의 관점에서 해석해야 한다.`

`클래스에 정의된 형태대로 코드를 분석하고 접근 가능성 여부를 판단하자.`

<br>

## *09-3. public 클래스와 default 클래스*

> ★ public 클래스는 인스턴스의 생성 범위 제한의 목적보다는 핵심이 되는 main 클래스를 표시하려는 marking(마킹)의 역할이 강하다.

> 멤버가 아닌 클래스의 접근 제어를 할 때

※ `라이브러리` : 누군가가 사용할 수 있도록 잘 정의해놓은 클래스들의 모임

  - 라이브러리 내에서 핵심이 되는 클래스 앞에 public 선언을 한다.
  
  - 이외의 클래스는 default 클래스로 설계한다.
  
    - default 클래스들은 외부 영역에서 인스턴스를 생성할 수 없다.
    
### default 클래스

> 동일한 패키지 내에 정의된 클래스에 의해서만 인스턴스 생성이 가능 → 문법적 설명

```java
package apple;
class AAA     // default 클래스 선언
{
    . . . . 
}
```

```java
package pear;
class BBB     // defalut 클래스 선언
{
    public void make()
    {
        apple.AAA inst=new apple.AAA();
        /*
           AAA와 BBB 클래스의 패키지가 다르므로 인스턴스 생성 불가!
           만약 AAA 클래스가 public으로 선언되었다면 인스턴스 생성 가능.
           파일을 대표할 정도로 외부에 의미가 있는 클래스 파일을 public으로 선언한다.
        */
        . . . .
    }
    . . . .
}
```

<br>

### public 클래스

- 어디서든지 인스턴스 생성이 가능하다.
- 하나의 소스파일에 하나의 클래스만 public으로 선언 가능 (★ 하나의 클래스만 public으로 마킹이 가능하다.)
- public 클래스 이름과 소스파일 이름은 일치해야 한다.

```java
package apple;
public class AAA     // default 클래스 선언
{
    . . . . 
}
```

```java
package pear;
public class BBB     // defalut 클래스 선언
{
    public void make()
    {
        apple.AAA inst=new apple.AAA();
        // AAA는 public 클래스이므로 어디서든 생성 가능!
        . . . .
    }
    . . . .
}
```

<br>

### 생성자는 public인데, 클래스는 default인 경우

> 클래스는 public으로 선언되어서 파일을 대표하는 상황.<br>그럼에도 불구하고 생성자가 default로 선언되어서 동일 패키지 내에서만 인스턴스 생성을 허용하는 상황.

> 하나의 패키지 안에 하나의 클래스만 있는 것이 아닌, 여러 개의 클래스가 존재할 때 특정 클래스를 마킹하기 위해 public을 선언하는 것은 나름의 의미를 가진다.

```
public class AAA
{
    AAA(){···}
    . . . .
}
```

> 생성자가 public임에도 클래스가 default로 선언되어서 동일 패키지 내에서만 인스턴스 생성이 허용되는 상황.

```
class BBB
{
    public BBB(){···}
    . . . .
}
```

<br>

### 디폴트 생성자

> 디폴트 생성자의 접근제어 지시자는 클래스의 선언형태에 따라서 결정된다.

```
// public 클래스에 디폴트로 삽입되는 생성자
// 클래스가 public으로 선언이 되면 그 뜻을 따르기 위해선 생성자도 public으로 선언되어야 한다. 

public class AAA
{
    public AAA(){···}
    . . . .
}
```

```
// default 클래스에 디폴트로 삽입되는 생성자
// 클래스가 default로 선언이 되어 있다면 생성자도 그 뜻을 따르기 위해 default로 선언되어야 한다.

class BBB
{
    BBB(){···}
    . . . .
}
```


<br>

## *09-4. 어떤 클래스를 public으로 선언할까?*

### 파일당 하나의 외부 제공 클래스 정의하기

### 클래스를 하나로 묶으면 안되나?

- 변경이 있을 때, 변경되는 클래스의 범위를 줄일 수 있다.

- 작은 크기의 클래스를 다른 클래스의 정의에 활용할 수 있다.

- 객체 지향에서는 아주 큰 하나의 클래스보다, 아주 작은 열 개의 클래스가 더 큰 힘과 위력을 발휘한다.

    - SRP(Single responsibility principle, 단일 책임 원칙) → 캡슐화

### ★ SOLID

```
컴퓨터 프로그래밍에서 SOLID란 로버트 마틴이 2000년대 초반에 명명한 객체 지향 프로그래밍 및 설계의

다섯 가지 기본 원칙을 마이클 페더스가 두문자어 기억술로 소개한 것이다. 

프로그래머가 시간이 지나도 유지 보수와 확장이 쉬운 시스템을 만들고자 할 때 이 원칙들을 함께 적용할 수 있다.

SOLID 원칙들은 소프트웨어 작업에서 프로그래머가 소스 코드가 읽기 쉽고 확장하기 쉽게 될 때까지

소프트웨어 소스 코드를 리팩터링하여 코드 냄새(code smell)를 제거하기 위해 적용할 수 있는 지침이다. 
```

|두문자|약어|개념|
|:---:|:---:|---|
|S|SRP|Single responsibility principle(단일 책임 원칙)<br>한 클래스는 하나의 책임만 가져야 한다.|
|O|OCP|Open/closed principle(개방-폐쇄 원칙)<br>소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.|
|L|LSP|Liskov substitution principle(리스코브 치환 원칙)<br>프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서<br>하위타입의 인스턴스를 바꿀 수 있어야 한다.|
|I|ISP|Interface segregation principle(인터페이스 분리 원칙)<br>특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다|
|D|DIP|Dependency inversion principle(의존관계 역전 원칙)<br>프로그래머는 추상화에 의존해야지, 구체화에 의존하면 안된다.|

<br>

## *09-5. 캡슐화(Encapsulation)*

### 감기약 비유를 통한 캡슐화의 설명

> 캡슐화가 이뤄지지 않았을 때

```java
class SinivelCap    // 콧물 처치용 캡슐
{
    public void take(){ · · · · }
}
class SneezeCap    // 재채기 처치용 캡슐
{
    public void take(){ · · · · }
}
class SnuffleCap    // 코막힘 처치용 캡슐
{
    public void take(){ · · · · }
}

/*
세개의 클래스가 하나의 목적인 '콧물 감기의 치료'라는 일치된 목적을 갖고 있다.
그럼에도 불구하고 클래스가 나뉘어 있다.

SinivelCap, SneezeCap, SnuffleCap의 연관관계가 깊다면 캡슐화가 이뤄지지 않은 상태
*/
```

```java
public static void main(String[] args)
{
    ColdPatient sufferer = new ColdPatient();
    sufferer.takeSinivelCap(new SinivelCap());
    sufferer.takeSneezeCap(new SneezeCap());
    sufferer.takeSnuffleCap(new SnuffleCap());
}

/*
약의 복용순서가 정해져 있다면 약을 복용하는 사람은 복용과 관련해서 추가적인 지식이 필요하다.
캡슐화가 이뤄지지 않으면, 클래스의 사용을 위해서 알아야 할 것들이 많아진다.
*/
```

<br>

> 캡슐화가 이뤄졌을 때

```java
class CONTAC600
{
    SinivelCap sin;
    SneezeCap sne;
    SnuffleCap snu;
    
    public CONTAC600()
    {
        sin=new SinivelCap();
        sne=new SneezeCap();
        sne=new SnuffleCap();
    }
    
    public void take()
    {
        sin.take();
        sne.take();
        snu.take();
    }
}

/*
"캡슐화"는 관련 있는 모든 메소드아 변수를 하나의 클래스로 묶는 것이다.

둘 이상의 클래스를 묶어서 캡슐화를 완성할 수도 있다. 캡슐화는 메소드와 변수가
코드레벨에서 묶이는 것을 의미하지 않는다.(예시는 인스턴스 관점에서 보자) 캡슐화는 개념적인 의미의 묶음을 의미한다.

take 메소드 내에 약의 복용순서가 그래도 기록되어 있다. 따라서 약을 복용을 위해 알아야 할 것이
take 메소드 하나이다. 이것이 캡슐화의 목적 및 장점이다.
*/
```
