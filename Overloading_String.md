# Chapter 11. 메소드 오버로딩과 String 클래스

## ※ 생성자

생성자는 말그대로 객체를 생성하고 초기화를 하는 역할을 담당하고 있으며, 생성자를 실행할 수 없다면 클래스는 객체를 생성할 수가 없다.
그렇기 때문에 모든 클래스에는 생성자가 한 개 이상 반드시 존재하며 따로 선언을 하지 않더라도 보이지 않는 디폴트 생성자가 자동으로 추가된다.

### 디폴트 생성자(Default Constructor)

- 기본 생성자라고도 부른다.

- 클래스에 매개변수나 아무 생성자가 없을 때 따로 정의하지 않 자바 컴파일러에 의해 기본적으로 만들어지는 생성자 

- 하지만 클래스 내부에 선언한 생성자가 하나라도 있으면 디폴트 생성자는 생성되지 않는다.

<br>

### 생성자의 조건

- 클래스명과 생성자의 이름이 동일해야 한다.

- 리턴타입이 없다.

- 매개변수나 내용이 없으면 디폴트 생성자가 자동으로 생성된다.

- 매개변수가 다른 생성자를 여러 개 만드는 오버로딩이 가능하다.

- new 연산자를 이용하여 선언한다.

- 주된 목적은 필드(멤버변수)의 초기화를 하기 위해 사용된다.

- this와 super로 생성자를 호출할 수 있다.

<br>

### 필드 초기화

> 클래스 안의 동일한 이름을 가지는 객체들은 처음에 모두 같은 데이터를 가지고 있는데,<br>그 객체마다 값을 변경해주기 위해 초기화를 한다.

```java
public class School{
  
    String name = "지훈";
    int age = 27;
    
    public static void main(String[] args){
    
        School sc1 = new School();
        System.out.println("sc1의 이름 :" + sc1.name);
        System.out.println("sc1의 나이 :" + sc1.age);
        
        School sc2 = new School();
        System.out.println("sc2의 이름 :" + sc1.name);
        System.out.println("sc2의 이름 :" + sc1.age);
    }
}
```

```
//출력결과 

sc1의 이름 :지훈
sc1의 나이 :27
sc2의 이름 :지훈
sc2의 나이 :27
```

필드를 초기화 해주지 않아 데이터가 동일하게 출력된다.

<br>

```java
public class School{
  
    String name = "지훈";
    int age = 27;             // 필드
    
    public School(String na, int ag){
        name = na;
        age = ag;             // 필드 초기화
    }
    
    public static void main(String[] args){
    
        School sc1 = new School("후니", 28);
        System.out.println("sc1의 이름 :" + sc1.name);
        System.out.println("sc1의 나이 :" + sc1.age);
        
        School sc2 = new School("심지", 29);
        System.out.println("sc2의 이름 :" + sc1.name);
        System.out.println("sc2의 이름 :" + sc1.age);
    }
}
```

```
//출력결과 

sc1의 이름 :후니
sc1의 나이 :28
sc2의 이름 :심지
sc2의 나이 :29
```


매개변수의 값을 다르게 하고 싶을 때 필드의 초기화를 하는 것이고, 이것이 생성자의 역할이다.

<br>

- - -

## 11-1. 메소드 오버로딩(Overloading)

- 메소드 오버로딩이란 동일한 이름의 메소드를 둘 이상 동시에 정의하는 것을 뜻한다.

- 메소드의 매개변수 선언(개수 또는 자료형)이 다르면 메소드 오버로딩이 성립한다.

- 오버로딩 된 메소드는 호출 시 전달하는 인자를 통해서 구분된다.

```java
class AAA
{
    // 오버로딩 된 메소드
    void isYourFunc(int n) { · · · · }
    void isYourFunc(int n1, int n2) { · · · · }
    void isYourFunc(int n1, double n2) { · · · · }
    · · · ·
}
```

```
//  전달되는 인자의 유형을 통해서 호출되는 함수가 결정

AAA inst = new AAA();
inst.isYourFunc(10);
inst.isYourFunc(10, 20);
inst.isYourFunc(12, 3.15);
```

<br>

### 생성자도 오버로딩의 대상이 된다

> 생성자의 오버로딩은 하나의 클래스를 기반으로 다양한 형태의 인스턴스 생성을 가능하게 한다.

