# 21. 제네릭(Generics)

## 21-1. 제네릭 클래스의 이해와 설계

### AppleBox와 OrangeBox 클래스의 설계

```java
// Apple 상자와 Orange 상자를 구분한 모델

class AppleBox
{
    Apple item;
    public void store(Apple item) { this.item=item; }
    public Apple pullOut() { return item; }
}

class OrangeBox
{
    Orange item;
    public void store(Orange item) { this.item=item; }
    public Orange pullOut() { return item; }
}
```

```java
// 무엇이든 담을 수 있는 상자 클래스

class FruitBox
{
    Object item;
    public void stroe(Object item) { this.item=item; }
    public Object pullOut() { return item; }
}
```

- 구현의 편의만 놓고 보면, FruitBox 클래스가 더 좋아보인다. 하지만 FruitBox 클래스는 `자료형에 안전하지 못하다`는 단점이 있다.

- 물론 AppleBox와 OrangeBox는 구현의 불편함이 따르는 단점이 있다. 그러나 `자료형에 안전`하다는 장점이 있다.

<br>

> AppleBox, OrangeBox의 **장점인 자료형의 안전성**과 FruitBox의 장점인 **구현의 편의성**을 한데 모은 것이 `제네릭`이다.

`자바의 제네릭과 C++의 템플릿(Template)이 연관된다.`

<br>

### 자료형의 안전성에 대한 논의

```java
public static void main(String[] args)
{
    OrangeBox fBox1=new OrangeBox();
    fBox1.store(new Orange(10));
    Orange org1=(Orange)fBox1.pullOut();
    org1.showSugarContent();
    
    FruitBox fBox2=new FruitBox();
    fBox2.store("오렌지");   // Orange 인스턴스가 아닌 "오렌지"라는 String(문자열) 인스턴스를 담음 → 논리적 오류
    Orange org2=(Orange)fBox2.pullOut();  // 예외 발생지점
    org2.showSugarContent();
}
```

> `fBox2.store("오렌지");`

- Orange 인스턴스가 아닌 "오렌지"라는 String(문자열) 인스턴스를 담는 **논리적 오류**가 발생

  - Object 기반으로 인스턴스를 저장하면 예외가 발생하지 않을 수도 있다.

  - 잘못된 자료형을 기반으로 인스턴스를 저장했음에도 오류가 발생되지 않는 것을 **자료형이 안전하지 않다**는 것을 의미한다.

- 실행 중간에 Class Casting Exception이 발생한다. 그런데 실행 중간에 발생하는 예외는 컴파일 과정에서 발견되는 오류상황보다 발견 및 정정이 어렵다.

  - 즉, 이는 **자료형에 안전한 형태가 아니다.**
  
  <br>

```java
public static void main(String[] args)
{
    OrangeBox fBox1=new OrangeBox();
    fBox1.store(new Orange(10));
    Orange org1=(Orange)fBox1.pullOut();
    org1.showSugarContent();
    
    OrangeBox fBox2=new OrangeBox();
    fBox2.store("오렌지");   // 에러 발생지점
    Orange org2=fBox2.pullOut();
    org2.showSugarContent();
}
```

- 컴파일 과정에서 메소드 store에 문자열 인스턴스가 전달될 수 없이 발견된다.

- 이렇듯 컴파일 과정에서 발견된 자료형 관련 문제는 발견 및 정정이 간단하다.

- 즉, 이는 **자료형에 안전한 형태**이다.

<br>

### 제네릭(Generics) 클래스 설계

```java
class FruitBox
{
    Object item;
    public void store(Object item) {this.item=item;}
    public Object pullOut() {return item;}
}
```

`↓ 제네릭 정의의 결과`

```java
class FruitBox<T>
{
    T item;
    public void store(T item) {this.item=item;}
    public T pullout() {return item;}
}
```

- `T`라는 이름이 매개변수화 된 자료형임을 나타낸다. (자료형 정보를 특정하지 않겠다.)

  - T에 해당하는 자료형의 이름은 클래스를 생성할 때가 아닌, *인스턴스를 생성하는 순간*에 결정이 된다.

