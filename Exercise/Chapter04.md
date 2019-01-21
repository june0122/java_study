# chapter 04. 조건문과 반복문

## 주사위 문제

주사위를 계속 던졌을 때, 숫자 6이 3번 나오면 프로그램을 종료하는 코드 작성

```java
    public static void main(String[] args) {
        int count = 0;
        int num;

        while (count < 3) {
            num = (int) ((Math.random() * 6) + 1);

            System.out.println("num: " + num);

            if (num == 6) {
                count++;
                System.out.println("count: " + count);
            }

        }
        System.out.println("프로그램 종료");
    }
```

<br>

## Ex 04

while문과 Math.random() 메소드를 이용해서 두 개의 주사위를 던졌을 때 나오는 눈을 (눈1, 눈2) 형태로 출력하고, 눈의 합이 5가 아니면 계속 주사위를 던지고, 눈의 합이 5이면 실행을 멈추는 코드를 작성해보세요. 눈의 합이 5가 되는 조합은 (1, 4), (4, 1), (2, 3), (3, 2)입니다.

```java
    public static void main(String[] args) {

        while (true) {

            int num1 = (int) (Math.random() * 6) + 1;
            int num2 = (int) (Math.random() * 6) + 1;

            System.out.println("(" + num1 + ", " + num2 + ")");

            if (num1 + num2 == 5) {
                break;
            }
        }
        System.out.println("주사위의 합이 5가 나왔습니다.");
        System.out.println("프로그램을 종료합니다.");
    }
```

```java
    public static void main(String[] args) {
        int num1 = 0;
        int num2 = 0;

        while (num1 + num2 != 5) {

            num1 = (int) (Math.random() * 6) + 1;
            num2 = (int) (Math.random() * 6) + 1;

            System.out.println("(" + num1 + ", " + num2 + ")");
        }
        System.out.println("주사위의 합이 5가 나왔습니다.");
        System.out.println("프로그램을 종료합니다.");
    }
```

<br>

## Ex 06

for문을 이용해서 실행 결과와 같은 삼각형을 출력하는 코드를 작성해보세요.

```java
// 실행결과

*
**
***
****
*****
```

```java
    public static void main(String[] args) {
        for (int x = 1; x <= 5; x++) {
            for (int y = 1; y <= x; y++) {
                System.out.print("*");
            }
            System.out.println();
        }
    }
```

`System.out.println();`은 자동으로 다음 줄로 넘어간다. 즉, 개행문자(줄바꿈 문자)가 자동으로 붙는다.

`System.out.print();`은 줄바꿈을 하지 않는다.

<br>

## EX 07

whlie문과 Scanner를 이용해서 키보드로부터 입력된 데이터로 예금, 출금, 조회, 종료 기능을 제공하는 코드를 작성해보세요. 이 프로그램을 실행시키면 다음과 같은 실행 결과가 나와야 합니다.

```java
import java.util.Scanner;

public class Program {
    public static void main(String[] args) {
        boolean run = true;

        int balance = 0;

        Scanner scanner = new Scanner(System.in);  // Scanner 객체 생성

        while (run) {
            System.out.println("----------------------------");
            System.out.println("1.예금 | 2.출금 | 3. 잔고 | 4. 종료");
            System.out.println("----------------------------");
            System.out.println("선택> ");

            int number = scanner.nextInt();   // 키보드로 입력한 값 얻음

            int money = 0;

            switch (number) {
                case 1:
                    System.out.println("예금액> ");
                    money = scanner.nextInt();
                    balance += money;
                    break;
                case 2:
                    System.out.println("출금액> ");
                    money = scanner.nextInt();
                    balance -= money;
                    break;
                case 3:
                    System.out.println("잔고> " + balance);
                    break;
                case 4:
                    run = false;
                    break;
            }
        }
        System.out.println("프로그램 종료");
        scanner.close();
    }
}

```



