# 아이템 10 - equals는 일반 규약을 지켜 재정의하라

equals 메서드는 재정의하기에 쉬워 보이지만 곳곳에 함정이 도사리고 있다. 우선적으로 equals 메서드를 재정의하지 않아도 될 경우를 살펴보고, 재정의가 필요하다면 어떤 규약을 지켜서 재정의해야하는지 알아보자.

## equals 메서드를 재정의 할 필요가 없을 때
다음 열거한 상황 중 하나에 해당된다면 equals 메서드를 재정의하지 않는것이 좋다.

#### 각 인스턴스가 본질적으로 고유하다.

값을 표현하는것이 나닌 동작하는 개채를 표현하는 클래스가 여기에 해당된다. 예시로 Thread와 같은 클래스가 여기 해당된다. Object의 equals 메서드는 이미 이러한 클래스에 딱 맞게 구현되어있다.

#### 인스턴스의 `논리적 동치성(logical equality)`을 검사할 일이 없다.

인스턴스의 논리적인 동치성을 검사해야할 일이 있다면 그에 맞는 equals를 꼭 재정의해야한다. 하지만 이런 방식을 원하지 않거나 애초에 필요하지 않다면 기본 equals 만으로 해결된다.

#### 상위 클래스에서 재정의한 equals가 하위 클래스에도 들어맞는다.

이러한 경우의 예시로는 Set의 구현체는 AbstractSet으로 부터, Map의 구현체들은 AbstractMap으로 부터 equals를 그대로 상속받아 쓴다.

#### 클래스가 private이나 package-private이고 equals 메서드를 호출할 일이 없다.

이러한 경우에도 실수로 호출될 수 있는 equals를 막고 싶다면 아래와 같이 구현하여 문제를 막을 수 있다.
```java
@Override
public boolean equals(Object o) {
    throw new AssertionError(); // 호출 금지!
}
```

---

## equals 메서드가 필요할 때

* 객체 식별성: 두 객체가 물리적으로 같은가
* 논리적 동치성: 두 객체가 논리적으로 같은가
* 인스턴스 통제 클래스: 같은 인스턴스를 반환하여 인스턴스의 생성을 컨트롤 할 수 있는 정적 팩토리 메서드를 의미

equals를 재정의해야 할 때는 객체의 논리적 동치성을 확인해야 하는데, 상위 클래스의 equals가 논리적 동치성을 비교하도록 재정의되지 않았을 때다. 주로 **값 클래스**들이 이에 해당된다. 값 클래스란 Integer와 String처럼 값을 표현하는 클래스를 말한다. 값 클래스중 예외로는, 인스턴스가 둘 이상 만들어지지 않음을 보장하는 인스턴스 통제 클래스가 존재한다. 예시로는 `Enum`을 들수있다. 이런 클래스는 논리적으로 같은 인스턴스가 2개 이상 만들어지지 않으니 논리적 동치성과 객체 식별성이 사실상 똑같은 의미가 된다.

---

## equals 일반 규약 - 동치관계 (equivalence relation)
euqals 메서드는 동치관계를 구현하며, 다음을 만족한다.
* 반사성(reflexivity) : null 아닌 모든 참조 값 x에 대해, x.equals(x)는 true다.
* 대칭성(symmetry) : null이 아닌 모든 참조 값 x, y에 대해 x.equals(y)가 true면 y.equals(x)도 true다.
* 추이성(transitivity) : null이 아닌 모든 참조 값 x, y, z에 대해 x.equals(y)가 true아고 y.equals(z)도 true면 x.equals(z)도 true다.
* 일관성(consistency) : null이 아닌 모든 참조 값 x, y에 대해, x.equals(y)를 반복해서 호출하면 항상 true를 반환하거나 항상 false를 반환한다.
* null-아님 : null이 아닌 모든 참조 값 x에 대해, x.equals(null)은 false다.