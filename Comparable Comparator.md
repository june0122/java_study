# Comparable & Comparator

|이름|정의|
|:--:|:--:|
|Comparable|클래스의 기본 정렬 기준을 설정하는 인터페이스|
|Comparator|기본 정렬 기준과는 다르게 정렬하고 싶을 때 이용하는 클래스|

## Comparable과 Comparator란?

### 1. Comparable 인터페이스

- 정의 : 정렬 수행 시 **기본적으로 적용되는 정렬 기준이 되는 메서드를 정의하는 인터페이스**
- 사용법 : 정렬할 객체에 Comparable 인터페이스를 implements 한 후, `compareTo()` 메서드를 override하여 구현한다.
  - `compareTo()` 메서드 작성법
    - 현재 객체 < 파라미터로 넘어온 객체: 음수 리턴
    - 현재 객체 == 파라미터로 넘어온 객체: 0 리턴
    - 현재 객체 > 파라미터로 넘어온 객체: 양수 리턴
    - 음수 또는 0이면 객체의 자리가 그대로 유지되며, 양수인 경우에는 두 객체의 자리가 바뀐다.
- 패키지 : `java.lang.Comparable`
- 자바에서 제공되는 정렬이 가능한 클래스들은 모두 Comparable 인터페이스를 구현하고 있으며, 정렬 시에 이에 맞게 정렬이 수행된다.
  - 아래와 같이 Integer, String과 같은 클래스의 문서에 Comparable 인터페이스가 구현된 것을 확인할 수 있다.
    - Integer, Double 클래스 : nondecreasing order(비내림차순)<sup id = "a1">[1](#f1)</sup>
    - String 클래스 : 사전순 정렬

<p align = 'center'>
<img width = '600' src = 'https://user-images.githubusercontent.com/39554623/115342308-05b0fd00-a1e5-11eb-8252-62f4ec68312f.png'>
</p>
<p align = 'center'>
<img width = '600' src = 'https://user-images.githubusercontent.com/39554623/115342437-3bee7c80-a1e5-11eb-845a-419fed6c4e12.png'>
</p>

### 2. Comparator 클래스

- 정의 : 정렬 가능한 클래스 <sup>Comparable이 구현된 클래스</sup>들의 **기본 정렬 기준과는 다른 방식으로 정렬하고 싶을 때 사용하는 클래스**
- 사용법 : Comparator 클래스를 생성하여, 내부에 `compare()` 메서드를 원하는 정렬 기준대로 구현하여 사용할 수 있다.
- 패키지 : `java.util.Comparator`
- 주로 익명 클래스 `new Comparator() { ... }`로 사용되며, 기본적으로 오름차순이 정렬 기준인 것을 내림차순으로 정렬하는 등의 용도로 사용된다.

## 사용 방법

### 1. Comparable

```java
class Student{
    String name; //이름
    int id; //학번
    double score; //학점
    public Student(String name, int id, double score){
        this.name = name;
        this.id = id;
        this.score = score;
    }
    public String toString(){ //출력용 toString 오버라이드
        return "이름: "+name+", 학번: "+id+", 학점: "+score;
    }
}

class Main{
    public static void main(String[] args) {
        Student[] student = new Student[5];
        //순서대로 "이름", 학번, 학점
        student[0] = new Student("Dave", 20170001, 4.2);
        student[1] = new Student("Amie", 20150001, 4.5);
        student[2] = new Student("Emma", 20110001, 3.5);
        student[3] = new Student("Brad", 20130001, 2.8);
        student[4] = new Student("Cara", 20140001, 4.2);
    }
}
```

Student 클래스에 기본 정렬 기준이 없으므로 우선 java.lang.Comparable 패키지를 import 한 다음 Comparable을 implements 해준다.

이 때, 주의해야 할 것은 Comparable의 제너릭스인데, 우리는 Student 클래스 끼리 비교하는 것이므로 제너릭스 타입도 Student가 된다.

Comparable을 implements하고 나면 구현하지 않은 메서드를 구현하라고 나오는데, 이 메서드는 compareTo메서드로 경고 메세지를 클릭하거나 직접 오버라이딩 해 주면 된다.

```java
import java.lang.Comparable; // package를 import 해준다.

class Student implements Comparable<Student>{
    String name; //이름
    int id; //학번
    double score; //학점
    public Student(String name, int id, double score){
        this.name = name;
        this.id = id;
        this.score = score;
    }
    public String toString(){ //출력용 toString 오버라이드
        return "이름: "+name+", 학번: "+id+", 학점: "+score;
    }

    @Override
    public int compareTo(Student o) { // compareTo 오버라이딩
        return 0;
    }
}
```

이제 `compareTo` 메서드 내용을 우리가 원하는대로 구현해야 한다. 우리가 원하는 정렬 기준은 '학번 오름차순'이므로 아래와 같이 구현해주면 된다.

```java
@Override
public int compareTo(Student anotherStudent) { // compareTo 오버라이딩
    return Integer.compare(id, anotherStudent.id);
}
```
`Integer.compare`은 단순히 첫번째 매개변수와 두번째 매개변수가 오름차순으로 유지될 수 있도록 값을 비교해 주는 메서드이다. Integer클래스에서 함수 원형을 찾아보면 아래와 같다.

<br>
<p align = 'center'>
<img width = '500' src = 'https://user-images.githubusercontent.com/39554623/115348313-70fecd00-a1ed-11eb-970e-c6ccd173b392.png'>
</p>
<br>

이 `int compare` 메서드를 간단히 설명하자면, **정렬이 진행될 때 자리바꿈(=정렬) 여부를 결정하는 값을 넘겨주는 역할**을 한다.
- 만약 return값이 **0이나 음수이면 자리바꿈을 하지 않고**, **양수이면 자리바꿈을 수행**한다.
- 만약 오름차순이 아니라 내림차순으로 정렬하고 싶다면 매개변수의 순서를 바꿔주면 된다.

Integer.compare를 사용하지 않고 아래와 같이 직접 구현해도 상관 없다.

```java
@Override
public int compareTo(Student anotherStudent) {
    return (id < anotherStudent.id) ? -1 : ((id == anotherStudent.id) ? 0 : 1);
}
```

여기까지 했으면 기본 정렬 기준이 완성된 것이다. 이를 바탕으로 main() 메서드에서 자바에서 기본적으로 제공하는 퀵소트 메서드인 Arrays.sort()를 이용하여 정렬을 수행해보면 학번 오름차순으로 잘 정렬된 것을 확인할 수 있다.

```java
import java.util.Arrays; // 퀵 소트 사용을 위해 import

// Student Class { ... }

class Main{
    public static void main(String[] args) {
        Student[] student = new Student[5];
        //순서대로 "이름", 학번, 학점
        student[0] = new Student("Dave", 20170001, 4.2);
        student[1] = new Student("Amie", 20150001, 4.5);
        student[2] = new Student("Emma", 20110001, 3.5);
        student[3] = new Student("Brad", 20130001, 2.8);
        student[4] = new Student("Cara", 20140001, 4.2);

        Arrays.sort(student); // 퀵 소트
        for (Student value : student) { // toString() 에 정의된 형식으로 출력
            System.out.println(value);
        }
    }
}
```

여담으로 만일 Comparable이 구현되지 않은 상태에서 Arrays.sort()를 이용하여 정렬을 수행하려 하면 아래와 같은 에러가 발생하게 된다.
- 이유는 바로 **Arrays.sort()등 정렬 메서드들은 정렬 시 자동으로 Comparable에 구현되어 있는 compareTo() 메서드를 호출해서 사용하기 때문이다.**

<p align = 'center'>
<img width = '800' src = 'https://user-images.githubusercontent.com/39554623/115350738-6e51a700-a1f0-11eb-9ef2-640e33ba9338.png'>
</p>
<br>

### 2. Comparator

Comparable을 이용해 학번 오름차순대로 정렬을 하였다. 이제 이 5명의 학생들 중에 성적우수자 2명을 선정하여 장학금을 지급하려 한다. 이 때, 성적이 같은 학생이 여러 명이라면 학번이 빠른 순서대로 정하려고 한다. 우리는 성적이 높은 학생 2명을 편하게 찾기 위해서 학생들을 **"성적이 높은 순으로 정렬한 뒤, 앞에서 2명을 선택"**하려고 한다. 어떻게 할 수 있을까?

성적이 높은 순서대로 정렬하는 것이 우선이다. 그런데 Student클래스의 기본 정렬 기준은 "학번 오름차순"이므로 이 **기본 정렬 기준과 다른 새로운 정렬 기준**을 세워야 한다. 이 때 이용되는 것이 바로 **Comparator**이다. **Comparator는 `java.util.Comparator`패키지**에 있고, **주로 익명 클래스로 사용**되며, 이를 **`Arrays.sort()`내부에 정렬 기준으로 구현**하면 된다.

> Array.sort() 내부에 Comparator를 구현할 수 있도록 정의되어 있다.
<p align = 'center'>
<img width = '500' src = 'https://user-images.githubusercontent.com/39554623/115351808-98f02f80-a1f1-11eb-89b2-f209b47a5112.png'>
</p>
<br>

```java
Arrays.sort(student, new Comparator<Student>() {
    @Override
    public int compare(Student s1, Student s2) {
        return 0;
    }
});
```

이제 정렬 조건을 다시한번 확인해보면 아래와 같다.
1) 학점이 높은 순서대로(학점 내림차순)
2) 학점이 같다면 학번이 빠른 순서대로(학번 오름차순)

