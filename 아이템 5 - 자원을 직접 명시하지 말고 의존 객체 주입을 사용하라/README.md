# 아이템 5 - 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라

많은 클래스는 하나 이상의 자원에 의존한다. 그리고 이런 클래스들을 정적 유틸리티 클래스나 싱글턴 방식으로 만들어 사용하는 모습을 볼 수 있다. 하지만 그런 방법은 좋은 방법이 아니다.

맞춤법 검사 로직을 담고 사전이라는 자원(resource)에 의존하는 클래스를 다음과 같은 방법으로 만들었을 때를 살펴보자. 차례대로, 정적 유틸리티 클래스와 싱글턴 방식의 예시이다.

- 정적 유틸리티 클래스

```java
public class SpellChecker {
    private static final Lexicon dictionary = ...;

    private SpellChecker() {} // 객체 생성 방지

    public static boolean isValid(String word) { ... };
    public static List<String> suggestion(String typo) { ... };
}
```

- 싱글턴

```java
public class SpellChecker {
    private final Lexicon dictionary = ...;

    private SpellChecker(...) {}
    public static SpellChecker INSTANCE = new SpellChecker(...);

    public boolean isValid(String word) { ... };
    public List<String> suggestion(String typo) { ... };
}
```

이런 방법들은 유연하지 못하며 테스트 하기 어렵다. 위의 코드들은 하나의 사전에 대해서만 지원 가능하다. 실제 사용에서 언어별 사전, 특수 어휘용 사전, 테스트용 사전 등도 고려되어야 하지만 위의 코드는 이런 것들을 커버해주지 못한다.

---

## 여러 자원을 이용할 수 있도록 만들어주기 위한 수정 - 의존 객체 주입

위의 예시인 SpellChecker가 여러 사전을 사용할 수 있도록 만들어보자. 간단히 dictionary 필드에서 final 한정자를 제거하고 다른사전으로 교체하도록 만들어 줄 수도 있지만 이런 방법은 오류를 내기 쉬우며 멀티쓰레드 환경에서 쓸 수 없다. **사용하는 자원에 따라 동작이 달라지는 클래스에는 정적 유틸리티 클래스나 싱글턴 방식이 적합하지 않다.**

클래스는 여러 자원 인스턴스를 지원해야하며 클라이언트가 원하는 자원을 사용해야한다. 그리고 이럴 경우에 사용할 수 있는 패턴은 인스턴스 생성 시 생성자에 필요한 자원을 넘겨주는 방식이다. 이는 **의존 객체 주입**의 한 형태이다.

```java
public class SpellChecker {
    private final Lexicon dictionary;

    public SpellChecker(Lexicon dictionary) { // 생성자 호출 시 의존 객체(자원) 주입
        this.dictionary = dictionary;
    }

    public boolean isValid(String word) { ... };
    public List<String> suggestion(String typo) { ... };
}
```

## 의존 객체 주입의 장점

의존 객체주입을 사용한다면 원하는 자원을 통해 동작할 수 있도록 **유연함**을 보장할 수 있다. 또한 생성자에서 초기화를 한번 해주기 때문에 final 한정자를 이용하여 **자원의 불변함**을 보장할 수 있다. 그렇기에 여러 클라이언트가 의존 객체들을 안심하고 공유할 수 있다.

의존 객체를 직접 주입하는 것의 변형으로 생성자에 자원 팩토리를 넘겨줄 수도 있다. 이때 한 가지 주의할 점은 팩토리의 타입 매개변수를 제한해야 한다는 것이다. 이 방식을 통해 클라이언트는 자신이 명시한 타입의 하위 타입이라면 무엇이든 생성할 수 있는 팩토리를 넘길 수 있다.

- #### `Supplier<T>`

  자바 8에서 등장한 `Supplier<T>` 인터페이스는 팩토리를 완벽히 표현한 예시이다.

        Mosaic create(Supplier<? extends Tile> tileFactory) { ... }

  위의 함수는 매개변수 타입을 <? extends Tile> 통해 제한하였다. 클라이언트가 제공하는 팩토리는 Tile의 하위 타입만을 생성해야 한다고 제한할 수 있다.

## 의존 객체 주입의 단점

의존 객체 주입이 유연성과 테스트 용이성을 개선해주지만, 너무 많은 의존성을 가지면 코드를 어지럽게 만들 수 있다. 이런 문제는 스프링(Spring)과 같은 의존 객체 주입을 도와주는 프레임워크를 사용하여 해소할 수 있다.

---

### 정리

> 클래스가 내부적으로 하나 이상의 자원에 의존하고, 그 자원이 클래스 동작에 영향을 준다면 싱글턴과 정적 유틸리티 클래스는 사용하지 않는 것이 좋다. 대신 필요한 자원 또는 그 자원을 만들어주는 팩토리를 생성자에 넘겨주는 의존 객체 주입을 사용할 수 있다. 의존 객체 주입은 클래스의 유연성, 재사용성, 테스트 용이성을 개선해준다.
