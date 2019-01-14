# 클래스의 상속 1: 상속의 기본 

## 1. 상속의 기본 문법 이해

> 상속은 코드의 `재활용을 위한 문법이 아니다.` 연관된 일련의 클래스들에 대해 `공통적인 규약을 정의`할 수 있는 것이다.

### 상속의 가장 기본적인 특성

```java
class Man {
    String name;
    public void tellYourName() { 
        System.out.println("My name is " + name); 
    }
}

class BusinessMan extends Man {
    String company;
    String position;
    public void tellYourInfo() {
        System.out.println("My company is " + company);
        System.out.println("My position is " + position);
        tellYourName();
    }
}
```

<br>

> BusinessMan man = new BusinessMan();

`man 참조변수 → BusinessMan 인스턴스`

```
// BusinessMan 인스턴스

String name                 // man의 멤버
String company
String position;
void tellYourName() {..}    // man의 멤버
void tellYourInfo() {..}
```

★ private로 선언될 경우, 같은 인스턴스 안에 존재함에도 불구하고 접근이 불가능하다.

<br>

### 상속 관련 용어의 정리와 상속의 UML 표현

> 상속의 대상이 되는 클래스 : 상위 클래스, 기초 클래스, 부모 클래스

> 상속을 하려는 클래스 : 하위 클래스, 유도 클래스, 자식 클래스

<br>

> UML 표현 : 클래스들의 역할과 관계를 표시해놓는 기호

