# 아이템 11 - equals를 재정의하려거든 hashCode도 재정의하라

equals를 재정의한 클래스 모두에서 **hashCode**도 재정의해야 한다. 그렇지 않으면 hashCode 일반 규약을 어기게 되어 해당 클래스의 인스턴스를 HashMap이나 HashSet 같은 컬렉션의 원소로 사용할 때 문제를 일으키게 된다.

## hashCode 재정의의 필요성

아래의 내용은 Object 명세 규약에 포함된 내용이다.

- equals 비교에 사용되는 정보가 변형되지 않았다면, 애플리케이션이 실행되는 동안 객체의 hashCode 메서드는 몇 번을 호출해도 일관되게 항상 같은 값을 반환해야 한다. 단, 애플리케이션을 다시 실행한다면 이 값이 달라져도 상관없다.
- equals(Object)가 두 객체를 같다고 판단했다면, 두 객체의 hashCode는 똑같은 값을 반환해야 한다.
- equals(Object)가 두 객체를 다르다고 판단했더라도, 두 객체의 hashCode가 서로 다른 값을 반환할 필요는 없다. 단, 다른 객체에 대해서는 다른 값을 반환해야 해시테이블의 성능이 좋아진다.

hashCode를 재정의하지 않았을 때의 가장 큰 문제가 되는 조항은 두 번째 조항이다. **논리적으로 같은 두 객체는 반드시 같은 해시코드를 반환해야 한다.**

### hashCode를 재정의하지 않았을 때

해시 관련 컬렉션(HashMap, HashSet...)은 해시 코드가 다른 엔트리끼리는 동치성 비교를 시도조차 하지 않도록 최적화되어있다 (hashCode가 같아야만 equals를 판단한다). 그렇기에 hashCode를 재정의 하지 않은 클래스는 논리적으로 동치이고 equals를 구현했더라도 해시 컬렉션은 이를 다른 객체로 인식한다.

### 잘못된 hashCode 구현의 예

```java
@Override
public int hashCode() {
    return 42;
}
```

위의 hashCode는 모든 객체에서 똑같은 해시 코드를 반환하니 동치성 검사를 수행하도록 만들어줄 수 있다. 하지만 모든 객체에게 똑같은 값을 내어주므로 모든 객체가 해시 테이블의 버킷 하나에 담겨 `LinkedList`처럼 동작하게 된다. 그 결과 평균 수행 시간이 `O(1)`인 해시 테이블이 `O(n)`으로 느려져 객체가 많아지면 쓸 수 없게 된다.

좋은 해시 함수는 서로 다른 인스턴스에 대해 다른 해시 코드를 반환한다. 이상적인 해시함수는 hashCode의 반환형인 `int`크기에 맞게 32비트 정수 범위에서 균일하게 분배해야 한다.

### 간단한 hashCode 구현 요령

아래의 예시와 함께 살펴보자

```java
@Override
public int hashCode() {
    int result = Short.hashCode(areaCode); // 초기화, Type.hashCode()
    result = 31 * result + Short.hashCode(prefix); // 다음 필드값을 포함하도록 hashCode 갱신
    result = 31 * result + Short.hashCode(lineNum); // 다음 필드값을 포함하도록 hashCode 갱신
    return result; // hashCode 반환
}
```

1. int 변수 result를 선언한 후 값 c로 초기화
   - 값 c란 핵심 필드 f에 대해 계산한 해시 코드값을 말한다.
     - 기본 타입 필드에 대해서는 Type.hashCode(f)를 수행한다.
     - 참조 타입 필드면서 이 클래스의 equals 메서드가 이 필드의 equals를 재귀적으로 호출해 비교한다면, 이 참조 필드의 hashCode를 재귀적으로 호출한다. 참조 필드의 값이 null이라면 0을 사용한다.
     - 필드가 배열이라면 핵심 원소 각각을 별도 필드로 다룬다. 모든 배열의 원소가 핵심 필드라면 Arrays.hashCode를 이용한다.
2. 해당 객체의 나머지 핵심 필드 f 각각에 대해 해시코드 값 c를 가져온 후 result를 갱신한다.
   - result = 31 \* result + c;
3. result를 반환한다.

파생 필드는 해시 코드 계산에서 제외해도 되며, equals 비교에 사용되지 않는 필드는 **반드시** 제거해야 한다.

> 31을 곱하는 이유는 소수이기 때문에 오버플로우가 일어나도 값의 고유함을 어느 정도 보장해줄 수 있기 때문이다. 그렇다면 왜 하필 `31`이라는 소수 값일까? 이는 간단한 시프트 연산과 뺄셈을 통해 해결할 수 있기 때문이다. `31 * i`는 `(i << 5) - i` 연산으로 최적화할 수 있다. 또한 요즘 VM들은 이러한 최적화를 자동으로 해준다.

### 캐싱을 이용한 hashCode

클래스가 불변이고 해시 코드를 계산하는 비용이 크다면, 매번 새로 계산하기보다는 캐싱하는 방식을 고려할 수 있다.

```java
private int hashCode; // 자동으로 0으로 초기화

@Override
public int hashCode() {
    int result = hashCode;
    if (result == 0) {
        int result = Short.hashCode(areaCode); // 초기화, Type.hashCode()
        result = 31 * result + Short.hashCode(prefix); // 다음 필드값을 포함하도록 hashCode 갱신
        result = 31 * result + Short.hashCode(lineNum); // 다음 필드값을 포함하도록 hashCode 갱신
        hashCode = result;
    }
    return result; // hashCode 반환
}
```

이때 주의할 점은 hashCode 필드의 초깃값은 흔히 생성되는 객체의 해시 코드와 달라야 한다는 점이다.

- 실제로 String의 hashCode 구현도 같은 캐싱을 이용한다.

```java
/** Cache the hash code for the string */
private int hash; // Default to 0

// Returns a hash code for this string
@Override
public int hashCode() {
    int h = hash;
    if (h == 0 && value.length > 0) {
        char val[] = value;

        for (int i = 0; i < value.length; i++) {
            h = 31 * h + val[i];
        }
        hash = h;
    }
    return h;
}
```

### hashCode 구현 시 주의사항

1. **hashCode를 계산할 때 발생하는 비용을 줄이고자 핵심 필드를 생략해서는 안된다.** 계산 성능을 위해 핵심 필드들을 제외하고 계산하는 경우가 있는데, 오히려 이런 행위는 해시의 품질을 떨어뜨리며 결과적으로 해시 테이블의 성능을 심각하게 떨어뜨릴 수 있다.

   > 실제 자바 2 전의 String은 최대 16개 문자만으로 해시 코드를 계산했다. 길이가 길 경우 16자만 잘라서 사용하였고 이런 행위로 수많은 인스턴스가 단 몇 개의 해시 코드로 집중되어 해시 테이블의 속도에 심각한 문제를 일으켰었다.

2. **hashCode가 반환하는 값의 생성 규칙을 API 사용자에게 자세히 공표하지 말자.** 그래야 클라이언트가 이 값에 의지하지 않게 되고, 추후에 계산방식을 바꿀 수도 있다.

---

### 정리

> equals를 재정의할 때는 hashCode도 반드시 재정의해야 한다. 재정의한 hashCode는 Object API문서의 일반 규약을 따라야 하며, 서로 다른 인스턴스이면 hashCode도 서로 다르게 구현해야 한다. AutoValue 프레임워크를 사용하면 equals와 hashCode를 자동으로 만들어 주기 때문에 이런 프레임워크를 이용하는 것도 하나의 좋은 방법인 것 같다.