```java
class Person
{
    private int perID;
    privae int milID;
    
    public Person(int PID, int mID)
    {
        perID=pID;
        MilID=0;
    }
    public void showInfo()
    {
        System.out.println("민번 : "+perID)
        if(milID!=0)
            System.out.println("군번 : "+milID+'\n');
        else
            System.out.println("군과 관계 없음 \n");
    }
}
```

```java
public static void main(String[] args)
{
    Person man=new Person(950123, 880102);
    Persn woman=new Person(941125);
    man.showInfo();
    woman.showInfo();
}
```

→ 생성자의 오버로딩이 가능하다는 것은 클래스가 하나로 정의되어 있을지라도 인스턴스의 생성 방법을 세분화 할 수 있다는 것이다.

<br>

### 키워드 this를 이용한 다른 생성자의 호출

- 키워드 this를 이용하면 생성자 내에서 다른 생성자를 호출할 수 있다.

- 이는 생성자의 추가 정의에 대한 편의를 제공한다.

- 생성자마다 중복되는 초기화 과정의 중복을 피할 수 있다.

```java
class Person
{
    private int perID;
    private int milID;
    private int birthYear;
    private int birthMonth;
    private int birthDay;
    
    public Person(int perID, int milID, int bYear, int bMonth, int bDay)
    {
        this.perID=perID;
        this.milID=milID;
        birthYear=bYear;
        birthMonth=bMonth;
        birthDay=bDay;
    }
    public Person(int perID, int bYear, int bMonth, int bDay)
    {
        this(pID, 0, bYear, bMonth,bDay;)
        
        /* 생성자 재호출을 위한 키워드 this가 존재하지 않았다면
        
        this.perID=perID;
        this.milID=0;
        birthYear=bYear;
        birthMonth=bMonth;
        birthDay=bDay;
        
        으로 초기화 과정의 중복이 있었을 것이다.
        */
    }
}
```

<br>

## 11-2. String 클래스

### String 클래스의 인스턴스 생성

- JAVA는 큰 따옴표로 묶여서 표현되는 문자열을 모두 **인스턴스화** 한다.

- 문자열은 String 이라는 이름의 클래스로 표현된다.

```
// 두 개의 String 인스턴스 생성, 그리고 참조변수 str1과 str2로 참조

Sting str1 = "String Instance";
String str2 = "My String";
```

```
// println 메소드의 매개변수형이 String이기 때문에 이러한 문장의 구성이 가능하다.

System.out.println("Hello JAVA!");
System.out.println("My Coffee");
```

<br>

`System.out.println("Hello JAVA!");`

System.out.println 메소드를 호출하면서 실질적으로 전달이 되는 것은 "Hello JAVA!"라는 문자열 자체가 아니다.

문자열 "Hello JAVA!"가 선언되면서 생성된 **인스턴스의 참조값**이 전달이 된다.


`println(String str)`

println이라는 메소드는 인자값으로 String, 매개변수로 str 과 같은 형태로 선언되어 있고

println 메소드를 호출하면 문자열 뿐만 아니라 숫자들도 전달하는데

전달할 수 있는 자료형이 두 가지 이상인 것을 통해 println이 **메소드 오버로딩**이 되어 있다는 것을 유추할 수 있다.

