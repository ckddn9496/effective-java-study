# 아이템 13 - clone 재정의는 주의해서 진행하라

clone 메서드를 잘 동작하게 해주는 구현 방법과 주의할 점들에 대해 알아보자.

## Cloneable

Cloneable은 복제해도 되는 클래스임을 명시하는 용도의 믹스인 인터페이스이다. 하지만 이 인터페이스는 의도한 목적을 잘 이루지 못하였다. 가장 큰 문제는 clone 메서드가 선언된 곳이 Cloneable이 아닌 Object이고 그 마저도 protected이다. 그렇기에 Cloneable을 구현하는 것만으로는 외부에서 clone 객체를 호출할 수 없다. 그럼에도 불구하고 Cloneable 방식은 많이 사용되고 있기 때문에 알아둘 필요가 있다.

Cloneable을 구현한 클래스의 인스턴스에서 clone을 호출하면 그 객체의 필드를 하나하나 복사흔 객체를 반환하며, 그렇지 않은 클래스의 인스턴스에서 호출하면 CloneNotSupportedException을 던진다.

> Cloneable을 구현하지 않으면 예외를 던지는 것으로 보아 기본 Objeect의 clone 메서드는 다음과 깉이 생기지 않았을까? 추측해본다.

```java
class Object {
    ...

    protected Object clone() {
        if (this instanceof Cloneable) {
            // clone instance
            return ...;
        } else {
            throw new CloneNotSupportedException();
        }
    }
}
```

명세에서는 이야기 하지 않지만 실무에서 Cloneable을 구현한 클래스는 clone 메서드를 public으로 제공하며, 사용자는 당연히 복제가 잘 이루어질 것이라고 생각한다.

clone 메서드의 일반 규약은 매우 허술하다.

> x.clone() != x -> True  
>   
> x.clone().getClass() == x.getClass() -> True  
>   
> x.clone().equals(x) -> 일반적으로 True이어야 하지만 필수는 아니다 (?)  
>   
> 관례상, 반환된 객체와 원본 객체는 독립적이어야 한다. 이를 만족 시기키 위해 super.clone으로 얻은 객체의 필드 중 하나 이상을 반환 전에 수정해야 할 수 도 있다.

## clone 메서드 구현

자바는 공변 반환 타이핑(covariant return typing)을 지원한다. 반환 타입이 Object인 clone() 메서드를 구현할 때는 구현하는 클래스의 객체 타입으로 반환해줄 수 있도록 하자.

> covariant retrun typing: 재정의한 메서드의 반환 타입은 상위 클래스의 메서드가 반환하는 타입의 하위 타입일 수 있다.

## clone 구현 시 주의사항

```java
public class Example {
    private Object[] elements;

    ...
}
```

위와 같이 가변 객체를 가진 클래스를 clone 할때 주의해야 할 점이 있다. clone 메서드는 분명 동일한 값을 가진 인스턴스를 반환할 것이다. 하지만 원본이나 복제본 중 하나를 수정하면 다른 하나도 수정되어 불변식을 해치게 된다. clone은 생성자와 같은 효과를 내지만 clone은 원본 객체에 아무런 해를 끼치지 않는 동시에 복제된 객체의 불변식을 보장해야 한다. 위의 예제 코드에서 이를 방지하는 가장 좋은 방법은 elements 배열의 clone을 재귀적으로 호출해 주는 것이다.

```java
@Override
public Example clone() {
    try {
        Example result = (Example) super.clone();
        result.elements= elements.clone();
        return result;
    } catch (CloneNotSupportedException e) {
        throw new AssertionError();
    }
}
```

배열의 clone은 런타임 타입과 컴파일타임 타입 모두가 원본 배열과 똑같은 배열을 반환하기에 배열 복사를 위해서는 clone이 권장된다.

이제 다른 상황을 살펴보자. Example2 클래스는 내부의 가변 상태를 갖는 클래스를 배열로 가지고 있다.

```java
public class Example2 {
    private Entry[] entries;

    public static class Entry {
        String key;
        int value;
        Entry next;

        ...
    }

    ...
}
```

Example2 클래스를 clone 한다면 어떨까?

```java
@Override
public Example2 clone() {
    try {
        Example2 result = super.clone();
        result.entries = entries.clone();
        return result;
    } catch (CloneNotSupportedException e) {
        throw new AssertionError();
    }
}
```

Entry 배열은 잘 복사될 것이다. 하지만 복사본 Entry가 참조하는 next 객체는 원본과 같은 Entry를 참조할 것이다. 그렇기 때문에, clone를 재정의하려는 클래스의 속성에 따라 완전한 copy가 일어날 수 있도록 deepCopy에 대하여 구현해줄 필요가 있다.

```java
public static class Entry {
    String key;
    int value;
    Entry next;

    ...

    // Entry 객체에 대한 deepCopy
    Entry deepCopy() {
        return new Entry(key, value, next == null ? null : next.deepCopy());
    }
}

@Override
public Example2 clone() {
    try {
        Example2 result = super.clone();
        result.entries = new Entry[entries.length];
        for (int i = 0; i < entries.length; i++) {
            if (entries[i] != null) {
                result.entries[i] = entries[i].deepCopy(); // 재귀적으로 deepCopy
            }
        }
        return result;
    } catch (CloneNotSupportedException e) {
        throw new AssertionError();
    }
}
```

위 방법은 재귀적인 호출을 통해 리스트를 clone한다. 하지만 재귀적 호출에는 단점이 있다. 리스트의 원소 수만큼 Stack Frame을 소비하여, 혹시나 스택 오버플로를 일으킬 수 있는 위험이 있기 때문이다. 이 문제를 피하기 위해선 반복자를 통해 순회하는 방법으로 수정해야 한다.

```java
public static class Entry {
    String key;
    int value;
    Entry next;

    ...

    // Entry 객체에 대한 deepCopy - 재귀가 아닌 반복자를 이용하여 stack overflow 방지
    Entry deepCopy() {
        Entry result = new Entry(key, value, next);
        for (Entry p = result; p.next != null; p = p.next {
            p.next = new Entry(p.next.key, p.next.value, p.next.next);
        }    
        return result;
    }
}

```

## Catch CloneNotSupportedException

clone을 구현한 예제들에서 try-catch 문을 이용하여 CloneNotSupportedException을 감싸는 코드를 확인할 수 있다. 이는 Object의 clone 메서드가 checked exception인 CloneNotSupportedException을 던지도록 선언되어있기 때문이다. 만약 그대로 CloneNotSupportedException을 던진다면 클라이언트 단에서 clone()을 사용한다면 그곳에 catch혹은 메서드 밖으로 throw 하는 추가적인 보충이 들어가야 한다. 그리고 이는 clone 사용을 매우 지저분하게 만든다. 그렇기에 재정의하는 clone메서드 내부에서 이를 처리하도록 해주자.

---

### 정리

> Cloneable을 구현하는 모든 클래스는 clone을 재정의해야 한다. 이때 접근 제한자는 public으로, 반환 타입은 클래스 자신으로 변경한다. 이 메서드는 가장 먼저 super.clone을 호출한 후 필요한 필드를 전부 적절히 수정한다.  
>   
> 꼭 Cloneable을 사용하지 않아도 된다면 복사 생성자와 복사 팩토리를 사용하도록 하자.