# 아이템 14 - Comparable을 구현할지 고려하라

Comparable 인터페이스를 이용해야할 때와 어떻게 사용해야하는지 알아보자.

## Comparable
`Comparable` 인터페이스는 오직 단 하나의 메서드 `compareTo`를 가진다. 이전 장에서 살펴본 `Cloneable`은 clone 메서드를 Object에 가지지만, 이와 달리 `Comparable`의 `compareTo` 메서드는 Object에 속하지 않는다. compareTo 메서드는 단순 동치성 비교에 더해 순서까지 비교할 수 있으며 제네릭하다. 그러므로 Comparable을 구현했다는 것은 그 클래스의 인스턴스들에는 자연적인 순서(nature order)가 있음을 뜻한다.

```java
public interface Comparable<T> {
    public int compareTo(T o);
}
```

### Comparable.compareTo의 일반 규약
-   이 객체와 주어진 객체의 순서를 비교한다.
-   이 객체가 주어진 객체보다 작으면 음의 정수를, 같이면 0을, 크면 양의 정수를 반환한다.
-   이 객체와 비교할 수 없는 타입의 객체가 주어지면 ClassCaseException을 던진다.
-   Comparable을 구현한 클래스 x, y에 대해 `sgn(x.compareTo(y)) == -sgn(y.compareTo(x))`를 만족해야한다.
-   `(x.compareTo(y) > 0) && (y.compareTo(z) > 0)` 이면 `x.compareTo(z) > 0`이다.
-   `x.compareTo(y) == 0`이면 `sgn(x.compareTo(z)) == sgn(x.compareTo(y))` 이다.
-   권고사항은 아니지만 `x.compareTo(y) == 0 == x.equals(y)`는 지키는 게 좋다
    -   실제로 Collections, Set 또는 Map에서 동치성 비교에 equals대신 compareTo를 사용한다.


### compareTo 구현 요령
-   Generic이므로 입력 인수의 타입은 컴파일 타입에 정해진다. 입력 인수의 타입이 잘못되었다면 NullPointerException을 던지자.
-   객체 참조 필드를 비교하려면 compareTo 메서드를 재귀적으로 호출하자.
-   primitive type에 대하여 compareTo를 구현하지 말고 박싱 된 기본 타입 클래스들에 대하여 compare를 이용하자 (Float.compare, Double.compare)
-   비교 대상의 핵심 필드가 여러 개 라면, 가장 핵심적인 필드부터 비교해나가자.
-   자바 8 Comparator 인터페이스의 비 교자 생성 메서드를 통해서도 구현 가능하다.
    -   comparingInt, thenComparing
-   알파벳, 순서, 연대 같이 순서가 명확한 값 클래스를 작성한다면 반드시 Comparable 인터페이스를 구현하자.
-   다음과 같은 compare 구현은 피해야 한다. 정수 오버플로우가 일어날 수 있기 때문에 Integer.compare를 이용하자
    ```java
    static Comparator<Object> hashCodeOrder = new Comparator<>() {
        public int compare(Object o1, Object o2) {
            return o1.hashCode() - o2.hashCode();
        }
    }
    ```

### Comparable 활용 상황

-   Arrays.sort() 메서드는 Comparable을 구현한 객체의 배열에 대해 정렬을 지원
-   String은 Comparable을 구현하였기에 정렬 시 자동으로 알파벳순으로 정렬
-   비교를 활용하는 클래스로 정렬 Collection으로는 TreeSet, TreeMap, 검색과 정렬 알고리즘을 활용하는 유틸리티 클래스는 Collections, Arrays가 존재
---

### 정리
> 순서를 고려해야 하는 값 클래스를 작성한다면 꼭 Comparable 인터페이스를 구현하자. 이는 컬렉션에서 제공하는 정렬, 검색 및 비교 기능을 손쉽게 이용할 수 있도록 한다. compareTo 메서드에서 필드의 값을 비교할 때 '<' 혹은 '>' 연산자는 쓰지 않도록 하자. 그 대신 박싱 된 기본 타입 클래스가 제공하는 정적 compare 메서드나 Comparator 인터페이스가 제공하는 비교자 생성 메서드를 사용하도록 하자.