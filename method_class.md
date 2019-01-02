# Chapter 06. 메소드와 변수의 스코프

> `return` : 값의 반환, 메소드의 종료 두가지 의미를 지님

return이 값을 반환하지 않고 사용된다면 메소드의 종료만을 의미함

중괄호 내부에 선언되는 변수를 **지역변수**라 칭한다.

지역변수는 자신이 선언된 영역을 빠져나가면 자동으로 소멸된다.<br><br>

> 변수의 유효범위: 스코프(scope)

유효범위란 변수를 전역변수, 지역변수 나눠서 좀 더 관리하기 편리하도록 한 것이다. 프로그램이 커지면 여러 가지 이유로 이름이 충돌하게 된다. 이를 해결하기 위해서 고안된 것이 유효범위라는 개념이다. 흔히 스코프(Scope)라고도 부른다.
```java
for(int num=0; num<5; num++)
{
/*추가적인
    변수 num 선언 불가 지역 */
}

public static void myFunc(int num)
{
/*추가적인
    변수 num 선언 불가 지역 */
}
```

위 예시의 int형 매개변수들은 중괄호 내부가 아니라도 지역변수의 성격을 지닌다.<br><br>

> **★ `this`** : 자기 자신 클래스(객체)를 가리킨다.

<br><br>
### *06-3. 메소드의 재귀호출*

재귀적 메소드의 경우 메소드의 처음으로 돌아가는 것이 아니라 재귀 메소드 코드의 *복사본*이 실행된다고 이해하는 것이 재귀의 이해에 도움이 된다.

재귀 메소드의 종료 조건의 검사 위치는 재귀메소드가 재실행되기 이전에 위치해야 한다.<br><br>

# Chapter 07. 클래스와 인스턴스

레고틀 - 레고블럭 - 레고성

클래스 -   객체   - 프로그램<br><br>

> 클래스 = 필드(정보) + 메소드 (동작)

클래스 : 객체의 틀, 설계도

객체 : 클래스를 통해 만들어진 프로그램의 구성요소<br><br>



***■ 기본형 vs 레퍼런스형(참조형) 변수***

- 기본형(primitive types) : 특정값을 직접 저장

    ex) int, long, double, boolean, char, short, byte, float

- 레퍼런스형(reference types) : 특정 개체를 간접적으로 가리키는 변수(객체의 주소를 저장)

    ex) string, int[] ...

