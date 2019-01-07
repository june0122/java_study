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

### 






## *09-5. 캡슐화(Encapsulation)*

### 
