# 아이템 10 - equals는 일반 규약을 지켜 재정의하라

equals 메서드는 재정의하기에 쉬워 보이지만 곳곳에 함정이 도사리고 있다. 우선적으로 equals 메서드를 재정의하지 않아도 될 경우를 살펴보고, 재정의가 필요하다면 어떤 규약을 지켜서 재정의해야 하는지 알아보자.

---

## equals 메서드를 재정의 할 필요가 없을 때
다음 열거한 상황 중 하나에 해당된다면 equals 메서드를 재정의하지 않는것이 좋다.

#### 각 인스턴스가 본질적으로 고유하다.

값을 표현하는것이 나닌 동작하는 개채를 표현하는 클래스가 여기에 해당된다. 예시로 Thread와 같은 클래스가 여기 해당된다. Object의 equals 메서드는 이미 이러한 클래스에 딱 맞게 구현되어있다.

#### 인스턴스의 `논리적 동치성(logical equality)`을 검사할 일이 없다.

인스턴스의 논리적인 동치성을 검사해야 할 일이 있다면 그에 맞는 equals를 꼭 재정의해야 한다. 하지만 이런 방식을 원하지 않거나 애초에 필요하지 않다면 기본 equals 만으로 해결된다.

#### 상위 클래스에서 재정의한 equals가 하위 클래스에도 들어맞는다.

이러한 경우의 예시로는 Set의 구현체는 AbstractSet으로 부터, Map의 구현체들은 AbstractMap으로부터 equals를 그대로 상속받아 쓴다.

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

## equals 일반 규약 - 동치 관계 (equivalence relation)
euqals 메서드는 동치관계를 구현하며, 다음을 만족한다.
* 반사성(reflexivity) : null 아닌 모든 참조 값 x에 대해, x.equals(x)는 true다.
* 대칭성(symmetry) : null이 아닌 모든 참조 값 x, y에 대해 x.equals(y)가 true면 y.equals(x)도 true다.
* 추이성(transitivity) : null이 아닌 모든 참조 값 x, y, z에 대해 x.equals(y)가 true아고 y.equals(z)도 true면 x.equals(z)도 true다.
* 일관성(consistency) : null이 아닌 모든 참조 값 x, y에 대해, x.equals(y)를 반복해서 호출하면 항상 true를 반환하거나 항상 false를 반환한다.
* null-아님 : null이 아닌 모든 참조 값 x에 대해, x.equals(null)은 false다.

그렇다면 일반 규약들과 이를 위배하는 상황에 대해 알아보자

---

### 반사성 (reflexivity)
반사성은 객체는 자기 자신과 같아야 한다는 규약이다. 이 규약을 위반하는 경우는 없을 것이다.

---

### 대칭성 (symmetry)
대칭성은 두 객체는 서로에 대한 동치 여부에 똑같이 답해야 한다는 규약이다.

부모 클래스 A와 A를 상속하는 B가 존재하고 각각 equals를 구현했다고 가정해보자. A는 비교하는 인스턴스가 A인지 판단한 후 값을 비교하는 equals를 갖고 있다. B도 비교하는 인스턴스가 B인지 판단하고 값을 비교한다면 이 경우 대칭성을 위배한다.
```java
public class A {
    int val = 10;

    // ...

    @Override
    public boolean equals(Object o) {
        if (!(o instanceof A)) // B가 들어와도 A객체이기 때문에 false를 반환하지 않는다.
            return false;
        A a = (A) o;
        return this.val == a.val;
    }
}

public class B extends A {
    int val2 = 20;

    // ...

    @Override
    public boolean equals(Object o) {
        if (!(o instanceof B))
            return false;
        B b = (B) o;
        return super.equals(b) && (this.val2 == b.val2);
    }
}

A a = new A();
B b = new B();

System.out.println(a.equals(b)); // true
System.out.println(b.equals(a)); // false
```

---

### 추이성 (transitivity)
추이성은 첫 번째 객체와 두 번째 객체가 같고, 두 번째 객체와 세 번째 객체가 같다면, 첫 번째 객체와 세 번째 객체도 같아야 한다는 규약이다.

