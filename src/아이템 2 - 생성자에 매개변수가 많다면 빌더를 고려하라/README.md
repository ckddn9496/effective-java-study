# 아이템 2 - 생성자에 매개변수가 많다면 빌더를 고려하라

정적 팩토리나 생성자에는 선택적 매개변수가 많을때 적절히 대응하기 힘들다는 단점이 존재한다. 이런 단점들을 빌더를 이용하여 해결할 수 있다. 선택적 매개변수가 많은 경우 정측적 생성자 패턴, 자바빈즈 패턴, 빌더 패턴을 이용하여 해결하는 방법들과, 그 중 빌더 패턴이 갖는 장점과 단점에 대해 알아보자. 


## 1. 점층적 생성자 패턴 (telescoping constructor pattern)
필수 매개변수와 선택 매개변수를 받는 생성자 부터 선택 매개변수를 전부 다 받는 생성자까지 늘려가며 만들어 놓는 방식이다. 하지만 이 방법은 매개변수의 개수가 많아질수록 클라이언트 코드를 작성하거나 읽기 어렵다. 의미를 파악하기 힘들며 어려운 버그로 이어질 수 있다.

```java
public class NutritionFacts {
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;
    
    public NutritionFacts(int servingSize, int servings) {
        this(servingSize, servings);
    }

    public NutritionFacts(int servingSize, int servings, int calories) {
        this(servingSize, servings, calories);
    }

    // ...

    public NutritionFacts(int servingSize, int servings, int calories, 
                        int fat, int sodium, int carbohydrate) {
        this.servingSize = servingSize;
        this.servings = servings;
        this.calories = calories;
        this.fat = fat;
        this.sodium = sodium;
        this.carbohydrate = carbohydrate;
    }
}
```

## 2. 자바빈즈 패턴 (JavaBeans pattern)
매개변수가 없는 생성자로 객체를 만든 후 세터(setter) 메서드들을 호출해 원하는 매개변수의 값을 설정하는 방식이다. 점층적 생성자 패턴의 단점을 해결할 수 있지만 객체 하나를 만들기 위해 여러 개의 메서드를 호출해야하며, 객체가 완전히 생성되기 전까지는 일관성(consistency)이 무너진 상태에 놓이게 된다.

```java
public class NutritionFacts {
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;

    // 빈 생성자    
    public NutritionFacts() { }

    // setters
    public void setServingSize(int servingSize)     { this.servingSize = servingSize; }
    public void setServings(int servings)           { this.servings = servings; }
    public void setCalories(int calories)           { this.calories = calories; }
    public void setFat(int fat)                     { this.fat = fat; }
    public void setSodium(int sodium)               { this.sodium = sodium; }
    public void setCarbohydrate(int carbohydrate)   { this.carbohydrate = carbohydrate; }
}
```

## 3. 빌더 패턴
클라이언트는 필요한 객체를 직접 만드는 대신, 필수 매개변수만으로 생성자를 호출해 빌더 객체를 얻는다. 그런 다음 빌더 객체가 제공하는 일종의 세터 메서드들로 원하는 선택 매개변수들을 설정한다. 마지막으로 매개변수가 없는 build 메서드를 호출해 필요한 객체를 얻는다.

빌더는 주로 생성할 클래스 안에 정적 멤버 클래스로 만들어둔다.

```java
public class NutritionFacts {
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;

    public static class Builder {
        // 필수 매개변수
        private final int servingSize;
        private final int servings;

        // 선택 매개변수        
        private int calories = 0;
        private int fat = 0;
        private int sodium = 0;
        private int carbohydrate = 0;

        public Builder(int servingSize, int servings) {
            this.servingSize = servingSize;
            this.servings = servings;
        }

        public Builder calories(int val) {
            calories = val; // calories를 초기화
            return this;    // 자기자신을 반환
        }

        /* 선택 매개변수들에 대한 Builders 추가 */

        // private 생성자를 통해 객체 생성 후 전달
        public NutritionFacts build() {
            return new NutritionFacts(this);
        }
    }

    private NutritionFacts(Builder builder) {
        servingSize = builder.servingSize;
        servings = builder.servings;
        calories = builder.calories;
        fat = builder.fat;
        sodium = builder.sodium;
        carbohydrate = builder.carbohydrate;
    }

}

```

