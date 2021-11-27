# [Java] 문자열 더하기 연산의 내부 구현 - String, StringBuilder, StringBuffer

Java를 공부하다보면 String은 불변<small>(immutable)</small> 객체라는 이야기를 많이 듣게 된다.

```java
String text = "Java";
text = text + " String";
System.out.println(text); // Java String
```

아니 위 코드처럼 String 값이 잘만 변하는데 무슨 말이야! 라고 생각될 수 있지만 **Java의 String 객체는 변하지 않는다.** JDK 5 이전에는 String 문자열을 더하면 새로운 String 객체가 생성되고, 기존 객체는 쓰레기가 되어 나중에 GC<small>(Garbage Collection)</small>의 대상이 된다. 이것이 반복되면 쓰레기가 계속 생성될 것이고 성능적인 문제점을 야기한다.

<p align = 'center'>
<img width = '900' src = 'https://user-images.githubusercontent.com/39554623/143585132-463dee44-ab86-41ea-a508-ee1c8a0a6ec4.png'>
</p>


이러한 String 클래스의 단점을 보완하기 위해 나온 클래스가 바로 **StringBuffer**와 **StringBuilder**다. 두 클래스에서 제공하는 메소드는 동일하다. StringBuffer와 StringBuilder는 문자열을 더해도 새로운 객체를 생성하지 않는다. 두 클래스는 `append()` 메소드를 가장 많이 사용하며, 매개 변수로 모든 기본 자료형과 참조 자료형을 포함한다.

> StringBuilder 예시

```java
StringBuilder sb = new StringBuilder();
sb.append("Java");
sb.append(" String");
System.out.println(sb); // Java String
```

> Method chaining이 가능한 `append()`

참고로 `append()` 메소드를 수행하면 해당 StringBuilder 객체, 즉 자신을 리턴하므로 다음과 같이 [Method chaining](https://en.wikipedia.org/wiki/Method_chaining)을 사용할 수 있다.

```java
StringBuilder sb = new StringBuilder();
sb.append("Java").append(" String");
```

||String|StringBuilder|StringBuffer|
|:--|:--|:--|:--|
|불변성|O|X|X|
|쓰레드 안전(Thread safe)|O|X|O|
|동기화(synchronized)|O|X|O|

```console
Thread safety
Thread safety safety
```

Java에서 문자열을 연결하기 위해 String의 더하기 연산<small>(`+`, 문자열 연결 연산)</small>을 사용하는 경우가 많다. 

```java
public class StringConcat {
    public static void main(String[] args) {
        String s1 = "Java ";
        String s2 = "String Concatenation";
        String s3 = s1 + s2;
        
        System.out.println(s3);
    }
}
```

> JDK 8

```kotlin
public class StringConcat {
  public StringConcat();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: ldc           #2                  // String Java
       2: astore_1
       3: ldc           #3                  // String String Concatenation
       5: astore_2
       6: new           #4                  // class java/lang/StringBuilder
       9: dup
      10: invokespecial #5                  // Method java/lang/StringBuilder."<init>":()V
      13: aload_1
      14: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      17: aload_2
      18: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      21: invokevirtual #7                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      24: astore_3
      25: getstatic     #8                  // Field java/lang/System.out:Ljava/io/PrintStream;
      28: aload_3
      29: invokevirtual #9                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      32: ldc           #10                 // String A
      34: astore        4
      36: iconst_0
      37: istore        5
      39: iload         5
      41: bipush        10
      43: if_icmpge     74
      46: new           #4                  // class java/lang/StringBuilder
      49: dup
      50: invokespecial #5                  // Method java/lang/StringBuilder."<init>":()V
      53: aload         4
      55: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      58: ldc           #11                 // String B
      60: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      63: invokevirtual #7                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      66: astore        4
      68: iinc          5, 1
      71: goto          39
      74: getstatic     #8                  // Field java/lang/System.out:Ljava/io/PrintStream;
      77: aload         4
      79: invokevirtual #9                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      82: return
}

```

> JDK 11

```java
public class StringConcat {
  public StringConcat();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: ldc           #7                  // String Java
       2: astore_1
       3: ldc           #9                  // String String Concatenation
       5: astore_2
       6: aload_1
       7: aload_2
       8: invokedynamic #11,  0             // InvokeDynamic #0:makeConcatWithConstants:(Ljava/lang/String;Ljava/lang/String;)Ljava/lang/String;
      13: astore_3
      14: getstatic     #15                 // Field java/lang/System.out:Ljava/io/PrintStream;
      17: aload_3
      18: invokevirtual #21                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      21: ldc           #27                 // String A
      23: astore        4
      25: iconst_0
      26: istore        5
      28: iload         5
      30: bipush        10
      32: if_icmpge     50
      35: aload         4
      37: invokedynamic #29,  0             // InvokeDynamic #1:makeConcatWithConstants:(Ljava/lang/String;)Ljava/lang/String;
      42: astore        4
      44: iinc          5, 1
      47: goto          28
      50: getstatic     #15                 // Field java/lang/System.out:Ljava/io/PrintStream;
      53: aload         4
      55: invokevirtual #21                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      58: return
}

```



String의 더하기 연산<small>(`+`, 문자열 연결 연산)</small>을 할 경우, JDK 5 이상에서는 컴파일할 때 자동으로 해당 연산을 StringBuilder로 변환해주는데 왜 이펙티브 자바 item 63에서 문자열 연결은 느리니 주의하라고 하는 것일까?

(그런데 JAVA 9부터는 makeConcatWithConstants 라는 메소드를 호출하는 방식으로 변경되었으니 확인이 필요하다)

[Java] String '+'문자열 연결 연산은 내부에서 어떻게 이루어질까? (tistory.com)

How is String concatenation implemented in Java 9? - Stack Overflow

## References

- https://jinseongsoft.tistory.com/369
- https://stackoverflow.com/questions/46512888