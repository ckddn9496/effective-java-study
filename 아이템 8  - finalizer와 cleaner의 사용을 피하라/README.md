# 아이템 8 - finalizer와 cleaner의 사용을 피하라

자바는 두 가지 객체 소멸자를 제공한다. **finalizer**와 **cleaner**이며 해당 소멸자의 사용은 예측할 수 없고, 상황에 따라 위험할 수 있어 일반적으로 불필요하다.

## 파괴자

자바의 finalizer와 cleaner는 C++에서의 파괴자(destructor)와 다른 개념이다. C++의 파괴자는 특정 객체와 관련된 자원을 회수하는 보편적인 방법임에 반해 자바에서는 가비지 컬렉터가 그 작업을 담당하고, 프로그래머에게는 아무런 작업도 요구하지 않는다. C++의 파괴자는 비 메모리 자원을 회수하는 용도로도 쓰인다. 하지만 자바에서는 `try-with-resources`와 `try-finally`를 사용해 해결한다.

finalizer와 cleaner는 즉시 수행된다는 보장이 없다. 객체에 접근할 수 없게 된 후 finalizer와 cleaner가 실행되기까지 얼마다 시간이 걸릴지는 알 수 없다. 즉, finalizer와 cleaner로는 제때 실행되어야 하는 작업은 절대 할 수 없다. finalizer와 cleaner의 수행은 가비지 컬렉터 알고리즘에 달렸으며, 이는 가비지 컬렉터 구현마다 천차만별이다.

## finalizer와 cleaner 사용으로 일어날 문제

finalizer와 cleaner를 사용할 때 발생할 수 있는 문제들은 다음과 같다.

### 메모리 문제

finalizer 스레드가 다른 애플리케이션 스레드보다 우선순위가 낮아 실행할 기회를 얻지 못하면 해당 인스턴의 자원의 회수가 잘 일어나지 못해 원인을 알지못할 `OutOfMemoryError`가 발생하기도 한다. 또한 finalizer 동작 중 발생한 예외는 무시되며, 처리할 작업이 남았더라도 그 순간 종료된다. 잡지못한 예외 때문에 해당 객체는 마무리가 되지 못한 채 남아있을 수 있다. 보통의 경우엔 잡지 못한 예외가 스레드를 중단시키고 스택 추적 내용(Stack trace)을 출력하겠지만, 백그라운드에서 수행되는 finalizer에서 발생한다면 경고조차 출력하지 않는다.

그나마 cleaner는 자신을 수행할 스레드를 통제하기 때문에 위와 같은 문제는 발생하지 않는다. 하지만 여전히 백그라운드에서 수행되며 가비지 컬렉터의 통제하에 있으니 즉각 수행된다는 보장은 없다.

### 성능 문제

JVM은 finalizer나 clenaner가 존재하지 않는 객체에 대해 생성, 파괴할 때 더 우수한 성능을 보인다. 간단한 `AutoCloseable` 객체를 생성하고 가비지 컬렉터가 수거하기 까지의 시간에 비해, finalizer와 cleaner를 사용한다면 성능이 `50배`에서 `1000배`까지 느려지게된다. 구현에 따라 정확한 성능 저하의 정도는 다를 수 있지만 JVM이 fianlizer와 cleaner에 대한 객체를 파괴하는데 추가적인 작업이 발생하게 된다.

### finalizer 공격

finalizer를 사용한 클래스는 finalizer 공격에 노출되어 심각한 보안 문제를 일으킬 수 있다. 공격 방법은 생성자나 직렬화 과정에서 예외가 발생할 때, 이 생성되다 만 객체에서 악의적인 하위 클래스의 finalizer가 수행될 수 있게 한다. 이 finalizer는 정적 필드에 자신의 참조를 할당하여 가비지 컬렉터가 수집하지 못하게 막을 수 있다.

final 클래스는 하위 클래스를 만들 수 없으므로 이 공격에서 안전하지만, final이 아닌 클래스는 아무일도 하지 않는 finalize 메서드를 만들고 final로 선언하여 방어할 수 있다.

---

### AutoCloseable

파일이나 스레드 등 종료해야 할 자원을 담고 있는 객체에 대해 finalizer와 cleaner의 대안으로 `AutoCloseable`을 사용할 수 있다.

AutoCloseable은 해당 객체가 close 될때까지 자원을 갖고 있는다. try-with-resources 와 함께 사용되어야 하며 해당 구문이 종료될 때 자동으로 close 메서드를 실행시킨다.

---

### finalizer와 cleaner의 쓰임

AutoCloseable이 finalizer와 cleaner의 역할을 대신해 준다면 이들은 어디에 쓰일까? finalizer와 cleaner의 적절한 쓰임새는 다음과 같다.

1. 자원의 소유자가 close 메서드를 호출하지 않는 것에 대한 안전망 역할

finalizer와 cleaner가 즉시 호출된다는 보장은 없지만, 클라이언트가 자원 회수를 해주지 않는 것에 대해 늦게라도 해주는 것이 아예 하지 않는 것보다는 낫다. 하지만 이 또한 심사숙고해서 사용해야 한다. 이미 자바 라이브러리의 일부 클래스는 이런 안전망인 finalizer를 제공한다 (FileInputStream, ThreadPoolExecutor)

2. 네이티브 피어(native peer)와 연결된 객체

네이티브 피어란 일반 자바 객체가 네이티브 메서드를 통해 기능을 위임한 네이티브 객체를 말한다. 네이티브 피어는 자바 객체가 아니므로 가비지 컬렉터는 네이티브 객체의 존재를 알지 못한다. 그렇기에 회수하지 못하며 이럴 때 finalizer와 cleaner를 이용해 자원을 회수해줄 수 있다.

---

### 정리

> finalizer와 cleaner는 네이티브 피어에 대한 자원 회수에만 사용하는 것이 적당하다고 한다. 이 또한 성능 저하를 고려했을 때 사용할만하다고 판단될 때만 이다. AutoCLoseable이라는 대체 제이자 더 좋은 인터페이스가 있으므로 사용 후 회수되어야 할 자원이 있다면 AutoCloseable과 try-with-resources를 이용하고, finalizer와 cleaner는 되도록 사용하지 않는 것이 좋다.
>
> 참고로 finalizer는 Java 9부터 deprecated 되었고 그의 개선된 버전으로 cleaner가 나온 것이다. 그렇기에 사용해야 한다면 Java 8 이하는 finalizer, Java 9 이상은 cleaner를 사용하도록 하자.
