> StringCompare 클래스

```java
package c.string;

public class StringCompare {
    public static void main(String[] args) {
        StringCompare sample = new StringCompare();
        sample.checkCompare();
    }

    public void checkString() {
        String text = "You must know String class.";
        System.out.println("text.length() = "+text.length());
        System.out.println("text.isEmpty() = "+text.isEmpty());
    }

    public void checkCompare() {
        String text = "Check value";
//        String text2 = "Check value";
        String text2 = new String("Check value");

        if (text == text2) {
            System.out.println("text == text2 result is same.");
        } else {
            System.out.println("text == text2 result is different.");
        }

        if (text.equals("Check value")) {
            System.out.println("text.equals(text2) result is same.");
        }
    }
}

```

> `javap -verbose -private StringCompare`

```java
➜  string javap -verbose -private StringCompare 
Warning: File ./StringCompare.class does not contain class StringCompare
Classfile /Users/june/workspace/godofjava/c/string/StringCompare.class
  Last modified 2021. 11. 12.; size 1516 bytes
  SHA-256 checksum 2e43277e2da512affe1086800e5a4614a1b8fbbe40e3d0747afd51ad2bbee88a
  Compiled from "StringCompare.java"
public class c.string.StringCompare
  minor version: 0
  major version: 57
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER
  this_class: #7                          // c/string/StringCompare
  super_class: #2                         // java/lang/Object
  interfaces: 0, fields: 0, methods: 4, attributes: 3
Constant pool:
   #1 = Methodref          #2.#3          // java/lang/Object."<init>":()V
   #2 = Class              #4             // java/lang/Object
   #3 = NameAndType        #5:#6          // "<init>":()V
   #4 = Utf8               java/lang/Object
   #5 = Utf8               <init>
   #6 = Utf8               ()V
   #7 = Class              #8             // c/string/StringCompare
   #8 = Utf8               c/string/StringCompare
   #9 = Methodref          #7.#3          // c/string/StringCompare."<init>":()V
  #10 = Methodref          #7.#11         // c/string/StringCompare.checkCompare:()V
  #11 = NameAndType        #12:#6         // checkCompare:()V
  #12 = Utf8               checkCompare
  #13 = String             #14            // You must know String class.
  #14 = Utf8               You must know String class.
  #15 = Fieldref           #16.#17        // java/lang/System.out:Ljava/io/PrintStream;
  #16 = Class              #18            // java/lang/System
  #17 = NameAndType        #19:#20        // out:Ljava/io/PrintStream;
  #18 = Utf8               java/lang/System
  #19 = Utf8               out
  #20 = Utf8               Ljava/io/PrintStream;
  #21 = Methodref          #22.#23        // java/lang/String.length:()I
  #22 = Class              #24            // java/lang/String
  #23 = NameAndType        #25:#26        // length:()I
  #24 = Utf8               java/lang/String
  #25 = Utf8               length
  #26 = Utf8               ()I
  #27 = InvokeDynamic      #0:#28         // #0:makeConcatWithConstants:(I)Ljava/lang/String;
  #28 = NameAndType        #29:#30        // makeConcatWithConstants:(I)Ljava/lang/String;
  #29 = Utf8               makeConcatWithConstants
  #30 = Utf8               (I)Ljava/lang/String;
  #31 = Methodref          #32.#33        // java/io/PrintStream.println:(Ljava/lang/String;)V
  #32 = Class              #34            // java/io/PrintStream
  #33 = NameAndType        #35:#36        // println:(Ljava/lang/String;)V
  #34 = Utf8               java/io/PrintStream
  #35 = Utf8               println
  #36 = Utf8               (Ljava/lang/String;)V
  #37 = Methodref          #22.#38        // java/lang/String.isEmpty:()Z
  #38 = NameAndType        #39:#40        // isEmpty:()Z
  #39 = Utf8               isEmpty
  #40 = Utf8               ()Z
  #41 = InvokeDynamic      #1:#42         // #1:makeConcatWithConstants:(Z)Ljava/lang/String;
  #42 = NameAndType        #29:#43        // makeConcatWithConstants:(Z)Ljava/lang/String;
  #43 = Utf8               (Z)Ljava/lang/String;
  #44 = String             #45            // Check value
  #45 = Utf8               Check value
  #46 = Methodref          #22.#47        // java/lang/String."<init>":(Ljava/lang/String;)V
  #47 = NameAndType        #5:#36         // "<init>":(Ljava/lang/String;)V
  #48 = String             #49            // text == text2 result is same.
  #49 = Utf8               text == text2 result is same.
  #50 = String             #51            // text == text2 result is different.
  #51 = Utf8               text == text2 result is different.
  #52 = Methodref          #22.#53        // java/lang/String.equals:(Ljava/lang/Object;)Z
  #53 = NameAndType        #54:#55        // equals:(Ljava/lang/Object;)Z
  #54 = Utf8               equals
  #55 = Utf8               (Ljava/lang/Object;)Z
  #56 = String             #57            // text.equals(text2) result is same.
  #57 = Utf8               text.equals(text2) result is same.
  #58 = Utf8               Code
  #59 = Utf8               LineNumberTable
  #60 = Utf8               main
  #61 = Utf8               ([Ljava/lang/String;)V
  #62 = Utf8               checkString
  #63 = Utf8               StackMapTable
  #64 = Utf8               SourceFile
  #65 = Utf8               StringCompare.java
  #66 = Utf8               BootstrapMethods
  #67 = MethodHandle       6:#68          // REF_invokeStatic java/lang/invoke/StringConcatFactory.makeConcatWithConstants:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;
  #68 = Methodref          #69.#70        // java/lang/invoke/StringConcatFactory.makeConcatWithConstants:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;
  #69 = Class              #71            // java/lang/invoke/StringConcatFactory
  #70 = NameAndType        #29:#72        // makeConcatWithConstants:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;
  #71 = Utf8               java/lang/invoke/StringConcatFactory
  #72 = Utf8               (Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;
  #73 = String             #74            // text.length() = \u0001
  #74 = Utf8               text.length() = \u0001
  #75 = String             #76            // text.isEmpty() = \u0001
  #76 = Utf8               text.isEmpty() = \u0001
  #77 = Utf8               InnerClasses
  #78 = Class              #79            // java/lang/invoke/MethodHandles$Lookup
  #79 = Utf8               java/lang/invoke/MethodHandles$Lookup
  #80 = Class              #81            // java/lang/invoke/MethodHandles
  #81 = Utf8               java/lang/invoke/MethodHandles
  #82 = Utf8               Lookup
{
  public c.string.StringCompare();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 3: 0

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=2, args_size=1
         0: new           #7                  // class c/string/StringCompare
         3: dup
         4: invokespecial #9                  // Method "<init>":()V
         7: astore_1
         8: aload_1
         9: invokevirtual #10                 // Method checkCompare:()V
        12: return
      LineNumberTable:
        line 5: 0
        line 6: 8
        line 7: 12

  public void checkString();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=2, locals=2, args_size=1
         0: ldc           #13                 // String You must know String class.
         2: astore_1
         3: getstatic     #15                 // Field java/lang/System.out:Ljava/io/PrintStream;
         6: aload_1
         7: invokevirtual #21                 // Method java/lang/String.length:()I
        10: invokedynamic #27,  0             // InvokeDynamic #0:makeConcatWithConstants:(I)Ljava/lang/String;
        15: invokevirtual #31                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        18: getstatic     #15                 // Field java/lang/System.out:Ljava/io/PrintStream;
        21: aload_1
        22: invokevirtual #37                 // Method java/lang/String.isEmpty:()Z
        25: invokedynamic #41,  0             // InvokeDynamic #1:makeConcatWithConstants:(Z)Ljava/lang/String;
        30: invokevirtual #31                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        33: return
      LineNumberTable:
        line 10: 0
        line 11: 3
        line 12: 18
        line 13: 33

  public void checkCompare();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=3, locals=3, args_size=1
         0: ldc           #44                 // String Check value
         2: astore_1
         3: new           #22                 // class java/lang/String
         6: dup
         7: ldc           #44                 // String Check value
         9: invokespecial #46                 // Method java/lang/String."<init>":(Ljava/lang/String;)V
        12: astore_2
        13: aload_1
        14: aload_2
        15: if_acmpne     29
        18: getstatic     #15                 // Field java/lang/System.out:Ljava/io/PrintStream;
        21: ldc           #48                 // String text == text2 result is same.
        23: invokevirtual #31                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        26: goto          37
        29: getstatic     #15                 // Field java/lang/System.out:Ljava/io/PrintStream;
        32: ldc           #50                 // String text == text2 result is different.
        34: invokevirtual #31                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        37: aload_1
        38: ldc           #44                 // String Check value
        40: invokevirtual #52                 // Method java/lang/String.equals:(Ljava/lang/Object;)Z
        43: ifeq          54
        46: getstatic     #15                 // Field java/lang/System.out:Ljava/io/PrintStream;
        49: ldc           #56                 // String text.equals(text2) result is same.
        51: invokevirtual #31                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        54: return
      LineNumberTable:
        line 16: 0
        line 18: 3
        line 20: 13
        line 21: 18
        line 23: 29
        line 26: 37
        line 27: 46
        line 29: 54
      StackMapTable: number_of_entries = 3
        frame_type = 253 /* append */
          offset_delta = 29
          locals = [ class java/lang/String, class java/lang/String ]
        frame_type = 7 /* same */
        frame_type = 16 /* same */
}
SourceFile: "StringCompare.java"
BootstrapMethods:
  0: #67 REF_invokeStatic java/lang/invoke/StringConcatFactory.makeConcatWithConstants:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;
    Method arguments:
      #73 text.length() = \u0001
  1: #67 REF_invokeStatic java/lang/invoke/StringConcatFactory.makeConcatWithConstants:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;
    Method arguments:
      #75 text.isEmpty() = \u0001
InnerClasses:
  public static final #82= #78 of #80;    // Lookup=class java/lang/invoke/MethodHandles$Lookup of class java/lang/invoke/MethodHandles
```