추이성을 어기는 예시는 상속관계에서 나타날 수 있다.
A 클래스에 equals를 구현하고, A를 상속하는 두 개의 클래스 B와 C 클래스가 각각 자신에게 맞는 equals를 구현했다고 가정하자.
이때 B와 C가 A와의 대칭성을 위해 equals에 자신들의 부모 클래스인 A 클래스에 대한 equals를 만들었다면, `A = B`, `A = C`를 만족하며 결과적으로 `B = C`가 되어야하지만 그렇지 못하다.

```java
A a = new A();
B b = new B(); // B extends A
C c = new C(); // C extends A

System.out.println(a.equals(b)); // true
System.out.println(b.equals(a)); // true

System.out.println(a.equals(c)); // true
System.out.println(c.equals(a)); // true

System.out.println(b.equals(c)); // false
System.out.println(c.equals(b)); // false
```

**구체 클래스를 확장해 새로운 값을 추가하면서 equals 규약을 만족시킬 방법은 존재하지 않는다.** 대신 우회 방법이 존재하는데, 상속 대신 컴포지션을 사용하는 것이다.

---

### 일관성 (consistency)
일관성은 두 객체가 같다면 앞으로도 영원히 같아야 한다는 규약이다. 반대로 두 객체가 다르다면 영원히 달라야 한다.

그렇기에 equals를 구현하는 클래스는 **equals의 판단에 신뢰할 수 없는 자원이 끼어들게 해서는 안된다.**

일관성을 위배하는 경우는 신뢰할 수 없는 자원이 끼어들었을 때이다. 예시로는 java.net.URL의 equals를 들 수 있다. URL 클래스의 equals는 매핑된 호스트의 IP 주소를 이용해 비교한다. 호스트 이름을 IP 주소로 바꾸려면 네트워크를 통해야 하는데, 그 결과가 항상 같다고 보장할 수 없다.

---

### null-아님
null-아님은 모든 객체가 null과 같지 않아야 한다는 것이다. 이 경우에 대해서는 대부분의 equals 메서드에서 `instanceof` 문을 통해 필터링하는 것으로 예방할 수 있다.

---
## equals 메서드의 구현 방법

대부분의 equals 메서드의 구현 방법은 다음과 같다.

1. `==` 연산자를 이용해 자기 자신의 참조인지 확인한다.
2. `instanceof` 연산자로 입력이 올바른 타입인지 확인한다.
3. 입력을 올바른 타입으로 형 변환한다. 이 경우는 2번 검사를 이미 진행한 후 일어날 일이라 반드시 성공하게 된다.
4. 입력 객체와 자기 자신의 대응되는 **핵심** 필드들이 모두 일치하는지를 하나씩 검사한다.

4번 과정에는 몇 가지 주의 사항이 존재한다. float와 double 같은 형은 특수한 부동소수값을 다루어야 하므로 `==` 연산 대신 Float.compare() 메서드나 Double.compare() 메서드를 사용해야 한다. 배열의 모든 원소가 핵심 필드라면 Arratys.equals 메서드를 사용하면 된다. 또한 null도 정상 값으로 취급하는 참조 타입도 존재한다. 이 경우 Object.equals()로 비교해 NullPointerException을 예방할 수 있다.

## equals 구현과 사용 팁
equals의 성능에 관한 이야기이다. 어떤 필드를 먼저 검사하느냐가 equals의 성능을 좌우할 수 있다. 최상의 성능을 위해서는 다를 가능성이 더 크거나 비교하는 비용이 싼 필드를 먼저 비교하는 것이 맞다. 핵심 필드를 통해 도출되는 파생 필드가 객체의 전체 상태를 대표한다면 이런 파생 필드를 먼저 비교하는 것도 좋은 전략이 된다.

equals 구현 시 주의 사항이다.
* equals를 재정의할 땐 hashCode도 반드시 재정의하자
* Object 외의 타입을 매개변수로 받는 equals 메서드는 선언하지 말자

---
### 정리
> 꼭 필요한 경우인지 판단하여 equals를 구현해야 한다. 필요한 경우라면 다섯 가지 equals 규약을 잘 지켜서 구현하는 것이 이후 발생할 혼란을 예방해 줄 것이다.