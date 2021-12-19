# [Java] Collection - Map 인터페이스

<p align = 'center'>
<img width = '600' src = 'https://user-images.githubusercontent.com/39554623/146663121-9c808d21-873d-4f50-8002-7e9388b420b9.png'>
</p>

## Map 인터페이스를 구현한 주요 클래스들

1. **HashMap**
2. **LinkedHashMap**
3. **TreeMap**
4. **Hashtable**

||HashMap|LinkedHashMap|TreeMap|Hashtable|ConcurrentHashMap|
|:--:|:--:|:--:|:--:|:--:|:--:|
|Iteration Order|랜덤|삽입 순서대로|키를 기준으로 정렬|랜덤|랜덤|
|Iterator type|Fail-Fast iterator|Fail-Fast iterator|Fail-Fast iterator|Enumerator in Hashtable is not fail-fast|Fail-Safe iterator|
|get / put / remove / containKey|<i>O(1)</i>|<i>O(1)</i>|<i>O(logN)</i>|<i>O(1)</i>|<i>O(1)</i>|
|Implementation|buckets|double-linked buckets|Red-Black Tree|buckets|buckets|
|Synchronization|X|X|X|O|O|

### HashMap

```java
HashMap<String, String> map = new HashMap<String, String>();

map.put("바나나", "과일");
map.put("당근", "채소");
map.put("양파", "채소");
map.put("사과", "과일");
map.put("삼겹살", "육류");

for (Map.Entry<String, String> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " - " + entry.getValue());
}
```

```console
양파 - 채소
당근 - 채소
사과 - 과일
바나나 - 과일
삼겹살 - 육류
```

### LinkedHashMap

```java
LinkedHashMap<String, String> map = new LinkedHashMap<String, String>();

map.put("바나나", "과일");
map.put("당근", "채소");
map.put("양파", "채소");
map.put("사과", "과일");
map.put("삼겹살", "육류");

for (Map.Entry<String, String> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " - " + entry.getValue());
}
```

```console
바나나 - 과일
당근 - 채소
양파 - 채소
사과 - 과일
삼겹살 - 육류
```

### TreeMap

```java
TreeMap<String, String> map = new TreeMap<String, String>();

map.put("바나나", "과일");
map.put("당근", "채소");
map.put("양파", "채소");
map.put("사과", "과일");
map.put("삼겹살", "육류");

for (Map.Entry<String, String> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " - " + entry.getValue());
}
```

```console
당근 - 채소
바나나 - 과일
사과 - 과일
삼겹살 - 육류
양파 - 채소
```

### HashMap과 Hashtable의 차이

Hashtable 클래스는 Map 인터페이스를 구현하기는 했지만 일반적인 Map 인터페이스를 구현한 클래스들과는 다르다. 이 두 종류의 다른 점을 간단하게 정리하자면 다음과 같다.

- Map은 컬렉션 뷰<small>(Collection View)</small>를 사용하지만, Hashtable은 Enumeration 객체를 통해서 데이터를 처리한다.
- Map은 키, 값, 키-값 쌍으로 데이터를 순환하여 처리할 수 있지만, HashTable은 이 중에서 키-값 쌍으로 데이터를 순환하여 처리할 수 없다.
- Map은 이터레이션을 처리하는 도중에 데이터를 삭제하는 안전한 방법을 제공하지만, HashTable은 그러한 기능을 제공하지 않는다.

#### Map의 Collection view는 뭘까?

-----

`hashCode()` 메서드는 기본적으로 객체의 메모리 주소를 16진수로 리턴한다.

두 객체를 equals() 메소드를 사용하여 비교한 결과 false를 리턴했다고 해서, hashCode() 메소드를 호출한 int 값이 무조건 달라야 할 필요는 없다. 하지만, 이 경우에 서로 다른 int 값을 제공하면 hashtable의 성능을 향상시키는데 도움이 된다.


## References

- https://stackoverflow.com/questions/2889777
- https://docs.oracle.com/javase/tutorial/collections/interfaces/map.html