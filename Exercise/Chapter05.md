# chapter 05. 참조 타입

## 문자열 비교

```java


public class Program {
    public static void main(String[] args) {
        String strVar1 = "신민철";
        String strVar2 = "신민철";

        if(strVar1 == strVar2) {
            System.out.println("strVar1과 strVar2는 참조가 같음");
        } else {
            System.out.println("strVar1과 strVar2는 참조가 다름");
        }

        if(strVar1.equals(strVar2)) {
            System.out.println("strVar1과 strVar2는 문자열이 같음");
        }

        String strVal3 = new String("신민철");
        String strVal4 = new String("신민철");

        if(strVal3 == strVal4) {
            System.out.println("strVar3과 strVar4는 참조가 같음");
        } else {
            System.out.println("strVar3과 strVar4는 참조가 다름");
        }

        if(strVal3.equals(strVal4)) {
            System.out.println("strVar3과 strVar4는 문자열이 같음");
        }
    }
}

```

<br>

## 배열 길이

> `배열변수.length` : 코드에서 배열의 길이를 얻으려면 배열 객체의 length 필드를 읽으면 된다.

```java
public class Program {
    public static void main(String[] args) {
        int scores[] = {83, 90, 87};

        System.out.println(scores[0]);
        System.out.println(scores[1]);
        System.out.println(scores[2]);

        int sum = 0;
        for (int i = 0; i < scores.length; i++) {
            sum += scores[i];
        }

        System.out.println(sum);
        double avg = (double) sum / scores.length;
        System.out.println(avg);

    }
}
```


<br>

## 배열 속의 배열


'''java
public class Program {
    public static void main(String[] args) {

        int[][] mathScores = new int[2][3];
        for (int i = 0; i < mathScores.length; i++) {
            for (int k = 0; k < mathScores.length; k++) {
                System.out.println("mathScores[" + i + "][" + k + "]=" + mathScores[i][k]);
            }
        }
        System.out.println();

        int[][] englishScores = new int[2][1];
        englishScores[0] = new int[2];
        englishScores[1] = new int[3];
        for (int i = 0; i < englishScores.length; i++) {
            for (int k = 0; k < englishScores[i].length; k++) {
                System.out.println("englishScores[" + i + "][" + k + "]=" + englishScores[i][k]);
            }
        }
        System.out.println();

        int[][] javaScores = {{95, 80}, {92, 96, 80}};
        for (int i = 0; i < javaScores.length; i++) {
            for (int k = 0; k < javaScores[i].length; k++) {
                System.out.println("javaScores[" + i + "][" + k + "]=" + javaScores[i][k]);
            }
        }
    }
}
```

<br>

## for문으로 배열 복사

```java
public class Program {
    public static void main(String[] args) {
        int[] oldIntArray = {1, 2, 3};
        int[] newIntArray = new int[5];

        for (int i = 0; i < oldIntArray.length; i++) {
            newIntArray[i] = oldIntArray[i];
        }

        for (int i = 0; i < newIntArray.length; i++) {
            System.out.print(newIntArray[i] + ", ");
        }

    }
}
```

<br>

## System.arraycopy()로 배열 복사

> `System.arraycopy(Object src, int srcPos, Object dest, int destPos, int length);`

  - 원본 배열, 원본 배열에서 복사할 항목의 시작 인덱스, 새 배열, 새 배열에서 붙어넣을 시작 인덱스, 복사할 개수

```java
public class Program {
    public static void main(String[] args) {
        int[] oldIntArray = {1, 2, 3};
        int[] newIntArray = new int[5];

        /*
        // for문을 이용한 배열 복사

        for (int i = 0; i < oldIntArray.length; i++) {
            newIntArray[i] = oldIntArray[i];
        }
        */

        System.arraycopy(oldIntArray, 0, newIntArray, 0, oldIntArray.length);

        for (int i = 0; i < newIntArray.length; i++) {
            System.out.print(newIntArray[i] + ", ");
        }

    }
}
```

<br>

## 향상된 for문

```java
public class Program {
    public static void main(String[] args) {
        int[] scores = {95, 71, 84, 93, 87};

        int sum = 0;
        for (int score : scores) {
            sum += score;
        }
        System.out.println("점수 총합 = " + sum);

        double avg = (double) sum / scores.length;
        System.out.println("점수 평균 = " + avg);
    }
}
```

<br>

## 

```java

```

<br>