이를 바탕으로 학점 비교 식을 Comparator 내부에 구현하면 아래와 같다. 위의 Integer.compare 설명에서 잠깐 언급했듯이 **내림차순 정렬을 하려면 매개변수의 순서를 바꿔**주면 된다.

```java
Arrays.sort(student, new Comparator<Student>() {
    @Override
    public int compare(Student s1, Student s2) {
        double s1Score = s1.score;
        double s2Score = s2.score;
        return Double.compare(s2Score, s1Score); // 학점 내림차순 정렬을 위해 매개변수 순서 변경
    }
});
```
근데 이렇게만 끝나면 안된다. 왜냐하면 4.5점 학생이 1명, 4.2점 학생이 2명이므로, 학번을 통해 2순위와 3순위를 구별해야 하기 때문이다.

```java
student[0] = new Student("Dave", 20170001, 4.2);  // 4.2점 1번
student[1] = new Student("Amie", 20150001, 4.5);
student[2] = new Student("Emma", 20110001, 3.5);
student[3] = new Student("Brad", 20130001, 2.8);
student[4] = new Student("Cara", 20140001, 4.2);  // 4.2번 2번
```

따라서 2번째 조건인 '학점이 같으면 학번 오름차순' 정렬 조건을 Comparator 내부에 구현해주면 아래와 같다.

