# 컬렉션 프레임워크(Collection Framework)

## 1. 컬렉션 프레임워크의 이해

### 컬렉션 프레임워크

> 인스턴스의 저장하는 방법을 클래스로 정의하여 제공하는 것.

  - 자료구조 및 알고리즘을 구현해놓은 일종의 제네릭 기반 라이브러리로 구현되어 있다.

> 구현하는 인터페이스에 따라 사용방법과 특성이 결정된다.

  - ex) Set은 인스턴스의 저장, 삭제, 참조하는 방법의 하나
  
<br>

![Collection Framework Map](https://www.javatpoint.com/images/java-collection-hierarchy.png)

<br>

## 2. List 인터페이스를 구현하는 컬렉션 클래스들

### `List<E>` 인터페이스

> `List<E>` 인터페이스를 구현하는 대표적인 컬렉션 클래스

- `ArrayList<E>` : **배열 기반** 자료구조, 배열을 이용하여 인스턴스 저장
  
  - 단점 : 배열의 길이를 늘릴 수가 없다. → 새로 배열을 만든 후, 데이터를 이동시키고 기존의 배열을 삭제

- `LinkedList<E>` : **리스트(연결) 기반** 자료구조, 리스트를 구성하여 인스턴스 저장

<br>

> `List<E>` 인터페이스를 구현하는 대표적인 컬렉션 클래스들의 공통 특성

- 인스턴스의 **`★ 저장` 순서 유지** → 나란히 줄을 세우는 것

- 동일 인스턴스의 **중복 저장을 허용**한다.

<br>

### `ArrayList<E>` 클래스

```java
public static void main(String[] args){
  List<String> list = new ArrayList<>();  // 컬렉션 인스턴스 생성
  /*
  ArrayList<String>라고 써도 되는데 List<String> 이라고 쓴 이유는?
  
  ArrayList를 List 관점에서 보겠다.
  즉, List 인터페이스가 가지고 있는 기능들만을 활용하겠다는 의미를 지닌다.
  */
  
  // 컬렉션 인스턴스에 문자열 인스턴스 저장
  list.add("Toy");
  list.add("Box");
  list.add("Robot");
  
  // 저장된 문자열 인스턴스의 참조
  for(int i = 0; i < list.size(); i++)
    System.out.print(list.get(i) + '\t');
  System.out.println();
  
  list.remove(0); // 첫 번째 인스턴스 삭제
  
  // 첫 번째 인스턴스 삭제 후 나머지 인스턴스들을 참조
  for(int i = 0; i < list.size(); i++)
    System.out.print(list.get(i) + '\t');
  System.out.println();
}
```
<br>

### `LinkedList<E>` 클래스

- 위의 ArrayList 예제 코드에서 ArrayList를 LinkedList로만 수정해주면 된다.

  - `List<String> list`라고 문장을 구성한 이유도 된다.

```java
public static void main(String[] args){
  List<String> list = new LinkedList<>();  // 유일한 변화

  list.add("Toy");
  list.add("Box");
  list.add("Robot");
  
  for(int i = 0; i < list.size(); i++)
    System.out.print(list.get(i) + '\t');
  System.out.println();
  
  list.remove(0);
  
  for(int i = 0; i < list.size(); i++)
    System.out.print(list.get(i) + '\t');
  System.out.println();
}
```

> (연결) 리스트 기반 자료구조는 열차 칸을 더하고 빼는 형태의 자료구조이다.

- 인스턴스 저장 : 열차 칸을 하나 더한다.

  - 데이터 저장에 있어서는 배열이 연결 리스트보다 빠르다.

- 인스턴스 삭제 : 해당 열차 칸을 삭제한다.

  - 삭제는 연결 리스트가 배열에 비해 장점을 가지는 부분으로 배열은 삭제 후 뒤의 데이터를 앞으로 한 칸씩 이동시켜야하므로 많은 데이터가 존재할 경우 시간이 많이 소요된다. 반면, 연결 리스트는 해당 데이터만 삭제하면 되므로 배열보다 장점을 가진다.

<br>

### `ArrayList<E>` vs `LinkedList<E>`

<br>

> 배열과 연결 리스트 형태

![배열과 연결 리스트 형태](http://www.codenuclear.com/wp-content/uploads/2017/11/ArrayList_LinkedList.jpg)

<br>

> 배열과 연결 리스트의 삭제

![배열과 연결 리스트의 삭제](http://www.codenuclear.com/wp-content/uploads/2017/11/ArrayList_LinkedList_Node_delete.jpg)

<br>

- `ArrayList<E>`의 단점

  - 저장 공간을 늘리는 과정에서 시간이 많이 소요된다.
  
  - 인스턴스의 삭제 과정에서 매우 많은 연산이 필요할 수 있다. 따라서 느릴 수 있다.
  
- `ArrayList<E>`의 장점
  
  - 저장된 인스턴스의 참조가 빠르다.

- `LinkedList<E>`의 단점

  - 저장된 인스턴스의 참조 과정이 베열에 비해 복잡하다. 따라서 많이 느릴 수 있다.
  
- `LinkedList<E>`의 장점

  - 저장 공간을 늘리는 과정이 간단하다.
  
  - 저장된 인스턴스의 삭제 과정이 단순하다.

<br>

### 저장된 인스턴스의 순차적 접근 방법 1 : enhanced for 문의 

```java
public static void main(String[] args) {
  List<String> list = new LinkedList<>();
  
  // 인스턴스 저장
  list.add("Toy");
  list.add("Box");
  list.add("Robot");
  
  // 전체 인스턴스 참조
  for(String s : list)
    System.out.print(S + '\t');
    . . . .
}
```

- for-each문의 대상이 되기 위한 조건 

  - Iterable<T> 인터페이스의 구현
  
   - `for(String s : list)`의 list의 위치에 오는 인스턴스의 클래스가 Iterable 인터페이스를 구현하고 있는가(Iterator 메소드 호출이 가능한가) 확인해야 한다.

<br>

> `Public interface Collection<E> extends Iterable<E>`

- 콜렉션 인터페이스는 Set, List, Queue와 같은 대다수의 컬렉션 인터페이스가 상속하는 부모 인터페이스이다.

- Collection 인터페이스는 Iterable 인터페이스를 상속한다.

  - Set, List, Queue를 구현하는 클래스를 대상으로 Iterable 인터페이스의 메소드 Iterator(반복자)를 호출할 수 있다.

<br>

### 저장된 인스턴스의 순차적 접근 방법 2

> **반복자**를 사용하는 방법을 습득하면 대상이 누구이든 동일한 패턴과 코드를 통해 순차적인 참조가 가능해진다.

```java
public static void main(String[] args){
  List<String> list = new LinkedList<>();
  . . . .
  Iterator<String> itr = list.iterator();   // '반복자' 획득, itr이 지팡이를 참조한다.
  . . . .
  // 반복자를 이용한 순차적 참조
  hile(itr.hasNext()) {  // next 메소드가 반환할 대상이 있다면,
    str = itr.next();     //next 메소드를 호출한다.
    . . . .
  }
}
```

```
// Iterator의 반복자를 참조할 수 있는 참조형

public interface Iterable<T> {
  Iterator<T> iterator();
  . . . .
}
```

<br>

### Iterator 반복자의 세 가지 메소드

- E next()              : 다음 인스턴스의 참조 값을 반환 (다음 인스턴스를 가리킨다)

- boolean hasNext()     : next 메소드 호출 시 참조 값 반호나 가능 여부 확인

- void remove()         : next 메소드 호출을 통해 반환해던 인스턴스 삭제 (현재 가리키고 있는 인스턴스를 삭제한다)

  - remove 메소드는 next 메소드 호출된 반환된 값을 삭제한다.

```java
// 반복자를 이용한 참조 과정 중 인스턴스의 삭제
while(itr.hasNext()) {
  str = itr.next();
  if(str.equals("Box"))
    itr.remove(); // 위에서 next 메소드가 반환한 인스턴스 삭제
}
```

<br>

### 배열보다는 컬렉션 인스턴스가 좋다 : 컬렉션 변환

- 다음 두 가지 이유로 배열보다 `ArrayList<E>`가 더 좋다.

  - 인스턴스의 저장과 삭제가 편하다.

  - 반복자를 쓸 수 있다.

  - size를 걱정하지 않아도 된다.

<br>

- 단, 배열처럼 선언과 동시에 초기화가 불가능하다 그러나 다음 방법을 쓸 수 있다.

> List<String> list = Arrays.`asList`("Toy", "Robot", "Box");

  - 인자로 전달된 인스턴스들을 저장한 컬렉션 인스턴스의 생성 및 반환

  - 이렇게 생선된 리스트 인스턴스는 **Immutable 인스턴스**이다. (변경이 불가능)

<br>

### 배열보다는 컬렉션 인스턴스가 좋다 : 이어서

다음 생성자를 통해서 새로운 ArrayList 인스턴스 생성 가능

> public ArrayList(Collection`<? extends E>` c) {. . .}

  - `Collection<E>`를 구현한 컬렉션 인스턴스를 인자로 전달받는다.
  
  - 그리고 E는 인스턴스 생성 과정에서 결정되므로 무엇이든 될 수 있다.
  
  - 덧붙여서 매개변수 c로 전달된 컬렉션 인스턴스에서는 참조만(꺼내기만) 가능하다. 
  
  <br>
  
  > [<? extends E> 설명](https://player.vimeo.com/video/233948431) : `11분 40초부터 참고`
  
  ArrayList를 구현하고 있는 생성자 내에서 인스턴스가 가지고 있는 인자들을 복사를 하는 것이 목적이므로 반대로 인스턴스에 무언가를 넣는 행위를 하면 안된다.
  
  이런 경우에는 컴파일 오류가 발생하도록 안정적으로 생성자를 정의할 필요가 있으므로 `? extends`를 넣어준 것이다.
  
<br>
  

```java
public static void main(String[] args) {
  List<String> list = Arrays.asList("Toy", "Box", "Robot", "Box");

  // 생성자 public ArrayList(Collection<? extends E> c)를 통한 인스턴스 생성
  list = new ArrayList<>(list);
  ...
}
```

<br>

### 배열 기반 리스트를 연결 기반 리스트로..

> public ArrayList(Collection<? extends E> c)   // `ArrayList<E>` 생성자 중 하나

  - 인자로 전달된 컬렉션 인스턴스로부터 `ArrayList<E>` 인스턴스 생성

> public LinkedList(Collection<? extends E> c)  // `LinkedList<E>` 생성자 중 하나

  - 인자로 전달된 인스턴스로부터 `LinkedList<E>` 인스턴스 생성

<br>

```java
public static void main(String[] args) {
  List<String> list = Arrays.asList("Toy", "Box", "Robot", "Box");
  list = new ArrayList<>(list);
  . . .
  
  // ↓ ArrayList<E> 인스턴스 기반으로 LinkedList<E> 인스턴스 생성
  list = new ArrayList<>(list);
  
  . . .
}
```

<br>

### 기본 자료형 데이터의 저장과 참조

> 오토 박싱과 오토 언박싱 덕분에 컬렉션 인스턴스에 기본 자료형의 값도 저장 가능하다.

```java
public static void main(String[] args){
  LinkedList<Integer> list = new LinkedList<>();
  list.add(10);   // 저장 과정에서 *오토 박싱* 진행
  list.add(20);
  list.add(30);
  
  int n;
  for(Iterator<Integer> itr = list.iterator(); itr.hasNext(); ) {
    n = itr.next();   // 우측에 인스턴스 값이 왔으므로 *오토 언박싱* 진행
                      // next 메소드를 호출하면 integer형 인스턴스의 참조 값이 반환
                      // 대입 연산자의 왼편엔 int형 변수가 왔으므로 오른편엔 int형 값이 와야한다. → 오토 언박싱
    
    System.out.print(n + "\t");
    }
    System.out.println();
}
```

<br>

> while문을 이용한 반복자 생성

```java
. . . .
Iterator<String> itr = list.iterator();   // '반복자' 획득, itr이 지팡이를 참조한다.
. . . .
  while(itr.hasNext()) {  // next 메소드가 반환할 대상이 있다면,
    str = itr.next();     //next 메소드를 호출한다.
```

<br>

> for문을 이용한 반복자 생성

  - for-each문을 구성하면 for-each문이 그대로 실행되는 것이 아닌, 아래와 같이 내부적으론 **반복자 기반의 반복문**으로 바뀐다.

```java
for(Iterator<Integer> itr = list.iterator(); itr.hasNext(); ) {
    n = itr.next();
```

<br>

### 리스트만 갖는 양방향 반복자

- 처음부터 다시 시작하고 싶을 땐 (단방향) 반복자를 다시 생성하면 된다.

- 단방향 반복자로 충분히 해결이 되는데 양방향 반복자를 사용하면 코드가 불필요하게 복잡해진다.

- 리스트 자료구조들만 양방향 반복자를 지원한다는 단점을 가지고 있다.

<br>

`public ListIterator listIterator()  //  List<E> 인터페이스의 메소드`
  
  - `ListIterator<E>`는 `Iterator<E>`을 상속한다.
  
<br>

E next()              : 다음 인스턴스의 참조 값을 반환

boolean hasNext()     : next 메소드 호출 시 참조 값 반환 가능 여부 확인

void remove()         : next 메소드 호출을 통해 반환했던 인스턴스를 삭제

<br>

E previous()          : next 메소드와 기능은 같고 방향만 반대

boolean hasPrevious() : hasNext 메소드와 기능은 같고 방향만 반대

<br>

void add(E e)         : 인스턴스의 추가

void set(E e)         : 인스턴스의 변경

<br>

### 양방향 반복자의 예

```java
public static void main(String[] args) {
        List<String> list = Arrays.asList("Toy", "Box", "Robot", "Box");
        list = new ArrayList<>(list);
       
        ListIterator<String> litr = list.listIterator();  // 양방향 반복자 획득
        
        String str; 
        while(litr.hasNext()) {     // 왼쪽에서 오른쪽으로 이동을 위한 반복문
            str = litr.next();
            System.out.print(str + '\t');

            if(str.equals("Toy"))   // "Toy" 만나면 "Toy2" 저장
                litr.add("Toy2");
        }
        System.out.println();

        while(litr.hasPrevious()) {   // 오른쪽에서 왼쪽으로 이동을 위하 반복문
            str = litr.previous();
            System.out.print(str + '\t');
            if(str.equals("Robot"))     // "Robot" 만나면 "Robot2" 저장
                litr.add("Robot2");
        }
        System.out.println();

        for(Iterator<String> itr = list.iterator(); itr.hasNext(); )
            System.out.print(itr.next() + '\t');
        System.out.println();
    }
```

```java
// 출력 결과
Toy   Box     Robot    Box
Box   Robot   Robot2   Box      Toy2    Toy
Toy   Toy2    Box      Robot2   Robot   Box
```

<br>

## 3. Set 인터페이스를 구현하는 컬렉션 클래스들

### `Set<E>`을 구현하는 클래스의 특성과 `HashSet<E>` 클래스

> `Set<E>` 인터페이스를 구현하는 제네릭 클래스들은 다음 두 가지 특성을 갖는다.
  
  - 저장 순서가 유지되지 않는다.
  
  - 데이터의 중복 저장을 허용하지 않는다.

<br>

```java
public static void main(String[] args) {
  Set<String> set = new HashSet<>();
  set.add("Toy");
  set.add("Box");
  set.add("Robot");
  set.add("Box");
  System.out.println("인스턴스 수: " + set.siz());
  
  // 반복자를 이용한 전체 출력
  for(Iterator<String> itr = set.iterator(); itr.hasNext(); )
    System.out.prnt(itr.next() + '\t');
  System.out.println();
  
  // for-each문을 이용한 전체 출력
  for(String s : set)
    System.out.print(s + '\t');
  System.out.printn();
} 
```

<br>

> 출력 결과

```
인스턴스 수 : 3
Box   Robot   Toy
Box   Robot   Toy
```

***동일(중복) 인스턴스의 기준은? → HashSet과 관련된 중요한 내용***

<br>

### 동일 인스턴스에 대한 기준은?

> public boolean **equals**(Object obj)

  - Object 클래스의 equals 메소드 호출 결과를 근거로 동일 인스턴스를 판단한다.
  
> public int **hashCode**()
  
  - 그런데 그에 앞서 Object 클래스의 hashCode 메소드 호출 결과가 같아야 한다.
  
<br>

> **정리하면**

  - 두 인스턴스가 hasCode 메소드 호출 결과로 반환하는 값이 동일해야 한다.
  
  - 그리고 이어서 두 인스턴스를 대상으로 equals 메소드의 호출 결과 true가 반환되면 동일 인스턴스로 간주한다.
  
<br>

### 해쉬 알고리즘의 이해 ~~(분류 기법 알고리즘)~~

- 분류 대상 : `3`, `5`, `7`, `12`, `25`, `31`

- 적용 해쉬 알고리즘 : `num % 3`

- 분류 결과 :
  
  - 연산결과 0 : `3`, `12`
  
  - 연산결과 1 : `7`, `25`, `31`
  
  - 연산결과 2 : `5`
  
- 해쉬 알고리즘을 이용해 `8`을 탐색하면 나머지값이 2인 집합하고만 비교하면 되므로 속도가 매우 빠르다.

<br>

> 이렇듯 분류를 해놓으면 탐색의 속도가 매우 빨라진다. 즉, 존재 유무 확인이 매우 빠르다.<br>Object 클래스의 hashCode 메소드는 이렇듯 인스턴스들을 분류하는 역할을 한다.

<br>

### hashCode 메소드의 다양한 정의의 예

```java
class Car {
  private String model;
  private String color;
  . . . .
  
  @Override
  public int hashCode() {
    return (model.hashCode() + color.hashCode()) / 2;
    // 모든 인스턴스 변수의 정보를 다 반영하여 해쉬 값을 얻으려는 노력이 깃든 문장.
    // 결과적으로 더 세밀하게 나뉘고, 따라서 그만큼 탐색 속도가 높아진다.
  }
  . . . .
}
```

<br>

### 해쉬 알고리즘 일일이 정의하기 조금 그렇다면...

> public static int hash(Object...values)

  - `java.util.Objects`에 정의된 메소드, 전달된 인자 기반의 해쉬 값 반환
  
```java
@Override
public int hashCode() {
  return Objects.hash(model, color);  // 전달인자 model, color 기반 해쉬 값 반환
}         // 전달된 인자를 모두 반영한 해쉬 값을 반환한다.
```

<br>

* * *

<br>

### `TreeSet<E>` 클래스의 이해와 활용

> `Set<E>` 인터페이스를 구현하는 `TreeSet<E>` 클래스
  
  - 트리(Tree) 자료구조를 기반으로 인스턴스를 저장, 이는 **정렬 상태가 유지되면서 인스턴스가 저장됨**을 의미

<br>

```java
public static void main(String[] args) {
  TreeSet<Integer> tree = new TreeSet<Integer>();
  tree.add(3);
  tree.add(1);
  tree.add(2);
  tree.add(4);  
  System.out.println("인스턴스 수: " + tree.siz());
  
  // 반복자를 이용한 전체 출력
  // 반복자의 인스턴스 참조 순서는 오름차순을 기준으로 한다는 특징이 있다.
  // Comparable 인터페이스의 구현을 통해서 두 인스턴스의 크고 작음에 대한 기준을 제공.
  for(Iterator<Integer> itr = tree.iterator(); itr.hasNext(); )
    System.out.prnt(itr.next().toString() + '\t');
  System.out.println();
  
  // for-each문을 이용한 전체 출력
  for(Integer n : tree)
    System.out.print(n.toString() + '\t');
  System.out.printn();
} 
```

<br>

> 출력 결과

```
인스턴스 수 : 4
1    2    3     4
1    2    3     4
```

<br>

### `TreeSet<E>` 클래스의 오름차순 출력이란?
  
> Ch.20의 Arrays 클래스의 sort 메소드 언급하며 설명한 내용

```
interface Comparable
  
  → int compareTo(Object o)
  
인자로 전달된 o가 작다면 양의 정수 반환

인자로 전달된 o가 크다면 음의 정수 반환

인자로 전달된 o와 같다면 0을 반환
```

<br>

> 제네릭 등장 이후로 추가된 인터페이스

```
interface Comparable<T>

  → int compareTo(T o)
  
인자로 전달된 o가 작다면 양의 정수 반환

인자로 전달된 o가 크다면 음의 정수 반환

인자로 전달된 o와 같다면 0을 반환
```

<br>

### TreeSet 인스턴스에 저장될 것을 고려한 클래스의 예

```java
class Person implements Comparable<Person> {
  private String name;
  private int age;
  . . .
  @Override
  // Comparable<T> 인터페이스의 구현 결과를 근거로 저장이 이뤄지고 또 참조가 진행된다.
  public int compareTo(Person p) {
    return this.age - p.age;
  }  
}
```

***따라서 TreeSet<T>에 저장할 인스턴스(클래스)들은 모두 Comparable<T> 인터페이스를 구현한 클래스의 인터페이스여야 한다.<br>아니면 예외가 발생!***

<br>

### `Comparator<T>` 인터페이스 기반으로 `TreeSet<E>`의 새로운 정렬 기준 제시하기
  
> `public interface Comparator<T>`
  
  - `int compare(T o1, T o2)` 의 구현을 통해 정렬 기준을 결정할 수 있다.
  
    - o1이 o2보다 크면 양의 정수 반환
    
    - o1이 o2보다 작으면 음의 정수 반환
    
    - o1과 o2가 같다면 0 반환
    
  <br>
  
  위 인터페이스를 구현한 클래스의 인스턴스를 `TreeSet<E>`의 다음 생성자를 통해 전달
  
    - public TreeSet(Comparator<? super E> comparator)
    
  <br>
  
### `Comparator<T>` 인터페이스 기반 `TreeSet<E>`의 예 ①
  
  > Person 클래스에 TreeSet을 위한 정렬 기준이 '이미' 마련되어 있으나<br>Comparator 구현 인스턴스를 전달하여 새로운 기준을 제공한다.
  
  ```java
  class Person implements Comparable<Person> {
    String name;
    int age;
    . . .

    @Override
    public int compareTo(Person p) {
        return this.age - p.age;
        // 'age(나이)'를 기준으로 사전편찬 순으로 정렬되어 있음
    }
}

class PersonComparator implements Comparator<Person> {
    public int compare(Person p1, Person p2) {
        return p2.age - p1.age;
        // ① 기존 p1은 p2보다 작다. 그런 p2에서 p1을 빼면 양의 정수를 반환한다.
        // ② 즉 p1이 p2보다 크다는 것이므로 새로운 정렬 기준으로 내림차순이 제시되었다.
    }
}

class ComparatorPerson {
    public static void main(String[] args) {
        TreeSet<Person> tree = new TreeSet<>(new PersonComparator());
        tree.add(new Person("YOON", 37));
        tree.add(new Person("HONG", 53));
        tree.add(new Person("PARK", 22));
	
        for(Person p : tree)
            System.out.println(p);
    }
}
```

<br>

### `Comparator<T>` 인터페이스 기반 `TreeSet<E>`의 예 ②

> String 클래스의 정렬 기준은 사전 편찬순이다. 아래는 이를 길이 순으로 새로운 정렬 기준을 적용한 예시이다.

```java
class StringComparator implements Comparator<String> {
    public int compare(String s1, String s2) {
        return s1.length() - s2.length();
    }
}

class ComparatorString {
    public static void main(String[] args) {
        TreeSet<String> tree = new TreeSet<>(new StringComparator()); // 정렬 기준을 바꾸는 문장
        tree.add("Box");    
        tree.add("Rabbit");
        tree.add("Robot");
	
        for(String s : tree)
            System.out.print(s.toString() + '\t');

        System.out.println();
    }
}
```

<br>

```
// 출력결과

Box   Robot   Rabbit
```

<br>

### 중복된 인스턴스의 삭제

```java
class ConvertCollection {
    public static void main(String[] args) {
        List<String> lst = Arrays.asList("Box", "Toy", "Box", "Toy");   // 리스트는 중복을 허용
        ArrayList<String> list = new ArrayList<>(lst);
        
        for(String s : list)
            System.out.print(s.toString() + '\t');
        System.out.println();

        HashSet<String> set = new HashSet<>(list);    // 중복을 허용하지 않는 Set → 중복이 삭제됨
        list = new ArrayList<>(set);    // 저장물을 다시 list 인스턴스로 옮긴다.

        for(String s : list)
            System.out.print(s.toString() + '\t');
        System.out.println();
    }
}
```

<br>

```
// 출력 결과

Box   Toy   Box   Toy
Box   Toy
```

<br>

## 4. `Queue<E>` 인터페이스를 구현하는 컬렉션 클래스들

### 스택과 큐의 이해

![스택과 큐의 구조](https://cdn-images-1.medium.com/max/1138/1*w2zgPM-PJoRWFWJG2GrSaQ.png)

> Stack

  - LIFO(last-in-first-out) : 먼저 저장된 데이터가 마지막에 빠져나간다.

> Queue

  - FIFO(first-in-first-out) : 먼저 저장된 데이터가 먼저 빠져나간다.

<br>

### 큐 인터페이스

> `Queue<E>` 인터페이스의 메소드들

```
  boolean add(E e)    넣기
  
  E remove()          꺼내기
  
  E element()         확인하기
  
  // 예외 발생
  
  
  boolean offer(E e)  넣기, 넣을 공간이 부족하면 false 반환
  
  E poll()            꺼내기, 꺼낼 대상 없으면 null 반환
  
  E peek()            확인하기, 확인할 대상이 없으면 null 반환
  
  // 예외 발생 
```

<br>

### 큐(Queue)의 구현

> `LinkedList<E>`는 `List<E>`와 동시에 `Queue<E>`를 구현하는 컬렉션 클래스이다.<br>따라서 어떠한 타입의 참조변수로 참조하느냐에 따라 **리스트**로도 **큐**로도 동작한다.

```java
 public static void main(String[] args) {
        Queue<String> que = new LinkedList<>(); // LinkedList<E> 인스턴스 생성
        que.offer("Box");
        que.offer("Toy");
        que.offer("Robot");

        // 무엇이 다음에 나올지 확인 
        System.out.println("next: " + que.peek());

        // 첫 번째, 두 번째 인스턴스 꺼내기
        System.out.println(que.poll());
        System.out.println(que.poll());
        
        // 무엇이 다음에 나올지 확인
        System.out.println("next: " + que.peek());

        // 마지막 인스턴스 꺼내기
        System.out.println(que.poll());
    }
```

<br>

```
// 출력결과

next: Box
Box
Toy
next: Robot
Robot
```

<br>

### 스택(Stack)의 구현

- Deque을 기준으로 스택을 구현하는 것이 자바에서의 원칙

- Deque 인터페이스를 구현한 콜렉션 클래스의 인스턴스를 생성하고<br>해당 인스턴스를 대상으로 offerFirst 메소드와 pollFisrt 메소드를 이용해서 넣고 꺼내기만 하는 것이 **Stack**이다.

> `Deque<E>` 인터페이스의 메소드들

```
 · 앞으로 넣고, 꺼내고, 확인하기
 
   boolean offerFirst(E e)  넣기, 공간 부족하면 false 반환
   E pollFirst()            꺼내기, 꺼낼 대상 없으면 null 반환
   E peekFirst()            확인하기, 확인할 대상 없으면 null 반환


 · 뒤로 넣고, 꺼내고, 확인하기
 
   boolean offerLast(E e)   넣기, 공간 부족하면 false 반환
   E pollLast()             꺼내기, 꺼낼 대상 없으면 null 반환
   E peekLast()             확인하기, 확인할 대상 없으면 null 반환
   
   // 예외 발생 안함
```

```
 · 앞으로 넣고, 꺼내고, 확인하기
 
   void addFisrt(E e)     넣기
   E removeFisrt()        꺼내기
   E getFirst()           확인하기
   

 · 뒤로 넣고, 꺼내고, 확인하기
 
   void addLast(E e)      넣기
   E removeLast()         꺼내기
   E getLast()            확인하기
   
   // 예외 발생
```

<br>

### 스택(Stack)의 예

```java
public static void main(String[] args) {
        Deque<String> deq = new ArrayDeque<>();   // → 배열 기반 Deque
        
        // └  Deaue<String> deq = new LinkedList<>(); 로도 구성 가능  → LinkedList 기반 Deque

        // 앞으로 넣고       
        deq.offerFirst("1.Box");
        deq.offerFirst("2.Toy");
        deq.offerFirst("3.Robot");

        // 앞에서 꺼내기
        System.out.println(deq.pollFirst());
        System.out.println(deq.pollFirst());
        System.out.println(deq.pollFirst());  
    }
```
<br>

> `LinkedList<E>`가 구현하는 인터페이스들
  
  - `Deque<E>`, `List<E>`, `Queue<E>`
  
    - `LinkedList<E>` 인스턴스가 누구를 참조하느냐에 따라 동작하는 방식이 다르다.

<br>

## 5. `Map<K, V>` 인터페이스를 구현하는 컬렉션 클래스들

### Key-Value 방식의 데이터 저장과 `HashMap<K, V>` 클래스

```java
public static void main(String[] args) {
        HashMap<Integer, String> map = new HashMap<>();
        
        // Key-Value 기반 데이터 저장
        map.put(45, "Brown");
        map.put(37, "James");
        map.put(23, "Martin");


        // 데이터 탐색
        System.out.println("23번: " + map.get(23));
        System.out.println("37번: " + map.get(37));
        System.out.println("45번: " + map.get(45));
        System.out.println();

        // 데이터 삭제
        map.remove(37);

        // 데이터 삭제 확인
        System.out.println("37번: " + map.get(37));
    }
```

```
// 출력결과

23번: Martin
37번: James
45번: Brown

37번: null
```

<br>

### `HashMap<K, V>`의 순차적 접근 방법

- HashMap<K, V> 클래스는 Iterable<T> 인터페이스를 구현하지 않으니<br>for-each문을 통해서, 혹은 **반복자**를 얻어서 순차적 접근을 진행할 수 없다.

- 대신 다음 메소드 호출을 통해서 Key를 따로 모아 놓은 컬렉션 인스턴스를 얻을 수 있다.<br>그리고 이때 반환된 컬렉션 인스턴스를 대상으로 반복자를 얻을 수 있다.

  - public `Set<K>` keySet()

<br>

> Map은 반복자가 어울리지 않는 자료구조?

```
Map이라는 자료 구조는 그 자체만으로 반복자를 제공하는 것이 어울리지 않는다.

Map의 순차적 접근을 원할때 Key값을 List와 Set 중, 어느 컬렉션 인스턴스에 담는 것이 좋을까?

Key는 중복되지 않는 집합을 성격을 가지고 있다. 고로 중복을 허용하는 List보다는 중복을 허용하지 않는 Set이 더 어울린다.

Set을 구현한 Collection 인스턴스는 Collection이 Iterable 인터페이스를 상속하므로 반복자를 얻어올 수 있다.
```

<br>

### HashMap<K, V>의 순차적 접근의 예

```java
public static void main(String[] args) {
        HashMap<Integer, String> map = new HashMap<>();
        
        // Key-Value
        map.put(45, "Brown");
        map.put(37, "James");
        map.put(23, "Martin");

        // Key만 담고 있는 컬렉션 인스턴스 생성
        Set<Integer> ks = map.keySet();

        // 전체 Key 출력 (for-each문 기반)
        for(Integer n : ks)
            System.out.print(n.toString() + '\t');
        System.out.println();

        // 전체 Value 출력 (for-each문 기반)
        for(Integer n : ks)
            System.out.print(map.get(n).toString() + '\t');
        System.out.println();

        // 전체 Value 출력 (반복자 기반)
        for(Iterator<Integer> itr = ks.iterator(); itr.hasNext(); )
            System.out.print(map.get(itr.next()) + '\t');
        System.out.println();
    }
```

```
// 출력결과

37	23	45
James	Martin	Brown
James	Martin	Brown
```

<br>

### TreeMap<K, V>의 순차적 접근의 예

> Tree 자료구조의 특성상 반복자가 **정렬된 순서대로** key들에 접근을 하고 있다.<br>이렇듯 반복자의 접근 순서는 컬렉션 인스턴스에 따라 달라질 수 있다.

```java
public static void main(String[] args) {
        TreeMap<Integer, String> map = new TreeMap<>();
        
        // Key-Value
        map.put(45, "Brown");
        map.put(37, "James");
        map.put(23, "Martin");

        // Key만 담고 있는 컬렉션 인스턴스 
        Set<Integer> ks = map.keySet();

        // 전체 Key 출력 (for-each문 기반)
        for(Integer n : ks)
            System.out.print(n.toString() + '\t');
        System.out.println();

        // 전체 Value 출력 (for-each문 기반)
        for(Integer n : ks)
            System.out.print(map.get(n).toString() + '\t');
        System.out.println();

        // 전체 Value 출력 (for-each문 기반)
        for(Iterator<Integer> itr = ks.iterator(); itr.hasNext(); )
            System.out.print(map.get(itr.next()) + '\t');
        System.out.println();
    }
```

```
// 출력결과

23	37	45
Martin	James	Brown
Martin	James	Brown
```
