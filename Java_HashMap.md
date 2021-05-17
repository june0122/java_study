# 자바의 HashMap은 어떻게 작동하는가?

> Java Collections Framework

<br>
<p align = 'center'>
<img width = '900' src = 'https://user-images.githubusercontent.com/39554623/116867336-e8018000-ac47-11eb-9419-136ae42ce606.png'>
</p>

HashMap은 자바 컬렉션 프레임워크 <sup>Java collections framework</sup>에 속한 구현체 클래스다. 자바 컬렉션 프레임워크는 1998년 12월에 발표한 자바 2에서 정식으로 선보였다. Map 인터페이스 자체는 자바 5에서 제네릭이 적용된 것 외에 처음 선보인 이후 변화가 없지만 HashMap 구현체는 성능을 향상시키기 위해 지속적으로 변화해왔다.

이 글에서는 어떤 방식으로 HashMap 구현체의 성능을 향샹시켰는지 소개한다. 구체적으로 다루는 내용은 amortized constant time<sup id = "a1">[1](#f1)</sup> 을 위해 어떻게 해시 충돌 <sup>hash collision</sup> 가능성을 줄이고 있는가에 관한 것이다.

## HashMap과 HashTable

||HashMap|HashTable|
|:--:|:--:|:--:|
|키나 값에 null 저장 가능 여부|O|X|
|여러 스레드 안전 여부|X|O|

이 글에서 말하는 HashTable과 HashMap은 자바의 API 이름이다. HashTable이란 JDK 1.0부터 있던 자바의 API이고, HashMap은 자바 2에서 처음 선보인 자바 컬렉션 프레임워크에 속한 API다. HashTable 또한 Map 인터페이스를 구현하고 있어 HashMap과 HashTable이 제공하는 기능은 같다. 다만 HashMap은 보조 해시 함수 <sup>additional hash function</sup>을 사용하기 때문에 보조 해시 함수를 사용하지 않는 HashTable에 비해 해시 충돌 발생이 덜해 상대적으로 성능상 이점이 있다. 보조 해시 함수가 아니더라도 HashTable은 거의 변화가 없는 반면, HashMap은 지속적으로 개선되고 있다. 그러나 HashTable의 현재 가치는 JRE <sup>Java runtime Environment</sup> 1.0과 JRE 1.1 환경을 대상으로 구현한 자바 애플리케이션이 잘 작동할 수 있도록 하위 호환성을 제공하는 데 있기 때문에 이 둘 사이에 성능과 기능을 비교하는 것은 큰 의미가 없다.

HashMap과 HashTable을 정의하면 '키에 대한 해시 값을 사용해 값을 저장하고 조회하며, 키-값 쌍의 개수에 따라 동적으로 크기가 증가하는 연관 배열 <sup>associative array</sup>'이라고 할 수 있다. 이 연관 배열을 지칭하는 다른 용어가 있는데 대표적으로 Map, Dictionary, Symbol Table 등이 있다.

> HashTable과 HashMap의 선언부

```java
public class Hashtable<K,V>
    extends Dictionary<K,V>
    implements Map<K,V>, Cloneable, java.io.Serializable {

public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable {        
```

연관 배열을 지칭하기 위해 HashTable에서는 Dictionary라는 이름을 사용하고, HashMap에서는 이름 그대로 맵(map)이라는 용어를 사용한다.

맵은 원래 수학 함수에서의 대응 관계를 지칭하는 용어로, 경우에 따라서는 함수 자체를 의미하기도 한다. 즉 HashMap이란 이름에서 알 수 있듯이 HashMap은 키 집합인 정의역과 값 집합인 공역의 대응에 해시 함수를 이용한다.

## 해시 분포와 충돌

동일하지 않은 어떤 객체 X와 Y가 있을 때, 즉 X.equals(Y)가 거짓일때 `X.hashCode() != Y.hashCode()`라면 이때 사용하는 해시 함수는 완전한 해시 함수 <sup>perfect hash functions</sup>라고 한다.

Boolean 같이 서로 구별되는 객체의 종류가 적거나 Integer, Long, Double 같은 Number 객체는 객체가 나타내려는 값 자체를 해시 값으로 사용할 수 있기 때문에 완전한 해시 함수 대상으로 삼을 수 있다. 하지만 String이나 POJO <sup>plain old java object</sup>에 대해 완전한 해시 함수를 제작하는 것은 사실상 불가능하다.

적은 연산만으로 빠르게 작동할 수 있는 완전한 해시 함수가 있다 하더라도, 그것을 HashMap에서 사용할 수 있는 것은 아니다. HashMap은 기본적으로 각 객체의 hashCode() 메서드가 반환하는 값을 사용하는데, 결과 자료형은 int다. 32비트 정수 자료형으로는 완전한 자료 해시 함수를 만들 수 없다. 논리적으로 생성 가능한 객체의 수가 2<sup>32</sup>보다 많을 수 있고, 모든 HashMap 객체에서 O(1)을 보장하기 위해 랜덤 접근이 가능하게 하려면 원소가 2<sup>32</sup>인 배열을 모든 HashMap이 가지고 있어야 하기 때문이다.

따라서 HashMap을 비롯한 많은 해시 함수를 이용하는 연관 배열 구현체에서는 메모리를 절약하기 위해 실제 해시 함수의 표현 정수 범위 `|N|`보다 작은 M개의 원소가 있는 배열만을 사용한다. 따라서 다음과 같이 객체에 대한 해시 코드의 나머지 값을 해시 버킷 인덱스 값으로 사용한다.

> 해시를 사용하는 연관 배열 구현체에서 저장/조회할 해시 버킷을 계산하는 방법

```java
int index = X.hashCode() % M;
```

이 코드와 같은 방식을 사용하면 서로 다른 해시 코드를 가지는 서로 다른 객체가 1/M 확률로 같은 해시 버킷을 사용하게 된다. 이는 해시 충돌을 회피하도록 잘 구현됐느냐에 상관없이 발생할 수 있는 또 다른 종류의 해시 충돌이다. 이렇게 해시 충돌이 발생하더라도 키-값 쌍 데이터를 잘 저장하고 조회할 수 있게 하는 방식에는 대표적으로 두 가지가 있는데, 하나는 open addressing이고, 다른 하나는 seperate chaining이다. 이 둘 외에도 해시 충돌을 해결하기 위한 다양한 자료구조가 있지만 거의 모두 이 둘을 응용한 것이라고 할 수 있다.

> open addressing <sup>좌</sup> & separate Chaining <sup>우</sup> 구조

<br>
<p align = 'center'>
<img height = '300' src = 'https://user-images.githubusercontent.com/39554623/116873995-89420380-ac53-11eb-9d63-2aec0c7ea78a.png'>
<img height = '300' src = 'https://user-images.githubusercontent.com/39554623/116873998-8a733080-ac53-11eb-8c3b-2e5538f35f9e.png'>
</p>


open addressing은 데이터를 삽입하려는 해시 버킷이 이미 사용 중이면 다른 해시 버킷에 해당 데이터를 삽입하는 방식이다. 데이터를 저장, 조회할 해시 버킷을 찾을 때는 선형 검색법 <sup>linear probing</sup>, 2차 검색법 <sup>quadratic probing</sup> <sup id = "a1">[2](#f1)</sup> 등의 방법을 사용한다.

separate Chaining에서 각 배열의 인자는 인덱스가 같은 해시 버킷을 연결한 연결 리스트 <sup>linked list</sup>의 첫 부분 <sup>head</sup>이다.

둘 모두 최대 연산 횟수는 O(M)이다. 하지만 open addressing은 연속된 공간에 데이터를 저장하기 때문에 separate chaining보다 캐시 효율이 높다. 따라서 데이터 개수가 충분히 적다면 open addressing이 separate chaining보다 성능이 더 좋다. 하지만 배열의 크기가 커질수록 <sup>M 값이 커질수록</sup> 캐시 효율이 높다는 open addressing의 장점은 사라진다. 배열의 크기가 커지면 L1, L2 캐시<sup id = "a1">[3](#f1)</sup> 적중률 <sup>cache hit ratio</sup>이 낮아지기 때문이다. (cache miss ratio는 아래의 그래프와 같이 올라간다)

<br>
<p align = 'center'>
<img width = '400' src = 'https://user-images.githubusercontent.com/39554623/116876649-bf818200-ac57-11eb-94e6-26da20740269.png'>
</p>
<br>

자바의 HashMap에서 사용하는 방식은 separate chaining이다. open addressing은 데이터를 삭제할 때 효율적으로 처리하기 어려운데, HashMap에서는 remove() 메서드가 매우 빈번하게 호출될 수 있기 때문이다. 게다가 HashMap에 저장된 키-값 쌍 개수가 일정 개수 이상으로 많아지면 일반적으로 open addressing은 separate chaining보다 느리다. open addressing의 경우 해시 버킷을 채운 밀도가 높아질수록 worst case 발생 빈도가 더 높아지기 때문이다. 반면 separate chaining 방식의 경우 해시 충돌이 잘 발생하지 않도록 '조정'할 수 있다면 worst case 또는 worst case에 가까운 일이 발생하는 것을 줄일 수 있다. <sup>보조 해시 함수 설명과 연관</sup>





## References

- [캐시가 동작하는 아주 구체적인 원리](https://parksb.github.io/article/29.html)
- [What is primary and secondary clustering in hash?](https://stackoverflow.com/questions/27742285/what-is-primary-and-secondary-clustering-in-hash?rq=1)
- [해싱, 해시함수, 해시테이블](https://ratsgo.github.io/data%20structure&algorithm/2017/10/25/hash/)
- 컴퓨터 사이언스 부트캠프 with 파이썬 (amortized constant time 개념)

----------------------------------

## <b id = "f1"><sup> 1 </sup></b>  [분할 상환 상수 시간(amortized constant time)↩](#a1)

알고리즘 성능이 나와 있는 자료를 보면 분할 상환 분석(amortized analysis)을 적용하여 '이 알고리즘의 복잡도는 분할 상환 상수 시간(amortized constant time)을 가진다' 같은 표현을 볼 수 있습니다. 분할 상환 분석이란 특정 상황에서는 좋지 않은 성능을 내지만, 나머지 상황에서는 좋은 성능을 낼 때 모든 연산을 고려해 성능을 분석하는 것을 말합니다. 다시 말해 특정 상황에서의 고비용(high cost)을 일정 기간으로 분산시켜(마치 은행 대출금을 분할하여 상환하듯이) 성능을 평가하는 것입니다.

분할 상환 분석의 대표적인 예에는 동적 배열(dynamic array)이 있습니다. 주어진 메모리 공간이 가득 차면 공간을 두 배로 할당받아 기존 요소를 옮긴 후 더 많은 요소를 추가할 수 있는 동적 배열이 있다고 가정합시다. 배열 크기가 요소를 열 개 담을 수 있고 현재 요소가 네 개만 존재하면 배열이 꽉 차기 전까지 새로운 요소를 추가할 때 성능이 O(1)입니다. 요소를 여섯 개 삽입하여 배열이 꽉 차면 요소 스무 개를 담을 수 있는 공간을 확보한 후 기존 배열의 요소를 옮기게 되는데, 이 경우에만 성능이 O(n)입니다. 이후 새로운 요소를 열 개 삽입하는 동안에는 빅오가 다시 O(1) 즉, 상수 시간이 됩니다.

배열 크기를 늘릴 때는 고비용(기존 배열의 요소 열 개를 옮기면서 발생한 비용)이 발생하지만 이후 요소 열 개를 추가하는 동안에는 비용이 거의 들지 않으므로 이 기간 동안에 분산시키면 성능은 상수 시간과 비슷해집니다. 이를 분할 상환 상수 시간(amortized constant time)이라고 합니다.

## <b id = "f1"><sup> 2 </sup></b>  [Probing ↩](#a1) 

### 선형 검색법 <sup>linear probing</sup>

<br>
<p align = 'center'>
<img height = '300' src = 'https://user-images.githubusercontent.com/39554623/116875105-54cf4700-ac55-11eb-8c22-5affc692d05f.png'>
</p>
<br>



### 2차 검색법 <sup>quadratic probing</sup>

<p align = 'center'>
<img height = '300' src = 'https://user-images.githubusercontent.com/39554623/116875099-5436b080-ac55-11eb-9aba-bb2a2ea57854.png'>
</p>
<br>


## <b id = "f1"><sup> 3 </sup></b>  [L1, L2 캐시 ↩](#a1) 
