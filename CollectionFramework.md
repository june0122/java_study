# 컬렉션 프레임워크(Collection Framework)

## 1. 컬렉션 프레임워크의 이해

### 컬렉션 프레임워크

> 인스턴스의 저장하는 방법을 클래스로 정의하여 제공하는 것.

  - 자료구조 및 알고리즘을 구현해놓은 일종의 라이브러리로 제네릭 기반으로 구현되어 있다.

> 구현하는 인터페이스에 따라 사용방법과 특성이 결정된다.

  - ex) Set은 인스턴스의 저장, 삭제, 참조하는 방법의 하나
  
<br>

![Collection Framework Map](https://www.javatpoint.com/images/java-collection-hierarchy.png)

<br>

## 2. List 인터페이스를 구현하는 컬렉션 클래스들

### List<E> 인터페이스

> List<E> 인터페이스를 구현하는 대표적인 컬렉션 클래스

- `ArrayList<E>` : **배열 기반** 자료구조, 배열을 이용하여 인스턴스 저장
  
  - 단점 : 배열의 길이를 늘릴 수가 없다. → 새로 배열을 만든 후, 데이터를 이동시키고 기존의 배열을 삭제

- `LinkedList<E>` : **리스트(연결) 기반** 자료구조, 리스트를 구성하여 인스턴스 저장

<br>

> List<E> 인터페이스를 구현하는 대표적인 컬렉션 클래스들의 공통 특성

- 인스턴스의 **`★ 저장` 순서 유지** → 나란히 줄을 세우는 것

- 동일 인스턴스의 **중복 저장을 허용**한다.

<br>

### ArrayList<E> 클래스

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

### LinkedList<E> 클래스

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

### ArrayList<E> vs LinkedList<E>

<br>

> 배열과 연결 리스트 형태

![배열과 연결 리스트 형태](http://www.codenuclear.com/wp-content/uploads/2017/11/ArrayList_LinkedList.jpg)

<br>

> 배열과 연결 리스트의 삭제

![배열과 연결 리스트의 삭제](http://www.codenuclear.com/wp-content/uploads/2017/11/ArrayList_LinkedList_Node_delete.jpg)

<br>

- ArrayList<E>의 단점

  - 저장 공간을 늘리는 과정에서 시간이 많이 소요된다.
  
  - 인스턴스의 삭제 과정에서 매우 많은 연산이 필요할 수 있다. 따라서 느릴 수 있다.
  
- ArrayList<E>의 장점
  
  - 저장된 인스턴스의 참조가 빠르다.

- LinkedList<E>의 단점

  - 저장된 인스턴스의 참조 과정이 베열에 비해 복잡하다. 따라서 많이 느릴 수 있다.
  
- LinkedList<E>의 장점

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

> Public interface **Collection<E>** extends **Iterable<E>**

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

- 다음 두 가지 이유로 배열보다 ArrayList<E>가 더 좋다.

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

  - Collection<E>를 구현한 컬렉션 인스턴스를 인자로 전달받는다.
  
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

> public ArrayList(Collection<? extends E> c)   // ArrayList<E> 생성자 중 하나

  - 인자로 전달된 컬렉션 인스턴스로부터 ArrayList<E> 인스턴스 생성

> public LinkedList(Collection<? extends E> c)  // LinkedList<E> 생성자 중 하나

  - 인자로 전달된 인스턴스로부터 LinkedList<E> 인스턴스 생성

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
  
  - ListIterator<E>는 Iterator<E>을 상속한다.
  
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