> 문자열 리터럴 사용 `"~~"`

```java
➜  string javap -verbose -private StringPoolTest
Warning: File ./StringPoolTest.class does not contain class StringPoolTest
Classfile /Users/june/workspace/godofjava/c/string/StringPoolTest.class
  Last modified 2021. 11. 12.; size 701 bytes
  SHA-256 checksum 405531ce8698dff5123a07d6c0b0397059bf9de3c17a1154c75dc1741dd5f2e0
  Compiled from "StringPoolTest.java"
public class c.string.StringPoolTest
  minor version: 0
  major version: 57
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER
  this_class: #7                          // c/string/StringPoolTest
  super_class: #2                         // java/lang/Object
  interfaces: 0, fields: 0, methods: 3, attributes: 1
Constant pool:
   #1 = Methodref          #2.#3          // java/lang/Object."<init>":()V
   #2 = Class              #4             // java/lang/Object
   #3 = NameAndType        #5:#6          // "<init>":()V
   #4 = Utf8               java/lang/Object
   #5 = Utf8               <init>
   #6 = Utf8               ()V
   #7 = Class              #8             // c/string/StringPoolTest
   #8 = Utf8               c/string/StringPoolTest
   #9 = Methodref          #7.#3          // c/string/StringPoolTest."<init>":()V
  #10 = String             #11            // HelloWorld
  #11 = Utf8               HelloWorld
  #12 = Methodref          #7.#13         // c/string/StringPoolTest.stringTest:(Ljava/lang/String;)V
  #13 = NameAndType        #14:#15        // stringTest:(Ljava/lang/String;)V
  #14 = Utf8               stringTest
  #15 = Utf8               (Ljava/lang/String;)V
  #16 = Class              #17            // c/string/StringTestArea
  #17 = Utf8               c/string/StringTestArea
  #18 = Methodref          #16.#3         // c/string/StringTestArea."<init>":()V
  #19 = Fieldref           #20.#21        // java/lang/System.out:Ljava/io/PrintStream;
  #20 = Class              #22            // java/lang/System
  #21 = NameAndType        #23:#24        // out:Ljava/io/PrintStream;
  #22 = Utf8               java/lang/System
  #23 = Utf8               out
  #24 = Utf8               Ljava/io/PrintStream;
  #25 = Methodref          #16.#26        // c/string/StringTestArea.TestString1:()Ljava/lang/String;
  #26 = NameAndType        #27:#28        // TestString1:()Ljava/lang/String;
  #27 = Utf8               TestString1
  #28 = Utf8               ()Ljava/lang/String;
  #29 = Methodref          #30.#31        // java/lang/String.equals:(Ljava/lang/Object;)Z
  #30 = Class              #32            // java/lang/String
  #31 = NameAndType        #33:#34        // equals:(Ljava/lang/Object;)Z
  #32 = Utf8               java/lang/String
  #33 = Utf8               equals
  #34 = Utf8               (Ljava/lang/Object;)Z
  #35 = Methodref          #36.#37        // java/io/PrintStream.println:(Z)V
  #36 = Class              #38            // java/io/PrintStream
  #37 = NameAndType        #39:#40        // println:(Z)V
  #38 = Utf8               java/io/PrintStream
  #39 = Utf8               println
  #40 = Utf8               (Z)V
  #41 = Utf8               Code
  #42 = Utf8               LineNumberTable
  #43 = Utf8               main
  #44 = Utf8               ([Ljava/lang/String;)V
  #45 = Utf8               SourceFile
  #46 = Utf8               StringPoolTest.java
{
  public c.string.StringPoolTest();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 3: 0

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=2, args_size=1
         0: new           #7                  // class c/string/StringPoolTest
         3: dup
         4: invokespecial #9                  // Method "<init>":()V
         7: astore_1
         8: aload_1
         9: ldc           #10                 // String HelloWorld
        11: invokevirtual #12                 // Method stringTest:(Ljava/lang/String;)V
        14: return
      LineNumberTable:
        line 7: 0
        line 8: 8
        line 10: 14

  public void stringTest(java.lang.String);
    descriptor: (Ljava/lang/String;)V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=3, locals=3, args_size=2
         0: new           #16                 // class c/string/StringTestArea
         3: dup
         4: invokespecial #18                 // Method c/string/StringTestArea."<init>":()V
         7: astore_2
         8: getstatic     #19                 // Field java/lang/System.out:Ljava/io/PrintStream;
        11: aload_2
        12: invokevirtual #25                 // Method c/string/StringTestArea.TestString1:()Ljava/lang/String;
        15: aload_1
        16: invokevirtual #29                 // Method java/lang/String.equals:(Ljava/lang/Object;)Z
        19: invokevirtual #35                 // Method java/io/PrintStream.println:(Z)V
        22: return
      LineNumberTable:
        line 13: 0
        line 15: 8
        line 17: 22
}
SourceFile: "StringPoolTest.java"
```

