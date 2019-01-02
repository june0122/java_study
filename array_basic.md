# 배열(Array)

### 배열의 개념

연관된 데이터를 모아서 관리하기 위해서 사용하는 '데이터 타입'이다.

변수가 하나의 데이터를 저장하기 위한 것이라면 배열은 여러 개의 데이터를 저장하기 위한 것이라고 할 수 있다.

```java
// 배열의 생성

public class DefineDemo {
 
    public static void main(String[] args) {
 
        String[] classGroup = { "최진혁", "최유빈", "한이람", "이고잉" };
        //classGroup 이라는 변수에는 문자열(String)의 데이터로 구성되어있는 배열이 담긴다.
 
    }
 
}
```

### 배열의 제어

```java
public class GetDemo {
 
    public static void main(String[] args) {
        String[] classGroup = { "최진혁", "최유빈", "한이람", "이고잉" };  // 중괄호 내부의 데이터는 elements
        System.out.println(classGroup[0]);
        System.out.println(classGroup[1]);
        System.out.println(classGroup[2]);
        System.out.println(classGroup[3]);
        
        // [] 내부의 숫자는 index, 즉 색인이라 칭한다. 0부터 시작한다.
 
    }
 
}
```

```java
public class LengthDemo {
 
    public static void main(String[] args) {
        String[] classGroup = new String[4];
        
        classGroup[0] = "최진혁";
        System.out.println(classGroup.length);
        // length : 배열을 처음 생성할 때 지정한 배열의 크기를 의미
        classGroup[1] = "최유빈";
        System.out.println(classGroup.length);
        classGroup[2] = "한이람";
        System.out.println(classGroup.length);
        classGroup[3] = "이고잉";
        System.out.println(classGroup.length);
 
    }
 
}
```

### for-each

for-each(향상된 for문)에서는 배열의 항목 수만큼 실행부분을 반복하는데, 반복이 이루어질 때마다 배열의 항목을 순서대로 꺼내어 변수에 자동으로 대입해준다. 오직 배열의 값을 가져다 사용할 수만 있고 수정할 수는 없다.

```java
public class ForeachDemo {
 
    public static void main(String[] args) {
        String[] members = { "최진혁", "최유빈", "한이람" };
        for (String e : members) {
            System.out.println(e + "이 상담을 받았습니다");
        }
    }
 
}
```
