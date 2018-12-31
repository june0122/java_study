# Chapter 06. 메소드와 변수의 스코프

> `return` : 값의 반환, 메소드의 종료 두가지 의미를 지님

return이 값을 반환하지 않고 사용된다면 메소드의 종료만을 의미함

중괄호 내부에 선언되는 변수를 **지역변수**라 칭한다.

지역변수는 자신이 선언된 영역을 빠져나가면 자동으로 소멸된다.<br><br>

> 변수의 유효범위: 스코프(scope)

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


### 06-3. 메소드의 재귀호출

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

cf) [String.format 문자열 포맷](http://library1008.tistory.com/5)



07-1. 클래스의 정의와 인스턴스의 생성

java의 객체는 변수와 메소드의 묶음이라 할 수 있다.

변수와 메소드는 따로 노는 것이 아닌 긴밀한 관계 를 맺고 있다.


■ 클래스를 기반으로 객체 생성하기

변수와 메소드를 묶어놓은 것이 클래스.
클래스는 '틀'이다.
틀은 실체화가 되지 않는다. (메모리에 할당이 되지 않는다.)
 
CLASSNAME reference = new CLASSNAME();
   
클래스(틀) ─인스턴스화→ 객체(인스턴스, 실체)


 FruitSeller seller = new FruitSeller();
// 참조변수의 선언     인스턴스의 생성 → 메모리 공간에 할당

//FruitSeller라는 클래스 틀을 통해서 객체를 생성하겠다.
//FruitSeller를 참조할 수 있는 변수 seller

하나의 객체는 둘 이상의 참조 변수를 통해 참조가 가능하다.

■  생성된 객체의 접근방법

// 객체 내에 존재하는 변수의 접근

Fruitseller seller=newFruitSeller();
seller.numOfApple=20;

/*
seller가 참조하고 있는 인스터스의 (안에 존재하는) numOfApple
seller라는 참조변수 안에 존재하는 numOfApple 에 20을 저장하라.
*/

seller.numOfApple 의 . 은 객체의 접근에 사용되는 연산자

// 객체 내에 존재하는 메소드의 접근(호출)

FruitSeller seller = new FruitSeller();
seller.saleApple(10);

/*
seller가 참조하는 인스턴스 안에 존재하는 saleApple이라는 메소드를 호출하면서 10을 전달하라.
*/

java 는 메모리에 직접적 접근이 불가능 (메모리 연산 불가능)
C 는 메모리 연산 가능


07-2. 생성자(Constructor)와 클래스 타입

생성자 
CLASSNAME reference = new CLASSNAME();

개념 : 객체를 생성하는 특별한 메소드
특징 : 리턴타입 명시가 없는 특별한 메소드 / 클래스 이름과 동일
