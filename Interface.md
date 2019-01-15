# 인터페이스와 추상 클래스

## 1. 인터페이스의 기본과 그 의미

> 인터페이스

- 제공되는 기능을 활용하는 방법

- 통신 도구·수단

- 클래스의 내부적인 내용을 감추고 쉽게 사용할 수 있도록 하는 장치

<br>

### 추상 메소드만 담고 있는 인터페이스

```java
interface Printable {
    public void print(String doc);    // 추상 메소드
    
    // 인터페이스의 정의 : 메소드의 몸체를 갖지 않는다. (클래스가 아니다.)
    // 따라서 인스턴스 생성 불가, 참조변수 선언 가능
}

class Printer implements Printable {  // Printer 클래스에서 print 메소드를 정의
    @Override     // 구현하는 메소드와 추상 메소드 사이에도 메소드 오버라이딩 관계 성립
    public void print(String doc) {
        System.out.println(doc);
    }
}

class PrintableInterface {
    public static void main(String[] args) {
        Printable prn = new Printer();    // 인터페이스형 참조변수 선언 가능
        // 인터페이스형 참조변수는 Printable 인터페이스를 구현한 클래스 Printer의 인스턴스를 참조 가능
        prn.print("Hello Java");
    }
}
```

<br>

### 상속과 구현

```java
class Robot extends Machine implements Movable, Runnable {. . .}
```

- Robot 클래스는 Machine 클래스를 상속한다.

  - 상속과 구현이 동시에 가능하다.

- Robot 클래스는 Movable과 Runnable 인터페이스를 구현한다.

  - 둘 이상의 인터페이스 구현도 가능하다.

<br>

## 2. 인터페이스의 문법 구성과 추상 클래스

### 인터페이스에 선언되는 메소드와 변수

```java
interface Printable {
  public void print(String doc);  // 추상 메소드
  
  // 인터페이스에는 접근 수준 지시자를 지정하지 않아도 기본으로 'public'으로 지정된다.
}

interface Printable {   // 인터페이스의 인스턴스 생성은 불가능하지만 상수 선언은 가능하다.
  public static final int PAPER_WIDTH = 70;
  public static final int PAPER_HEIGHT = 120;
  public void print(String doc);
  
  // static final이 없어도 자동으로 선언된 것으로 간주됨.
}
```

<br>

### 인터페이스 간 상속: 문제 상황의 제시

```java
interface Printable {
    public void print(String doc);
    // public void printCMYK(String doc);
    // 컬러 출력, 즉 메소드가 추가적으로 필요하게 되면 시스템 전체에 문제가 발생
    // 기존에 잘 동작하던 메소드를 수정해야함 → 문제 발생
}

class SPrinterDriver implements Printable {
    @Override
    public void print(String doc) {. . .}
}     // 이 클래스에서 printCMYK 메소드 구현해야 함

class LPrinterDriver implements Printable {
    @Override
    public void print(String doc) {. . .}
}     // 이 클래스에서 printCMYK 메소드 구현해야 함
```

<br>

### 제시한 문제의 해결책: 인터페이스의 상속

```java
interface Printable {
    void print(String doc);
}
                        // 인터페이스 간 상속도 extends로 표현
interface ColorPrintable extends Printable {
    void printCMYK(String doc);
}

class SPrinterDriver implements Printable {
    . . .
}     // 기존 클래스 수정할 필요 없음

class Prn909Drv implements ColorPrintable {
    @Override
    public void print(String doc) {
        System.out.println("black & white ver");
        System.out.println(doc);
    }
    
    @Override
    public void printCMYK(String doc) {
        System.out.println("CMYK ver");
        System.out.println(doc);
    }
}
```

<br>

### 인터페이스의 디폴트 메소드: 문제 상황의 제시

> void printCMYK(String doc);

  - 수많은 인터페이스가 존재하는 상황에서 모든 인터페이스에 추상 메소드를 추가하면 수가 크게 불어나 관리가 힘들어진다.

> `default` void printCMYK(String doc) { }  // 디폴트 메소드

  - 디폴트 메소드로 이 문제를 해결하면 **인터페이스의 수가 늘어나지 않는다.**
  
<br>

### 디폴트 메소드의 효과

> 인터페이스 교체 전

```java
interface Printable {
  void print(String doc);
}
```

> 인터페이스 교체 후

```java
interface Printalbe {
  void print(String doc);
  default void printCMYK(String doc) { }
}
```

<br><br>

> 기존에 정의된 클래스:<br>인터페이스 교체로 인해 코드 수정 필요 없음.

```java
class SPrinterDriver implements Printalbe {
  @Override
  public void print(String doc) {. . .}
}
```

> 새로 정의된 클래스

```java
class Prn909Drv implements Printable {
  @Override
  public void print(String doc) {. . .}
  
  @Override
  public void printCMYK(String doc) {. . .}
}
```

<br>

### 인터페이스의 static 메소드

- 인터페이스에도 static 메소드를 정의할 수 있다.

- 그리고 인터페이스의 static 메소드 호출 방법은 클래스의 static 메소드 호출 방법과 같다.

```java
interface Printable {
  static void printLine(String str) {
    System.out.println(str);
  }
  
  default void print(String doc) {
    printLine(doc); // 인터페이스의 static 메소드 호출
  }
}
```

<br>

### 인터페이스 대상의 instanceof 연산

> if(ca instanceof Cake) . . .

<br>

### 인터페이스의 또 다른 용도: Marker 인터페이스 (체크 용도)

> 클래스에 특정 표시를 해 두기 위한 목적으로 정의된 인터페이스를 마커 인터페이스라 한다.<br>마커 인터페이스에는 구현해야 할 메소드가 없는 경우가 흔하다.

```java
interface Upper { }   // 마커 인터페이스
interface Lower { }   // 마커 인터페이스

interface Printable {
    String getContents();
}

class Report implements Printable, Upper {
    String cons;
    
    Report(String cons) {
        this.cons = cons;
    }
    
    public String getContents() {
        return cons;
    }
}

class Printer { 
    public void printContents(Printable doc) {

        if(doc instanceof Upper) {
            System.out.println((doc.getContents()).toUpperCase());
        }
        else if(doc instanceof Lower) {
            System.out.println((doc.getContents()).toLowerCase());
        }
        else
            System.out.println(doc.getContents());
    }
}
```

<br>

### 추상 클래스

> 하나 이상의 추상 메소드를 지니는 클래스를 가리켜 추상 클래스라 한다.<br>그리고 추상 클래스를 대상으로는 **인스턴스 생성이 불가능**하다. 물론 **참조변수 선언은 가능**하다.

  - 해당 클래스의 인스턴스화를 원하지 않고 상위 클래스로써 디자인 한 것. (상속을 강제하기 위해)

```java
public abstract class House {   // 추상 클래스
  public void methodOne() {
    System.out.println("method one");
  }
  
  public abstract void methodTwo();   // 추상 메소드
}
```