> `new String("")` 이용

```java
➜  string javap -verbose -private StringPoolTest
Warning: File ./StringPoolTest.class does not contain class StringPoolTest
Classfile /Users/june/workspace/godofjava/c/string/StringPoolTest.class
  Last modified 2021. 11. 12.; size 724 bytes
  SHA-256 checksum 704df050ccc2d564e7bf6c250a6cd7cca235e999c651ac99d8818f1a1d3f6407
  Compiled from "StringPoolTest.java"
public class c.string.StringPoolTest
  minor version: 0
  major version: 57
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER
  this_class: #7                          // c/string/StringPoolTest
  super_class: #2                         // java/lang/Object
  interfaces: 0, fields: 0, methods: 3, attributes: 1
Constant pool:
   #1 = Methodref          #2.#3          // java/lang/Object."<init>":()V
   #2 = Class              #4             // java/lang/Object
   #3 = NameAndType        #5:#6          // "<init>":()V
   #4 = Utf8               java/lang/Object
   #5 = Utf8               <init>
   #6 = Utf8               ()V
   #7 = Class              #8             // c/string/StringPoolTest
   #8 = Utf8               c/string/StringPoolTest
   #9 = Methodref          #7.#3          // c/string/StringPoolTest."<init>":()V
  #10 = Class              #11            // java/lang/String
  #11 = Utf8               java/lang/String
  #12 = String             #13            // HelloWorld
  #13 = Utf8               HelloWorld
  #14 = Methodref          #10.#15        // java/lang/String."<init>":(Ljava/lang/String;)V
  #15 = NameAndType        #5:#16         // "<init>":(Ljava/lang/String;)V
  #16 = Utf8               (Ljava/lang/String;)V
  #17 = Methodref          #7.#18         // c/string/StringPoolTest.stringTest:(Ljava/lang/String;)V
  #18 = NameAndType        #19:#16        // stringTest:(Ljava/lang/String;)V
  #19 = Utf8               stringTest
  #20 = Class              #21            // c/string/StringTestArea
  #21 = Utf8               c/string/StringTestArea
  #22 = Methodref          #20.#3         // c/string/StringTestArea."<init>":()V
  #23 = Fieldref           #24.#25        // java/lang/System.out:Ljava/io/PrintStream;
  #24 = Class              #26            // java/lang/System
  #25 = NameAndType        #27:#28        // out:Ljava/io/PrintStream;
  #26 = Utf8               java/lang/System
  #27 = Utf8               out
  #28 = Utf8               Ljava/io/PrintStream;
  #29 = Methodref          #20.#30        // c/string/StringTestArea.TestString1:()Ljava/lang/String;
  #30 = NameAndType        #31:#32        // TestString1:()Ljava/lang/String;
  #31 = Utf8               TestString1
  #32 = Utf8               ()Ljava/lang/String;
  #33 = Methodref          #10.#34        // java/lang/String.equals:(Ljava/lang/Object;)Z
  #34 = NameAndType        #35:#36        // equals:(Ljava/lang/Object;)Z
  #35 = Utf8               equals
  #36 = Utf8               (Ljava/lang/Object;)Z
  #37 = Methodref          #38.#39        // java/io/PrintStream.println:(Z)V
  #38 = Class              #40            // java/io/PrintStream
  #39 = NameAndType        #41:#42        // println:(Z)V
  #40 = Utf8               java/io/PrintStream
  #41 = Utf8               println
  #42 = Utf8               (Z)V
  #43 = Utf8               Code
  #44 = Utf8               LineNumberTable
  #45 = Utf8               main
  #46 = Utf8               ([Ljava/lang/String;)V
  #47 = Utf8               SourceFile
  #48 = Utf8               StringPoolTest.java
{
  public c.string.StringPoolTest();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 3: 0

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
    Code:
      stack=3, locals=3, args_size=1
         0: new           #7                  // class c/string/StringPoolTest
         3: dup
         4: invokespecial #9                  // Method "<init>":()V
         7: astore_1
         8: new           #10                 // class java/lang/String
        11: dup
        12: ldc           #12                 // String HelloWorld
        14: invokespecial #14                 // Method java/lang/String."<init>":(Ljava/lang/String;)V
        17: astore_2
        18: aload_1
        19: aload_2
        20: invokevirtual #17                 // Method stringTest:(Ljava/lang/String;)V
        23: return
      LineNumberTable:
        line 7: 0
        line 8: 8
        line 10: 18
        line 12: 23

  public void stringTest(java.lang.String);
    descriptor: (Ljava/lang/String;)V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=3, locals=3, args_size=2
         0: new           #20                 // class c/string/StringTestArea
         3: dup
         4: invokespecial #22                 // Method c/string/StringTestArea."<init>":()V
         7: astore_2
         8: getstatic     #23                 // Field java/lang/System.out:Ljava/io/PrintStream;
        11: aload_2
        12: invokevirtual #29                 // Method c/string/StringTestArea.TestString1:()Ljava/lang/String;
        15: aload_1
        16: invokevirtual #33                 // Method java/lang/String.equals:(Ljava/lang/Object;)Z
        19: invokevirtual #37                 // Method java/io/PrintStream.println:(Z)V
        22: return
      LineNumberTable:
        line 15: 0
        line 17: 8
        line 20: 22
}
SourceFile: "StringPoolTest.java"
```