```java
Arrays.sort(student, new Comparator<Student>() {
    @Override
    public int compare(Student s1, Student s2) {
        double s1Score = s1.score;
        double s2Score = s2.score;
        
        if (s1.score == s2.score) { // 학점이 같다면
            return Double.compare(s1.id, s1.id); // 학번 오름차순으로 정렬 
        }
        
        return Double.compare(s2Score, s1Score); // 학점 내림차순 정렬을 위해 매개변수 순서 변경
    }
});
```

여기까지 완성된 Comparator를 바탕으로 정렬을 수행한 뒤 결과를 출력하면 아래와 같이 정상 출력되는 것을 볼 수 있다.

```java
import java.lang.Comparable; // package를 import 해준다.
import java.util.Arrays;
import java.util.Comparator;

class Student implements Comparable<Student>{
    String name; //이름
    int id; //학번
    double score; //학점
    public Student(String name, int id, double score){
        this.name = name;
        this.id = id;
        this.score = score;
    }
    public String toString(){ //출력용 toString 오버라이드
        return "이름: "+name+", 학번: "+id+", 학점: "+score;
    }

    @Override
    public int compareTo(Student anotherStudent) { // compareTo 오버라이딩
        return Integer.compare(id, anotherStudent.id);
    }
}

class Main{
    public static void main(String[] args) {
        Student[] student = new Student[5];
        //순서대로 "이름", 학번, 학점
        student[0] = new Student("Dave", 20170001, 4.2);
        student[1] = new Student("Amie", 20150001, 4.5);
        student[2] = new Student("Emma", 20110001, 3.5);
        student[3] = new Student("Brad", 20130001, 2.8);
        student[4] = new Student("Cara", 20140001, 4.2);

        Arrays.sort(student, new Comparator<Student>() {
            @Override
            public int compare(Student s1, Student s2) {
                double s1Score = s1.score;
                double s2Score = s2.score;
                if (s1.score == s2.score) { // 학점이 같다면
                    return Double.compare(s1.id, s2.id); // 학번 오름차순으로 정렬
                }
                return Double.compare(s2Score, s1Score); // 학점 내림차순 정렬을 위해 매개변수 순서 변경
            }
        });

        for (Student value : student) {
            System.out.println(value);
        }
    }
}
```

