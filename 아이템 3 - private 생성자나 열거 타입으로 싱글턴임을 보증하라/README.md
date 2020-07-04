# 아이템 3 - private 생성자나 열거 타입으로 싱글턴임을 보증하라

## 싱글턴 (Singleton)
싱글턴이란 인스턴스를 오직 하나만 생성할 수 있는 클래스를 말한다. 싱글턴의 전형적인 예로는 함수와 같은 무상태(stateless) 객체나 설계상 유일해야 하는 시스템 컴포넌트를 들 수 있다. 그런데 **클래스를 싱글턴으로 만들면 이를 사용하는 클라이언트를 테스트하기 어려워 질 수 있다.** 타입을 인터페이스로 정의한 다음 그 인터페이스를 구현해서 만든 싱글턴이 아니라면 싱글턴 인스턴스를 **mock 구현**으로 대체할 수 없기 때문이다.

## 싱글턴을 만드는 방법
싱글턴을 만드는 방식은 보통 아래를 따른다.
1. 생성자는 private 으로 감춘다
2. 유일한 인스턴스에 접근할 수 있는 수단으로 public static 멤버를 마련한다.

public static 멤버는 두가지 방법으로 구현가능하다. 하나는 필드 변수로 두는 방법, 나머지 하나는 접근 메서드로 두는 방법이다.

* ### public static final 필드 방식의 싱글턴
```java
public class Singleton {
    public static final Singleton INSTANCE = new Singleton();
    private Singleton() {
        // init
    }

    // methods...
}
```
private 생성자는 public static final 필드인 Singleton.INSTANCE를 **초기화 할 때 딱 한번 호출**된다. 그러므로 인스턴스가 전체 시스템에서 하나뿐임이 보장된다.

예외는 권한이 있는 클라이언트가 리플렉션 API를 사용하여 private 생성자를 호출하는 것이다. 이러한 공격을 방어하기 위해서는 생성자를 수정하여 두 번째 객체가 생성되려 할 때 예외를 던지게 하면 된다.

> 예외를 만들기 위한 리플렉션 API의 예시
```java
import java.lang.reflect.*; // 리플렉션 API 사용에 필요한 라이브러리를 추가한다.

public class SingletonReflectionTest {

    public static void main(String[] args) throws Exception {
        Singleton s1 = Singleton.INSTANCE;
        System.out.println("s1: " + s1.getNum());   //  s1: 100

        Singleton s2 = Singleton.INSTANCE;
        s2.setNum(101);
        System.out.println("s1: " + s1.getNum());   //  s1: 101
        System.out.println("s2: " + s2.getNum());   //  s2: 101
        System.out.println("s1.equals(s2): " + s1.equals(s2));  //  s1.equals(s2): true
        
        // 리플렉션 API를 사용해 private 생성자를 접근가능하게 한다.
        Constructor<Singleton> constructor = (Constructor<Singleton>) Singleton.class.getDeclaredConstructors()[0];
        constructor.setAccessible(true);
        
        Singleton s3 = constructor.newInstance();   //  새로운 Singleton 객체 생성
        s3.setNum(200);
        
        System.out.println("s1: " + s1.getNum());   //  s1: 101
        System.out.println("s2: " + s2.getNum());   //  s2: 101
        System.out.println("s3: " + s3.getNum());   //  s3: 200
        System.out.println("s1.equals(s3): " + s1.equals(s3));  //  s1.equals(s3): false
    }
    
    public static class Singleton {
        public static final Singleton INSTANCE = new Singleton();
        private int num;

        private Singleton() { num = 100; }

        public void setNum(int num) { this.num = num; }
        public int getNum() { return this.num; }
    }
}
```
***

* ### 정적 팩토리 방식의 싱글턴
```java
public class Singleton {
    private static final Singleton INSTANCE = new Singleton();
    private Singleton() {
        // init
    }
    public static Singleton getInstance() { return INSTANCE; }
    
    // methods...
}
```

**Singleton.getInstance()**는 항상 같은 객체의 참조를 반환하므로 제2의 Singleton 인스턴스는 결고 만들어지지 않는다. 리플렉션에 대한 예외는 동일하게 존재한다.

### 두 방식의 차이점
public 빌드 방식의 가장 큰 특징은 싱글턴임이 API에 명백히 드러난다는 점이다. 그리고 `getInstance()`와 같은 메서드가 없어 더 간결하다.

정적 팩토리 방식의 장점은 API를 바꾸지 않고도 싱글턴이 아니게 변경할 수 있다는 점이다. 또한, 정적 팩터리를 제네릭 싱글턴 팩토리나 공급자(supplier)로 사용할 수 있다는 점이다.
***

## 싱글턴 클래스의 직렬화
싱글턴 클래스를 직렬화하려면 단순히 Serializable을 선언하는 것만으로는 부족한다. 모든 인스턴스 필드를 일시적(transient)이라고 선언하고 [readResolve](https://docs.oracle.com/javase/7/docs/platform/serialization/spec/input.html#2971) 메서드를 제공해야한다. 이렇게 하지 않으면 인스턴스를 역질렬화할 때마다 새로운 인스턴스가 만들어진다.

* readResolve
**Serializable을 상속하는 하위클래스들은 자신만의 readResolve 메서드를 가진다.** 직렬화에는 **writeReplace**, 역직렬화(deserialization)에는 **readObject** 메서드를 자동으로 호출한다. 역직렬화 과정을 직접 제어하기 위해서는 readResolve 메서드를 직접 구현하면된다. 이 메서드를 직접 정의하여 역직렬화 과정에서 만들어진 인스턴스 대신에 기존에 생성된 싱글톤 인스턴스를 반환하도록 한다.

```java
private Object readResolve() {
    return INSTANCE;
}
```
readResolve 메서드를 구현했다고 해서 **자동으로 등록된 readReolve가 실행되지 않는것은 아니다**. 단지 클라이언트가 직접 구현한 readResolve의 결과물을 반환할 뿐이다. 그렇기에 자동으로 실행되는 readObject 결과물인 가짜 싱글턴 인스턴스는 가비지 컬렉터에의해 처리되게된다.

***

## 싱글턴을 만드는 다른 방법 - 열거형 (enum)
싱글턴을 만드는 세 번째 방법은 **원소가 하나인 열거 타입(enum)**을 선언하는 것이다. public 필드 방식과 비슷하지만 더 간결하며 추가 노력없이 직렬화 할 수 있다. 또한 아주 복잡한 직렬화 상황이나 리플렉션 공격에서도 제2의 인스턴스가 생기는 일을 완벽히 막아준다. 대부분 상황에서 원소가 하나뿐인 열거 타입이 싱글턴을 만드는 가장 좋은 방법이다. 단, 만드려는 싱글턴이 Enum 외의 클래스를 상속해야 한다면 이 방법은 사용할 수 없다.

> enum 클래스는 내부적으로 `Enum<T>` 클래스를 상속(extends)받고있다. 그렇기에 다른 클래스를 상속받을수는 없다. 그렇기에 enum의 기능을 계승하여 사용하고 싶다면 interface를 만들어서 구현하게 해야한다.

***
### 정리
> 싱글턴을 만드는 방법은 아래와 같다
> * public static final 필드
> * 정적 팩토리
> * 원소가 하나인 열거 타입(enum)
> 
> 가장 안전하고 성공적인 방법은 enum을 사용하는것이지만, 싱글턴을 위해 만든 열거형이라는것에 대해서 주석을 통해 자세한 설계 방법을 명시해주어야 할 것 같다. 