<br>

### 제네릭 클래스 기반 인스턴스 생성

`FruitBox<Orange> orBox=new FruitBox<Orange>();`

> T를 Orange로 결정해서 FruitBox의 인스턴스를 생성하고 이를 참조할 수 있는 참조변수를 선언해서 참조값을 저장한다.

`FruitBox<Apple> apBox=new FruitBox<Apple>();`

> T를 Apple로 결정해서 FruitBox의 인스턴스를 생성하고 이를 참조할 수 있는 참조변수를 선언해서 참조값을 저장한다.

```java
public static void main(String[] args)
{
    FruitBox<Orange> orBox=new FruitBox<Orange>();
    orBox.store(new Orange(10));
    Orange org=orgBox.pullOut();
    org.showSugarContent();
    
    FruitBox<Apple> apBox=new FruitBox<Apple>();
    apBox.store(new Apple(10));
    Apple app=apBox.pullOut();
    app.showSugarContent();
}
```

<br>

## 21-2. 제네릭을 구성하는 다양한 문법적 요소 ①

### 제네릭 메소드의 정의와 호출

> 클래스의 메소드만 부분적으로 제네릭화 할 수 있다.

```java
class InstanceTypeShower
{
	int showCnt=0;
	
	public <T> void showInstType(T inst)
	{
		System.out.println(inst);
		showCnt++;
	}
	
	void showPrintCnt() { · · · · · }
}
```

```java
	public static void main(String[] args)
	{
		AAA aaa=new AAA();
		BBB bbb=new BBB();
		
		InstanceTypeShower shower=new InstanceTypeShower();
		shower.<AAA>showInstType(aaa);
		shower.<BBB>showInstType(bbb);
		shower.showPrintCnt();
	}
```

> `shower.showInstType(aaa); → shower.<AAA>showInstType(aaa);`
  
  - aaa가 AAA의 참조변수이므로 위와 같이 표현
  
  - 제네릭 메소드의 호출과정에서 전달되는 인자를 통해서 제네릭 자료형을 결정할 수 있으므로 자료형의 표현은 생략 가능하다.

<br>

### 제네릭 메소드와 둘 이상의 자료형

```java
class InstanceTypeShower2
{
	public <T, U> void showInstType(T inst1, U inst2)
	{
		System.out.println(inst1);
		System.out.println(inst2);
	}
}
```

```java
	public static void main(String[] args)
	{
		AAA aaa=new AAA();
		BBB bbb=new BBB();
		
		InstanceTypeShower2 shower=new InstanceTypeShower2();
		shower.<AAA, BBB>showInstType(aaa, bbb);
		shower.showInstType(aaa, bbb);      // 자료형이 둘 이상일 때도 자료형의 정보는 생략이 가능
	}
```
<br>

### 매개변수의 자료형 제한

> `public static<T extends AAA> void myMethod(T param) { · · · · }`

- 키워드 extends는 매개변수의 자료형을 제한하는 용도로도 사용된다.

- T가 AAA를 상속(AAA가 클래스인 경우) 또는 구현(AAA가 인터페이스인 경우)하는 클래스의 자료형이 되어야 함을 명시함

```java
// SimpleInterface - 인터페이스 이름
public static <T extends SimpleInterface> void showInstanceAncestor(T param)
{
    param,showYourName();
}

// 인자는 SimpleInterface를 구현하는 클래스의 인스턴스여야 한다.
```

```java
//UpperClass - 클래스 이름
public static <T extends UpperClass> void showInterfaceName(T param)
{
    param.showYourAncestor();
}

// UpperClass를 상속하는 클래스의 인스턴스여야 한다.
```

<br>

### 제네릭 메소드와 배열

> 제네릭 메소드로의 배열 전달

- 배열도 인스턴스이므로 제네릭 매개변수에 전달이 가능하다. 하지만 이렇게 전달을 하면 다음과 같은 문장을 쓸 수 없다!

  - System.out.println(arr[i]);
  