> 새로운 정렬기준: 학점 내림차순 & 같으면 학번 오름차순

<br>
<p align = 'center'>
<img width = '400' src = 'https://user-images.githubusercontent.com/39554623/115353854-f4bbb800-a1f3-11eb-9b49-1633332a8125.png'>
</p>
<br>

여기까지 해서 Comparable과 Comparator에 대해 알아보았다. 상당히 자주 사용되는 개념들이므로 스스로 다양하게 사용해보면서 익히는 것이 중요하다.

※ 연습

Comparator를 이용해 학생들을 이름의 오름차순(=사전 순)으로 정렬하는 코드

- String클래스도 Comparable이 구현되어 있다. 즉 내부에 compareTo가 구현되어 있다.

```java
import java.lang.Comparable;
import java.util.Arrays;
import java.util.Comparator; //Comparator 사용하기 위한 import

class Student implements Comparable<Student> {
    String name; //이름
    int id; //학번
    double score; //학점

    public Student(String name, int id, double score) {
        this.name = name;
        this.id = id;
        this.score = score;
    }

    public String toString() {
        return "이름: " + name + ", 학번: " + id + ", 학점: " + score;
    }

    /* 기본 정렬 기준: 학번 오름차순 */
    public int compareTo(Student anotherStudent) {
        return Integer.compare(id, anotherStudent.id);
    }
}

class Main {
    public static void main(String[] args) {
        Student[] student = new Student[5];
        //순서대로 "이름", 학번, 학점
        student[0] = new Student("Dave", 20120001, 4.2);
        student[1] = new Student("Amie", 20150001, 4.5);
        student[2] = new Student("Emma", 20110001, 3.5);
        student[3] = new Student("Brad", 20130001, 2.8);
        student[4] = new Student("Cara", 20140001, 4.2);

        /*
         * 학생들의 이름은 String이며, String클래스 역시 Comparable이
         * 사전순으로 정렬되게 구현되어 있으므로,
         * (이름).compareTo(다른학생이름); 의 형식으로 Comparator을
         * 사용할 수 있다.
         */
        Arrays.sort(student, new Comparator<Student>() {
            @Override
            public int compare(Student s1, Student s2) {
                return s1.name.compareTo(s1.name);
            }
        });

        for (int i = 0; i < 5; i++)
            System.out.println(student[i]);
    }
}
```

**※ Arrays.sort()와 Collections.sort()의 차이**

- `Arrays.sort()`
  - `byte[], char[], double[], int[], Object[], T[]` 와 같은 배열 정렬
  - Primitive Array에서는 Dual Pivot Quick Sort <sup>Quick Sort + Insertion Sort</sup> 사용
  - Object Array에서는 TimSort <sup>Merge Sort + Insertion Sort</sup> 사용
- `Collections.sort()`
  - `ArrayList, LinkedList, Vector` 와 같은 List Collection 정렬
  - 내부적으로 `Arrays.sort()`를 사용한다.

## References

- https://blog.naver.com/occidere/220918234464
- https://gmlwjd9405.github.io/2018/09/06/java-comparable-and-comparator.html

---
<b id = "f1">비내림차순<sup> 1 </sup></b>  [ ↩](#a1)

- 각각의 원소가 바로 앞에 있는 원소보다 크거나 같을 경우를 말한다. 만약 그러한 수열이 여러개라면 사전순으로 앞서는 것을 출력한다
- "내림차순이 아니다"이지 "오름차순"은 아니다.