※ [System.out.println()의 오버로딩 형태](https://java.ihoney.pe.kr/99)

```java
public class StringInstance {
    public static void main(String[] args)
    {
        java.lang.String str="My name is Sunny";
        int strLen1=str.length();
        System.out.println("길이 1 : "+strLen1);
        int strLen2="한글의 길이는 어떻게?".length();  // 문자열의 선언은 인스턴스의 생성으로 이어짐을 보이는 문장
        System.out.println("길이 2 : "+strLen2);
    }
}
```

<br>

### String 인스턴스는 상수 형태의 인스턴스이다.

- String 인스턴스에 저장된 문자열의 내용은 변경이 불가능하다.

- 이는 동일한 문자열의 **인스턴스 하나만 생성해서 공유**하기 위함이다.

```java
public static void main(String[] args)
{
    String str1="My String";
    String str2="My String";
    String str3="Your String";
    
    if(str1==str2)    // ★ 비교 연산을 할 경우에는 데이터 비교가 아닌 "참조값" 비교
        System.out.println("동일 인스턴스 참조");
    else
        System.out.println("다른 인스턴스 참조");
    
}

// String 인스턴스의 문자열 변경이 불가능하기 때문에 둘 이상의 참조변수가 동시에 참조를 해도 문제가 발생하지 않는다.
```

<br>

## 11-3. API Document의 참조를 통한 String 클래스의 인스턴스 메소드 관찰

```java
public static void main(String[] args)
{
    String str1="Lemon"+"ade";  // String str1="Lemon".concat("ade");
    String str2="Lemon"+'A';    // String str2="Lemon".concat(String.valueOf('A'));
    String str3="Lemon"+3;      // String str3="Lemon".concat(String.valueOf(3));
    String str4=1+"Lemon"+2;
    str4+='!';
}
```

위 예제의 str4의 선언은 아래와 같이 처리된다고 생각할 수도 있다.

`String str4=String.valueOf(1).concat("Lemon").concat(String.valueOf(2));` → 이대로라면 4개의 인스턴스 생성

하지만 아무리 많은 + 연산을 취하더라도 **StringBulider 클래스**의 도움으로 딱 '2 개'의 인스턴스만 생성된다.

<br>

## 11-4. StringBuilder & StringBuffer 클래스

### StringBuilder

- StringBuilder는 문자열의 저장 및 **변경**을 위한 메모리 공간을 지니는 클래스
  
  - String 클래스의 인스턴스는 변경 불가능한 상수의 성격을 지니지만 StringBuilder는 변수의 성격을 지닌다.

- 문자열 데이터의 추가를 위한 append와 삽입을 위한 insert 메소드 제공

```java
class BuilderString
{
    public static void main(String[] args)
    {
        StringBuilder strBuf=new StringBuilder("AB");   // buf: AB
        strBuf.append(25);    // buf: AB25
        strBuf.append('Y').append(true);    // buf: AB25Ytrue
        System.out.println(strBuf);
        
        strBuf.insert(2, false);    // buf: ABfalse25Ytrue
        strBuf.insert(strBuf.length(), 'Z');    // buf: ABfalse25YtrueZ
        System.out.println(strBuf);
    }
}
```

<br>

### 참조를 반환하는 메소드

- this의 반환은 인스턴스 자신의 참조 값 반환을 의미한다.

- 그리고 이렇게 반환돠는 참조 값을 대상으로 연이은 함수호출이 가능하다.

```java
class SimpleAdder
{
    private int num;
    public SimpleAdder() {num=0;}
    public SimpleAdder add(int num)
    {
        this.num+=num;
        return this;
    }
    public void showResult()
    {
        System.out.println("add result : "+num");
    }
}

public static void main(String[] args)
{
    SimpleAdder adder=new SimpleAdder();
    adder.add(1).add(3).add(5).showResult();
}

// 실행결과
// add result : 9
```

<br>

### StringBuilder의 버퍼와 문자열 조합

- 추가되는 데이터 크기에 따라서 버퍼의 크기가 자동으로 확장된다.

- 생성자를 통해서 초기 버퍼의 크기를 지정할 수 있다.

```
public StringBuilder()  // 기본 16개의 문자 저장 버퍼 생성

public StringBuilder(int capacity)    // capacity개의 문자저장 버퍼 생성

public StringBuilder(String str)    // str.length()+16 크기의 버퍼 생성

```

- 문자열의 복잡한 조합의 과정에서는 StringBuilder의 인스턴스가 활용된다.

- 때문에 추가로 생성되는 인스턴스의 수는 최대 두 개이다!

> Sting str4=1+"Lemon"+2

**new StingBuilder()**.append(1).append("Lemon").append(2).**toString()**;

   └> StringBuilder 인스턴스의 생성에서 한 개, toString 메소드의 호출에 의해서 한 개
   
<br>

### StringBuffer 클래스

- StringBuffer 클래스와 StringBuilder 클래스의 공통점 세 가지
  
  - 메소드의 수 (생성자 포함)
  
  - 메소드의 기능
  
  - 메소드의 이름가 매개변수형
  
- StringBuffer 클래스와 StringBuilder 클래스의 **차이점**

  - **StingBuffer는 쓰레드에 안전, StringBuilder는 쓰레드에 불안전**
  
  - StringBuilder는 변경가능한 문자열이지만 synchronization이 적용되지 않았다. 하지만 StringBuffer는 thread-safe라는 말에서처럼 변경가능하지만 multiple thread환경에서 안전한 클래스라고 한다.