```java
package c.string;

public class StringPoolTest {
    public static void main(String[] args) {
        StringPoolTest spt = new StringPoolTest();
        String hw = new String("HelloWorld");
        String hw2 = "HelloWorld";
        String hw3 = "HelloWorld";

        System.out.println(hw == hw2);
        System.out.println(hw2 == hw3);
    }
}
```

```java
➜  string javap -verbose -private StringPoolTest
Warning: File ./StringPoolTest.class does not contain class StringPoolTest
Classfile /Users/june/workspace/godofjava/c/string/StringPoolTest.class
  Last modified 2021. 11. 12.; size 685 bytes
  SHA-256 checksum 070259794fbee763859d404310d1aea53a825dc3bda2f6eaa7d10c5c31c9140a
  Compiled from "StringPoolTest.java"
public class c.string.StringPoolTest
  minor version: 0
  major version: 57
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER
  this_class: #7                          // c/string/StringPoolTest
  super_class: #2                         // java/lang/Object
  interfaces: 0, fields: 0, methods: 2, attributes: 1
Constant pool:
   #1 = Methodref          #2.#3          // java/lang/Object."<init>":()V
   #2 = Class              #4             // java/lang/Object
   #3 = NameAndType        #5:#6          // "<init>":()V
   #4 = Utf8               java/lang/Object
   #5 = Utf8               <init>
   #6 = Utf8               ()V
   #7 = Class              #8             // c/string/StringPoolTest
   #8 = Utf8               c/string/StringPoolTest
   #9 = Methodref          #7.#3          // c/string/StringPoolTest."<init>":()V
  #10 = Class              #11            // java/lang/String
  #11 = Utf8               java/lang/String
  #12 = String             #13            // HelloWorld
  #13 = Utf8               HelloWorld
  #14 = Methodref          #10.#15        // java/lang/String."<init>":(Ljava/lang/String;)V
  #15 = NameAndType        #5:#16         // "<init>":(Ljava/lang/String;)V
  #16 = Utf8               (Ljava/lang/String;)V
  #17 = Fieldref           #18.#19        // java/lang/System.out:Ljava/io/PrintStream;
  #18 = Class              #20            // java/lang/System
  #19 = NameAndType        #21:#22        // out:Ljava/io/PrintStream;
  #20 = Utf8               java/lang/System
  #21 = Utf8               out
  #22 = Utf8               Ljava/io/PrintStream;
  #23 = Methodref          #24.#25        // java/io/PrintStream.println:(Z)V
  #24 = Class              #26            // java/io/PrintStream
  #25 = NameAndType        #27:#28        // println:(Z)V
  #26 = Utf8               java/io/PrintStream
  #27 = Utf8               println
  #28 = Utf8               (Z)V
  #29 = Utf8               Code
  #30 = Utf8               LineNumberTable
  #31 = Utf8               main
  #32 = Utf8               ([Ljava/lang/String;)V
  #33 = Utf8               StackMapTable
  #34 = Class              #35            // "[Ljava/lang/String;"
  #35 = Utf8               [Ljava/lang/String;
  #36 = Utf8               SourceFile
  #37 = Utf8               StringPoolTest.java
{
  public c.string.StringPoolTest();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 3: 0

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
    Code:
      stack=3, locals=5, args_size=1
         0: new           #7                  // class c/string/StringPoolTest
         3: dup
         4: invokespecial #9                  // Method "<init>":()V
         7: astore_1
         8: new           #10                 // class java/lang/String
        11: dup
        12: ldc           #12                 // String HelloWorld
        14: invokespecial #14                 // Method java/lang/String."<init>":(Ljava/lang/String;)V
        17: astore_2
        18: ldc           #12                 // String HelloWorld
        20: astore_3
        21: ldc           #12                 // String HelloWorld
        23: astore        4
        25: getstatic     #17                 // Field java/lang/System.out:Ljava/io/PrintStream;
        28: aload_2
        29: aload_3
        30: if_acmpne     37
        33: iconst_1
        34: goto          38
        37: iconst_0
        38: invokevirtual #23                 // Method java/io/PrintStream.println:(Z)V
        41: getstatic     #17                 // Field java/lang/System.out:Ljava/io/PrintStream;
        44: aload_3
        45: aload         4
        47: if_acmpne     54
        50: iconst_1
        51: goto          55
        54: iconst_0
        55: invokevirtual #23                 // Method java/io/PrintStream.println:(Z)V
        58: return
      LineNumberTable:
        line 5: 0
        line 6: 8
        line 7: 18
        line 8: 21
        line 10: 25
        line 11: 41
        line 12: 58
      StackMapTable: number_of_entries = 4
        frame_type = 255 /* full_frame */
          offset_delta = 37
          locals = [ class "[Ljava/lang/String;", class c/string/StringPoolTest, class java/lang/String, class java/lang/String, class java/lang/String ]
          stack = [ class java/io/PrintStream ]
        frame_type = 255 /* full_frame */
          offset_delta = 0
          locals = [ class "[Ljava/lang/String;", class c/string/StringPoolTest, class java/lang/String, class java/lang/String, class java/lang/String ]
          stack = [ class java/io/PrintStream, int ]
        frame_type = 79 /* same_locals_1_stack_item */
          stack = [ class java/io/PrintStream ]
        frame_type = 255 /* full_frame */
          offset_delta = 0
          locals = [ class "[Ljava/lang/String;", class c/string/StringPoolTest, class java/lang/String, class java/lang/String, class java/lang/String ]
          stack = [ class java/io/PrintStream, int ]
}
SourceFile: "StringPoolTest.java"
```


## References

- Java – Constant pool과 String pool : http://blog.breakingthat.com/2018/12/21/java-constant-pool%EA%B3%BC-string-pool/
- String pool vs Constant pool : https://stackoverflow.com/questions/23252767/string-pool-vs-constant-pool
- The javap Command - Oracle Help Center : https://docs.oracle.com/en/java/javase/14/docs/specs/man/javap.html
- Java - String Pool에 대해서 : https://memostack.tistory.com/231