- 워드 : CPU가 한 번에 읽을 수 있는 데이터의 크기 (32bit, 64bit 컴퓨터)

    워드 안에 담길 수 있는 변수들이 기본형 변수, 그렇지 않은 것들이 참조형 변수<br><br>

    cf) [String.format 문자열 포맷](http://library1008.tistory.com/5)<br><br>

### *07-1. 클래스의 정의와 인스턴스의 생성*

java의 객체는 변수와 메소드의 묶음이라 할 수 있다.

변수와 메소드는 따로 노는 것이 아닌 긴밀한 관계 를 맺고 있다.<br><br>


***■ 클래스를 기반으로 객체 생성하기***

변수와 메소드를 묶어놓은 것이 클래스.

클래스는 '틀'이다.

틀은 실체화가 되지 않는다. (메모리에 할당이 되지 않는다.)

```java
CLASSNAME reference = new CLASSNAME();
```
   
클래스(틀) ─인스턴스화→ 객체(인스턴스, 실체)

```java
 FruitSeller seller = new FruitSeller();
// 참조변수의 선언     인스턴스의 생성 → 메모리 공간에 할당

//FruitSeller라는 클래스 틀을 통해서 객체를 생성하겠다.
//FruitSeller를 참조할 수 있는 변수 seller
```

하나의 객체는 둘 이상의 참조 변수를 통해 참조가 가능하다.<br><br>

***■  생성된 객체의 접근방법***

```java
// 객체 내에 존재하는 '변수'의 접근

Fruitseller seller=newFruitSeller();
seller.numOfApple=20;

/*
seller가 참조하고 있는 인스터스의 (안에 존재하는) numOfApple
seller라는 참조변수 안에 존재하는 numOfApple 에 20을 저장하라.
*/
```

seller.numOfApple 의 ' . ' 은 객체의 접근에 사용되는 연산자

```java
// 객체 내에 존재하는 '메소드'의 접근(호출)

FruitSeller seller = new FruitSeller();
seller.saleApple(10);

/*
seller가 참조하는 인스턴스 안에 존재하는 saleApple이라는 메소드를 호출하면서 10을 전달하라.
*/
```

java 는 메모리에 직접적 접근이 불가능 (메모리 연산 불가능)

C 는 메모리 연산 가능
<br><br>

***■ 참조변수와 메소드의 관계***

```java
pubilc void myMethod()
{
    FruitSeller seller1 = new FruitSeller();
    instMethod(seller1);
    . . .
}
```

```java
pubilc void instMethod(FruitSeller seller2)
{
    ...
}
```

myMethod라는 메소드 내에서 생성된 인스턴스를 myMethod 밖에 있는 instMethod와 같이 참조값(주소값)만 전달해주면 다른 메소드에서도 접근이 가능하다.
<br><br>
***※ 클래스 변수, 인스턴스 변수, 지역변수***

```java
public class test {

    int iv; // 인스턴스 변수
    static int cv; // 클래스 변수
    
    void method() {
        int lv; // 지역 변수
    }
}
```
|변수의 종류|선언 위치|생성 시기(메모리 할당 시기)|
|----------|----------|----------|
|인스턴스 변수|클래스 영역|인스턴스가 생성될 때|
|클래스 변수|클래스 영역|클래스가 메모리에 올라갈 때|
|지역 변수|클래스 이외의 영역 (메소드, 생성자, 초기화 블럭)|변수 선언문이 수행 되었을 때|

> 인스턴스 변수

class 내부에 위치하며 **객체를 생성해야만 사용**할 수 있다. 인스턴스(객체)로 만들어지는 시점에 메모리에 할당되므로, 인스턴스로 만들어져야지만 실질적인 변수로써 의미를, 역할을 가지므로 인스턴스 변수라 칭한다.

> 클래스 변수

인스턴스 변수와 마찬가지로 class 내부에 위치한다. 인스턴스 변수 앞에 **static**을 붙여주면 클래스 변수가 된다. 인스턴스 변수는 각각 고유한 값을 가지지만 클래스 변수는 모든 인스턴스가 공통된 값을 공유하므로 한 클래스의 모든 인스턴스들이 공통적인 값을 가져야할 때 클래스 변수를 선언한다.

클래스가 로딩될 때 생성되어( ***그러므로 메모리에 딱 한 번만 올라간다.*** ) 종료될 때 까지 유지되는 클래스 변수는 pubilc을 붙이면 같은 프로그램 내에서 어디서든 접근할 수 있는 **전역 변수**가 된다. 또한 인스턴스 변수의 접근법과 다르게 인스턴스를 생성하지 않고 클래스 이름, 클래스 변수명을 통해서 접근할 수 있다.

> 지역 변수

메소드 내에서 선언되며 메소드 내에서만 사용할 수 있는 변수이다. 메소드가 실행될 때 메모리를 할당받으며 메소드가 끝나면 소멸되어 사용할 수 없게 된다.

<br><br>
***■ 참조변수의 null 초기화***

```java
MyInst my = null    // MyInst라는 클래스의 참조변수 my의 선언

if(my==null)
    System.out.println("참조변수 my는 현재 참조하는 객체가 없습니다.");
    
// null은 아무것도 참조하지 않음을 의미하는 키워드
```

<br><br>
### *07-2. 생성자(Constructor)와 클래스 타입*

> 생성자 
```java
CLASSNAME reference = new CLASSNAME();

// new CLASSNAME : CLASSNAME의 인스턴스 생성
// CLASSNAME() : CLASSNAME() 생성자 호출

// 해석 : CLASSNAME이라는 클래스의 인스터스를 생성하고, 그 과정에서 '인자값을 하나도 받지 않는' 생성자를 호출하겠다.
```

개념 : 객체를 생성하는 특별한 메소드

특징 : 리턴타입(반환형)이 선언되어 있지 않으면서, 반환하지 않는 메소드 / 클래스 이름과 동일
<br><br>

***■ 값을 전달받는 생성자***

```java
Number num = new Number(10);
Number num = new Number(30);
```
 ↓
        
> 호출되는 생성자
```java
pubilc Number(int n)
{
    ...
}
```

***★ 생성자 내에서는 final 멤버 변수의 초기화가 가능하다.***
<br><br>

***■ 디폴트 생성자(Default Constructor)***

생성자가 없어도 인스턴스 생성이 가능한 이유는 자동으로 삽입되는 디폴트 생성자에 있다.

<br><br>
### *07-3. 자바의 이름 규칙(Naming Rule)*

***■ 클래스, 메소드, 상수의 이름 규칙***

> 클래스 이름
```java
class MyClass
class ProgrammingBook

/*
Camel Case
    - 대문자로 시작
    - 둘 이상의 단어가 묶여서 하나의 이름 구성 시, 새 단어는 대문자로 시작
*/
```

> 인스턴스 변수, 메소드 이름
```java
int addYourMoney(int Money)
int yourAge

/*
변형된 Camel Case
    - 소문자로 시작
    - 둘 이상의 단어가 묶여서 하나의 이름 구성 시, 새 단어는 소문자로 시작
*/
```

> 상수 이름
```java
final int COLOR=7
final int COLOR_RAINBOW=7

/*
    - 전부 대문자로 표현
    - 둘 이상의 단어가 묶여서 하나의 이름 구성 시, 두 단어 사이에 _ 
*/
```