![](https://gmlwjd9405.github.io/images/class-diagram/association.png)

<br>

### 상속과 생성자 1

```java
class Man {
    String name;

    public Man(String name) {
        this.name = name;
    }

    public void tellYourName() { 
        System.out.println("My name is " + name); 
    }
}

class BusinessMan extends Man {
    String company;
    String position;
    
    public BusinessMan(String company, String position) {
        this.company = company;
        this.position = position;
    }

    public void tellYourInfo() {
        System.out.println("My company is " + company);
        System.out.println("My position is " + position);
        tellYourName();
    }
}
```

> BusinessMan 인스턴스 생성시 문제점은?

- BusinessMan이 Man을 상속하므로 BusinessMan의 인스턴스 안에는 `company`와 `position`만 있는 것이 아니라 Man의 인스턴스 변수인 `name`도 존재하게 된다.

- 위 코드에선 BusinessMan의 생성자가 호출이 되면서 `company`와 `position`은 초기화가 되었지만 `name`은 초기화가 되지 않았다.

<br>

### 상속과 생성자 2

```java
class Man {
    String name;

    public Man(String name) {
        this.name = name;
    }
    . . .
}

class BusinessMan extends Man {
    String company;
    String position;
    
    public BusinessMan(String name, String company, String position) {
        // 상위 클래스 Man의 멤버 초기화
        this.name = name;
        
        // 클래스 BusinessMan의 멤버 초기화
        this.company = company;
        this.position = position;
    }

    public void tellYourInfo() { . . . }
}
```

> 모든 멤버의 초기화는 이루어진다. 그러나 `생성자를 통한 초기화 원칙`에는 어긋난다.

- 해당 클래스가 정의될 때, 그 **클래스의 멤버는 해당 클래스의 생성자에서 초기화 시켜주는 것이 이상적**이다.

  - 상속 관계라 할지라도 해당 클래스의 생성자에서 초기화 시켜주는 것이 좋다.

<br>

### 상속과 생성자 3 : 생성자 호출 관계 파악하기

> **하위 클래스의 생성자에서는 반드시 상위 클래스의 생성자를 명시적으로 호출해줘야 한다.**

```java
class SuperCLS {
    public SuperCLS () {
        System.out.println("I'm Super Class");
    }
}

class SubCLS extends SuperCLS {
    public SubCLS () {                          
                                            // 표시되어 있지 않지만 super();가 컴파일러에 의해 들어가 있다.
                                            // 호출할 상위 클래스의 생성자를 명시하지 않으면 void 생성자가 호출 됨.
        System.out.println("I'm Sub Class");
    }
}

class SuperSubCon {
    public static void main(String[] args) {
        new SubCLS ();
    }
}
```

<br>

### 상속과 생성자 4 : 상위 클래스의 생성자 호출 지정

> 키워드 `super`를 통해 상위 클래스의 생성자 호출을 명시할 수 있다.

- `super` 키워드는 생성자 안에서만 사용 가능 

```java
class SuperCLS {
    public SuperCLS() {
        System.out.println("Con: SuperCLS()");
    }

    public SuperCLS(int i) {
        System.out.println("Con: SuperCLS(int i)");
    }

    public SuperCLS(int i, int j) {
        System.out.println("Con: SuperCLS(int i, int j)");
    }
}

class SubCLS extends SuperCLS {
    public SubCLS() {
        super();
        System.out.println("Con: SubCLS()");
    }

    public SubCLS(int i) {
        super(i);
        System.out.println("Con: SubCLS(int i)");
    }

    public SubCLS(int i, int j) {
        super(i, j);
        System.out.println("Con: SubCLS(int i, int j)");
    }
}
```

<br>

### 적절한 생성자 정의의 예

```java
class Man {
    String name;

    public Man(String name) {
        this.name = name;
    }

    public void tellYourName() { 
        System.out.println("My name is " + name); 
    }
}

class BusinessMan extends Man {
    String company;
    String position;
    
    public BusinessMan(String name, String company, String position) {
        super(name);

        this.company = company;
        this.position = position;
    }

    public void tellYourInfo() {
        System.out.println("My company is " + company);
        System.out.println("My position is " + position);
        tellYourName();
    }
}
```

<br>

## 2. 클래스 변수, 클래스 메소드와 상속 (staic변수, static 메소드와 상속)

### 클래스 변수와 메소드는 상속이 되는가? → NO

```java
class SuperCLS {
    static int count = 0;   // 클래스 변수
                            // private가 아닐 때(접근 수준 지시자에서 허용할 때), 하위 클래스에서 이름만으로 접근 가능.
    public SuperCLS() {
        count++;    // 클래스 내에서는 직접 접근이 가능
    }
}

class SubCLS extends SuperCLS {
    public void showCount() {
        System.out.println(count);    // 상위 클래스에 위치하는 클래스 변수에 접근
    }
}
```

<br><br>

# 클래스의 상속 2: 오버라이딩

## 1. 상속을 위한 두 클래스의 관계

### 상속 관계에 놓은 두 대상의 관찰

> 상속의 특성

  - 하위 클래스는 상위 클래스의 모든 특성`(변수와 메소드)`을 지닌다.

  - 거기에 더하여 하위 클래스는 자신만의 추가적인 특성을 더하게 된다.

<br>

```
모바일폰을 스마트폰이 상속한다.

class 스마트폰 extends 모바일폰 {. . .}
```

> 이렇듯, 상속 관계에 있는 두 대상은 `IS-A 관계`를 가져야 한다.

  - `IS-A 관계` : `~은 ~이다` or `일종의 ~` 라는 표현이 성립하는 관계


<br>

## 2. 메소드 오버라이딩

### 상위 클래스의 참조변수가 참조할 수 있는 대상의 범위

```java
class SmartPhone extends MobilePhone {. . . .}

  // 스마트폰은 모바일폰이다.

SmartPhone phone = new SmartPhone("010-555-777", "Nougat");

  // 따라서 스마트폰 참조변수로 스마트폰 참조 가능하고,

MobilePhone phone = new SmartPhone("010-555-777", "Nougat");

  // 모바일폰 참조변수로 스마트폰 참조도 가능하다.
  // 이 역은 성립하지 않으므로 주의한다.
```

<br>

### 참조변수의 참조 가능성 관련 예제

```java
class MobilePhone {
    protected String number;
    
    public MobilePhone(String num) {
        number = num;
    }    
    public void answer() {
        System.out.println("Hi~ from " + number);
    }
}

class SmartPhone extends MobilePhone { 
    private String androidVer;
    
    public SmartPhone(String num, String ver) {
        super(num);
        androidVer = ver;
    }    
    public void playApp() {
        System.out.println("App is running in " + androidVer);
    }
}


class MobileSmartPhoneRef {
    public static void main(String[] args) {
        SmartPhone ph1 = new SmartPhone("010-555-777", "Nougat");
        MobilePhone ph2 = new SmartPhone("010-999-333", "Nougat");
        
        ph1.answer();
        ph1.playApp();
        System.out.println();

        ph2.answer();
        // ph2.playApp(); → 주석을 해제하면 컴파일 오류 발생.
        
        /* MobilePhone형 참조변수는 MobilePhone 클래스에 정의되어있는 메소드는 호출 가능하다.
           MobilePhone형 참조변수가 SmartPhone 인스턴스를 참조하고는 있지만
           SmartPhone 클래스의 메소드는 호출 불가능하다.
           
           부모의 참조변수가 자식 클래스의 메소드는 호출 불가능하다.
        */
    }
}
```

> 부모의 참조변수는 자식의 인스턴스를 참조할 수는 있지만 자식에 대한 접근 권한은 없다.

<br>

### 참조변수 간 대입과 형 변환

```java
class Cake {

    public void sweet() { . . . }
  
}

class CheeseCake extends Cake {

    public void milky() { . . . }

}

CheeseCake ca1 = new CheeseCake();

Cake ca2 = ca1;             // 가능!



Cake ca3 = new CheeseCake();

CheeseCkae ca4 = ca3;        // 불가능!



```

<br>

### 참조변수의 참조 가능성: 배열 기반

```java
class Cake {
  public void sweet() {. . .}
}

class CheeseCake extends Cake {
  public void milky() {. . .}
}
```

> 상속의 관계가 배열 인스턴스의 참조 관계까지 이어진다.

`Cake cake = new CheeseCake();` 가능

`CheeseCake[] cakes = new CheeseCake[10];` 가능

`Cake[] cakes = new CheeseCake[10];` 가능

<br>

### 메소드 오버라이딩 1

```java
class Cake {
  public void yummy() {
    System.out.println("Yummy Cake");
  }
}

// ↑↑ 오버라이딩 관계 (가림)
// CheeseCake의 yummy 메소드가 Cake의 yummy 메소드를 오버라이딩

class CheeseCake extends Cake{
  public void yummy() {
    System.out.println("Yummy Cheese Cake");
  }
}
```

```
public static void main(String[] args) {
  Cake c1 = new CheeseCake();
  CheeseCake c2 = new CheeseCake();
  // Cake c1 = new Cake(); 일 때는 Cake의 yummy가 호출된다.
  
  c1.yummy();
  c2.yummy();
}
```

<br>

### 오버라이 된 메소드 호출하는 방법

> 오버라이딩 된 메소드를 인스턴스 외부에서 호출하는 방법은 없다.<br>그러나 인스턴스 내부에서는 **키워드 super**를 이용해 호출 가능

```java
class Cake {
  public void yummy() {
    System.out.println("Yummy Cake");
  }
}

class CheeseCake extends Cake{
  public void yummy() {
    super.yummy();
    System.out.println("Yummy Cheese Cake");
  }

  public void tasty() {
    super.yummuy();
    System.out,println("Yummy Tasty Cake")
  }
}
```

<br>

### 인스턴스 변수와 클래스 변수도 오버라이딩이 되는가?

> 인스턴스 변수는 오버라이딩 되지 않는다. 따라서 참조변수의 형에 따라 접근하는 멤버가 결정된다.

```java
class Cake {
  public int size;
  . . . 
}

class CheeseCake extends Cake {
  public int size;
  . . . 
}

CheeseCake c1 = new CheeseCake();
c1.size = . . . // CheeseCake의 size에 접근

Cake c2 = new CheeseCake();
C2.size = ... // Cake의 size에 접근

<br>

## 3. instanceof 연산자

### instanceof 연산자의 기본

```java
public static void main(String[] args) {
  Cake cake = new StrawberryCheeseCake();
  
  if (cake instanceof Cake) {. . .}
  
  if (cake instanceof CheeseCake) {. . .}
  
  if (cake instanceof StrawberryCheeseCake) {. . .}
}
```
> if(ref **instanceof** ClassName)
  
  - ref가 ClassName 클래스의 인스턴스를 참조하면 true 반환
  
  - ref가 ClassName를 상속하는 클래스의 인스턴스이면 true 번환
  
<br>

### instanceof 연산자의 활용

```java
//instanceof 연산자의 사용을 보여주기 위해 좋지 않게 만들어진 코드

class Box {
    public void simpleWrap() { 
        System.out.println("Simple Wrapping");
    }
}

class PaperBox extends Box {
    public void paperWrap() {
        System.out.println("Paper Wrapping");
    }
}

class GoldPaperBox extends PaperBox {
    public void goldWrap() {
        System.out.println("Gold Wrapping");
    }
}

class Wrapping {
    public static void main(String[] args) {
        Box box1 = new Box();
        PaperBox box2 = new PaperBox();
        GoldPaperBox box3 = new GoldPaperBox();
        
        wrapBox(box1);
        wrapBox(box2);
        wrapBox(box3);
    }

    public static void wrapBox(Box box) {
        if(box instanceof GoldPaperBox) {
            ((GoldPaperBox)box).goldWrap();
        }
        else if(box instanceof PaperBox) {
            ((PaperBox)box).paperWrap();
        }
        else {
            box.simpleWrap();
        }
    }
}
```

<br>

> ***상속은 연관된 일련의 클래스들에 대해 공통적인 규약을 정의할 수 있다.***

> 위의 코드를 메소드 오버라이딩을 이용하게 간단히 만들어본다.

```java
class Box {
    public void wrap() {
        System.out.println("Simple Wrapping");
    }
}

class PaperBox extends Box {
    public void wrap() {
        System.out.println("Paper Wrapping");
    }
}

class GoldPaperBox extends PaperBox {
    public void wrap() {
        System.out.println("Gold Wrapping");
    }
}

class Wrapping {
    public static void main(String[] args) {
        Box box1 = new Box();
        PaperBox box2 = new PaperBox();
        GoldPaperBox box3 = new GoldPaperBox();

        wrapBox(box1);
        wrapBox(box2);
        wrapBox(box3);
    }

    public static void wrapBox(Box box) {
       box.wrap();
    }
}
```

<br>

> 오버로딩(Overloading)과 오버라이딩(Overriding) 성립조건

|구분|오버로딩|오버라이딩|
|:---:|:---:|:---:|
|메소드 이름|동일|동일|
|매개변수, 타입|다름|동일|
|리턴 타입|상관없음|동일|

<br>

# 클래스의 상속 3: 상속의 목적 

## 1. 상속이 도움이 되는 상황의 소개

### 단순한 인맥 관리 프로그램: 관리 대상이 둘!

```java
 class UnivFriend {       // 대학 동창
    private String name;
    private String major;   // 전공
    private String phone;

    public UnivFriend(String na, String ma, String ph) {
        name = na;
        major = ma;
        phone = ph;
    }

    public void showInfo() {
        System.out.println("이름: " + name);
        System.out.println("전공: " + major);
        System.out.println("전화: " + phone);
    }
}

class CompFriend {        // 직장 동료
    private String name;
    private String department;    // 부서
    private String phone;

    public CompFriend(String na, String de, String ph) {
        name = na;
        department = de;
        phone = ph;
    }
    
    public void showInfo() {
        System.out.println("이름: " + name);
        System.out.println("부서: " + department);
        System.out.println("전화: " + phone);
    }
}
```

<br>

### 두 클래스를 대상으로 하는 코드

```java
// 확장성에 문제를 가지고 있는 코드
// 배열 이름이 다르므로 하나의 메소드로 결합할 수 없다.

class MyFriends {
    public static void main(String[] args) {
        UnivFriend[] ufrns = new UnivFriend[5]; // 정보를 저장할 수 있는 배열을 생성
        int ucnt = 0;   // 배열의 인덱스값, 변수

        CompFriend[] cfrns = new CompFriend[5];
        int ccnt = 0;

        ufrns[ucnt++] = new UnivFriend("LEE", "Computer", "010-333-555");
        ufrns[ucnt++] = new UnivFriend("SEO", "Electronics", "010-222-444");

        cfrns[ccnt++] = new CompFriend("YOON", "R&D 1", "02-123-999");
        cfrns[ccnt++] = new CompFriend("PARK", "R&D 2", "02-321-777");

        for(int i = 0; i < ucnt; i++) {
            ufrns[i].showInfo();
            System.out.println();
        }        
        for(int i = 0; i < ccnt; i++) {
            cfrns[i].showInfo();
            System.out.println();
        }        
    }
}
```

<br>

### 상속 기반의 문제 해결: 두 클래스 상속 관계로 묶기

```java
class Friend {
    protected String name;
    protected String phone;

    public Friend(String na, String ph) {
        name = na;
        phone = ph;
    }

    public void showInfo() {
        System.out.println("이름: " + name);
        System.out.println("전화: " + phone);
    }
    
}

class UnivFriend extends Friend {
    private String major;

    public UnivFriend(String na, String ma, String ph) {
        super(na, ph);
        major = ma;
    }

    public void showInfo() {
        super.showInfo();
        System.out.println("전공: " + major);
    }
}


class CompFriend extends Friend {
    private String department;

    public CompFriend(String na, String de, String ph) {
        super(na, ph);
        department = de;
    }
    
    public void showInfo() {
        super.showInfo();
        System.out.println("부서: " + department);
    }
}
```

<br>

### 상속으로 묶은 결과

```java
class MyFriends2 {
    public static void main(String[] args) {
        Friend[] frns = new Friend[10];
        int cnt = 0;
        
        frns[cnt++] = new UnivFriend("LEE", "Computer", "010-333-555");
        frns[cnt++] = new UnivFriend("SEO", "Electronics", "010-222-444");
        frns[cnt++] = new CompFriend("YOON", "R&D 1", "02-123-999");
        frns[cnt++] = new CompFriend("PARK", "R&D 2", "02-321-777");

        // 모든 동창 및 동료의 정보 전체 출력
        for(int i = 0; i < cnt; i++) {
            frns[i].showInfo();     // 오버라이딩 한 메소드가 호출된다.
            System.out.println();
        }        
    }
}
```

<br>

## 2. Object 클래스와 final 선언 그리고 @Override

### 모든 클래스는 Object 클래스를 상속합니다.

> JVM은 생성되는 모든 인스턴스들을 관리해야 한다. Type이나 이름이 다른 여러 개의 클래스를 관리하기는 힘들 수 있기 때문에 **Object 클래스라는 일련의 규칙**을 적용하여, 어떠한 클래스던지 간에 Object 클래스를 직·간접적으로 상속하도록 디자인을 하였다.

<br>

`class MyClass {. . .}`

  - 상속하는 클래스가 없다면 컴파일러에 의해 다음과 같이 `java.lang.Object` 클래스를 상속하게 코드가 구성된다.

<br>

`class MyClass extends Object {. . .}`

<br>

`class MyClass extends OherClass {. . .}`

  - 이렇듯 다른 클래스를 상속한다면 Object 클래스를 직접 상속하지는 않게 된다.
  
  - 그러나 간접적으로(Object 클래스를 상속하는 클래스를 상속하는 형태로) Object 클래스를 상속하게 된다.

<br>

### 모든 클래스가 Object를 직접 또는 간접 상속하므로

```java
// System.out.println
public void println(Object x) {
  . . .
  String s = x.toString();
  . . .
}
```

<br>

모든 클래스는 Object를 상속하므로 위 메소드의 인자로 전달이 가능하다.

toString 메소드는 Object 클래스의 메소드였음을 알 수 있다. 

<br>

### 프로그래머가 정의하는 toString은 메소드 오버라이딩

```java
class Cake {
  // Object 클래스의 toString 메소드를 오버라이딩
  public String toString() {
    return "My birthday cake";
  }
}

class CheeseCake extends Cake {
  // Cake 클래스의 toString 메소드를 오버라이딩
  public String toString() {
    return "My birthday cheese cake";
  }
}
```

<br>

### 클래스와 메소드의 final 선언

```
public final class MyLastCLS {. . .}
  
  // MyLastCLS 클래스는 다른 클래스가 상속할 수 없음
```

```
class Simple {
  // 아래의 메소드는 다른 클래스에서 오버라이딩 할 수 없음
  public final void func(int n) {. . .}
}
```

<br>

### @Override

- 특별한 기능을 제공하지는 않는다. **안정성을 높이기 위한 문법**.

- 그러나 컴파일러에게 상위 클래스의 메소드를 오버라이딩하는 것이 목적이라는 정보를 전달

- 오버라이딩을 하고자 했는데 실수로 오버로딩이 되는 상황을 막을 수 있다. (컴파일러가 오류 메세지 전달)

```java
// 오버라이딩이 아니라
// 상속으로 두 클래스에 걸쳐서 형성된 메소드 오버로딩이다. (변수형이 다름)

class ParentAdder {
    public int add(int a, int b) {
        return a + b;
    }
}

class ChildAdder extends ParentAdder {
    //@Override
    public double add(double a, double b) {
        System.out.println("덧셈을 진행합니다.");
        return a + b;
    }
}

class OverrideMistake {
    public static void main(String[] args) {
        ParentAdder adder = new ChildAdder();
        System.out.println(adder.add(3, 4));
    }
}
```