- 다음과 같이 매개변수를 선언하면, 매개변수에 전달되는 참조 값을 배열 인스턴스의 참조 값으로 제한할 수 있다.

  - T[] arr
  
- 그리고 이렇게 되면 참조 값은 배열 인스턴스의 참조 값임이 100% 보장되므로 [] 연산을 허용한다.

```java
public static <T> void showArrayData(T[] arr)
{
    for(int i=0; i<arr.length' i++)
      System.out.println(arr[i]);
}
```

> 이렇듯 [] 연산이 필요하다면 매개변수의 선언을 통해서 전달되는 참조 값을 배열의 참조 값으로 제한해야 한다.

<br>

### 제네릭 변수의 참조와 상속의 관계

`public void ohMethod(FruitBox<Fruit> param) { . . . . }`

- ohMethod의 인자로 전달될 수 있는 참조 값의 자료형은

  - `FruitBox<Fruit>`의 인스턴스 참조 값

  - `FruitBox<Fruit>`를 상속하는 인스턴스의 참조 값
    
<br>

### 와일드카드와 제네릭 변수의 선언

> `<? extends Fruit>`

- <? extends Fruit>`가 의미하는 바는 "Fruit을 상속하는 모든 클래스"이다.

  - `FruitBox<> extends Fruit> box1 = new FruitBox<Fruit>();`
  
  - `FruitBox<> extends Fruit> box2 = new FruitBox<Fruit>();`
  
 ```
 `FruitBox<Fruit>` 인스턴스의 참조 값도,
 
 `FruitBox<Apple>` 인스턴스의 참조 값도
 
 인자로 전달받을 수 있는 매개변수의 선언에는 와일드카드 문자 `?` 가 사용된다.
 ```

<br>

### 하위 클래스를 제한하는 용도의 와일드 카드

> FruitBox<? `extends` Apple> boundedBox;<br>(FruitBox는 제네릭 클래스이고 어떠한 자료형을 기반으로 해서 만든 인스턴스라도 참조하되 그 자료형은 반드시 Apple을 상속해야 한다.)

- ~`을` 상속하는 클래스라면 무엇이든지

- Apple을 상속하는 클래스의 인스턴스라면 무엇이든지 참조 가능한 참조변수 선언

<br>

> FruitBox<? `super` Apple> boundedBox;

- ~`이` 상속하는 클래스라면 무엇이든지

- Apple이 상속하는 클래스의 인스턴스라면 무엇이든지 참조 가능한 참조변수 선언

<br>

### 제네릭 클래스의 다양한 상속방법

```java
class AAA<T>
{
    T itemAAA;
}

class BBB<T> extends AAA<T>
{
    T itemBBB;
}
```

```java
class AAA<T>
{
    T itemAAA;
}

class BBB extends AAA<String>
{
    int itemBBB;
}
```

<br>

### 제네릭 인터페이스의 구현 방법

> 제너릭 인터페이스

```java
interace MyInterface<T>
{
    public T myFunc(T item);
}
```

> 제너릭 인터페이스 구현모델 1

```java
class MyImplement<T> implements MyInterface<T>
{
    public T myFunc(T item)
    {
        return item;
    }
}
```

> 제너릭 인터페이스 구현모델 2

```java
class MyImplement implements MyInterface<String>
{
    public String myFunc(String item)
    {
        return item;
    }
}
```

<br>

### 기본자료형의 이름은 제네릭에 사용 불가!

```java
// 컴파일 불가능한 문장들

FruitBox<int> fb1 = new FruitBox<int>();

FruitBox<double> fb1 = new FruitBox<double>();
```

- 기본 자료형 정보를 이용해서는 제네릭 클래스의 인스턴스 생성이 불가능하다.

- 제네릭은 클래스와 인스턴스에 관한 이야기이다.

  - 기본 자료형 정보를 대상으로 제네릭 인스턴스의 생성을 할 때는 Wrapper 클래스를 이용한다.