빌더 패턴을 사용할 때 객체 초기화하는 예시이다

```java
NutritionFacts cocaCola = new NutritionFacts.Builder(240, 8)
    .calories(100).sodium(35).carbohydrate(27).build();
```

빌더를 사용한 클라이언트 코드는 쓰기 쉽고, 읽기 쉽다.

***

## 계층적으로 설계된 클래스에서의 빌더 패턴

빌더 패턴은 계층적으로 설계된 클래스와 함께 쓰기 좋다고한다. 각 계층의 클래스에 관련 빌더를 멤버로 정의해서 추상 클래스는 추상 빌더를, 구체 클래스는 구체 빌더를 갖도록 하는것이다.

```java
public abstract class Pizza {
    public enum Topping { HAM, MUSHROOM, ONION };
    final Set<Topping> toppings;

    abstract static class Builder<T extends Builder<T>> {
        EnumSet<Topping> toppings = EnumSet.noneOf(Topping.class);
        public T addTopping(Topping topping) {
            toppings.add(Objects.requireNonNull(topping));
            return self();
        }

        abstract Pizza build();

        // 하위 클래스는 이 클래스를 overriding 하여 "this"를 반환하게 해야 한다
        protected abstract T self();
    }

    Pizza(Builder<?> builder) {
        toppings = build.toppings.clone();
    }
}
```
Pizza.Builder 클래스는 재귀적 타입 한정을 이용하는 제네릭 타입이다. 여기에 추상 메서드인 self를 더해 하위 클래스에서는 형변환하지 않고도 메서드 연쇄를 지원할 수 있다. 이를 셀프타입 관용구라 한다.

하위클래스는 다음과 같이 작성된다.
```java
public class NyPizza extends Pizza {
    public enum Size { SMALL, MEDIUM, LARGE };
    private final Size size;

    public static class Builder extends Pizza.Builder<Builder> {
        private final Size size;

        public Builder(Size size) {
            this.size = Objects.requireNotNull(size);
        }

        @Override
        public NyPizza build() {
            return new NyPizza(this);
        }

        @Override
        protected Builder self() {
            return this;
        }
    }

    private NyPizza(Builder builder) {
        super(builder);
        size = builder.size;
    }
}
```
각 하위 클래스의 빌더가 정의한 build 메서드는 해당하는 구체 하위 클래스를 반환하도록 선언한다. NyPizza.Builder는 NyPizza를 반환한다. 하위 클래스의 메서드가 상위 클래스의 메서드가 정의한 반환 타입이 아닌, 그 하위 타입을 반환하는 기능을 **공변반환 타이핑(covariant return typing)**이라 한다. 이 기능을 시용하면 클라이언트가 형변환에 신경 쓰지 않고도 빌더를 사용할 수 있다.

```java
NyPizza pizza = new NyPizza.Builder(SMALL).addTopping(ONION).build();
```

***
## 빌더 패턴에 대한 장점과 단점

빌더 패턴을 이용하면 가변인수(varags) 매개변수를 여러개 사용할 수 있다. 각각을 적절한 메서드로 나눠 선언하면 된다. 아니면 메서드를 여러 번 호출하도록 하고 각 호출 때 넘겨진 매개변수들을 하나의 필드로 모을 수도 있다.

또한 빌더 패턴은 상당히 유연하다. 빌더 하나로 여러 객체를 순회하면서 만들 수 있고, 빌더에 넘기는 매개변수에 따라 다른 객체를 만들 수 있다. 객체마다 부여되는 일렬번호와 같은 특정 필드도 빌더가 알아서 채우도록 할 수 있다.

빌더 패턴의 단점은 빌더를 만들어야 한다는 점이다. 생성 비용이 크지는 않지만 성능에 민감한 상황에서는 문제가 될 수 있다. 또한 점증적 생성자 패턴보다는 코드가 장황하기 때문에 매개변수가 4개 이상은 되어야 값어치를 한다고 한다.

***
### 정리

> 생성자나 정적 팩토리가 처리해야 할 매개변수가 많다면 빌더 패턴을 선택하는 게 더 낫다. 매개변수 중 다수가 필수가 아니거나 같은 타입이면 더더욱 그렇다. 빌더는 정층적 생성자보다 클라이언트 코드를 읽고 쓰기가 훨씬 간결하고, 자바빈즈보다 훨씬 안전하다.