# effective-java-study

:book: 이펙티브 자바 3판 스터디를 진행하여 정리한 내용입니다.

## 목차

### 2. 객체 생성과 파괴

- [아이템 1 - 생성자 대신 정적 팩터리 메서드를 고려하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2001%20-%20%EC%83%9D%EC%84%B1%EC%9E%90%20%EB%8C%80%EC%8B%A0%20%EC%A0%95%EC%A0%81%20%ED%8C%A9%ED%84%B0%EB%A6%AC%20%EB%A9%94%EC%84%9C%EB%93%9C%EB%A5%BC%20%EA%B3%A0%EB%A0%A4%ED%95%98%EB%9D%BC.md)
- [아이템 2 - 생성자에 매개변수가 많다면 빌더를 고려하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2002%20-%20%EC%83%9D%EC%84%B1%EC%9E%90%EC%97%90%20%EB%A7%A4%EA%B0%9C%EB%B3%80%EC%88%98%EA%B0%80%20%EB%A7%8E%EB%8B%A4%EB%A9%B4%20%EB%B9%8C%EB%8D%94%EB%A5%BC%20%EA%B3%A0%EB%A0%A4%ED%95%98%EB%9D%BC.md)
- [아이템 3 - private 생성자나 열거 타입으로 싱글턴임을 보증하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2003%20-%20private%20%EC%83%9D%EC%84%B1%EC%9E%90%EB%82%98%20%EC%97%B4%EA%B1%B0%20%ED%83%80%EC%9E%85%EC%9C%BC%EB%A1%9C%20%EC%8B%B1%EA%B8%80%ED%84%B4%EC%9E%84%EC%9D%84%20%EB%B3%B4%EC%A6%9D%ED%95%98%EB%9D%BC.md)
- [아이템 4 - 인스턴스화를 막으려거든 private 생성자를 사용하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2004%20-%20%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4%ED%99%94%EB%A5%BC%20%EB%A7%89%EC%9C%BC%EB%A0%A4%EA%B1%B0%EB%93%A0%20private%20%EC%83%9D%EC%84%B1%EC%9E%90%EB%A5%BC%20%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC.md)
- [아이템 5 - 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2005%20-%20%EC%9E%90%EC%9B%90%EC%9D%84%20%EC%A7%81%EC%A0%91%20%EB%AA%85%EC%8B%9C%ED%95%98%EC%A7%80%20%EB%A7%90%EA%B3%A0%20%EC%9D%98%EC%A1%B4%20%EA%B0%9D%EC%B2%B4%20%EC%A3%BC%EC%9E%85%EC%9D%84%20%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC.md)
- [아이템 6 - 불필요한 객체 생성을 피하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2006%20-%20%EB%B6%88%ED%95%84%EC%9A%94%ED%95%9C%20%EA%B0%9D%EC%B2%B4%20%EC%83%9D%EC%84%B1%EC%9D%84%20%ED%94%BC%ED%95%98%EB%9D%BC.md)
- [아이템 7 - 다 쓴 객체 참조를 해제하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2007%20-%20%EB%8B%A4%20%EC%93%B4%20%EA%B0%9D%EC%B2%B4%20%EC%B0%B8%EC%A1%B0%EB%A5%BC%20%ED%95%B4%EC%A0%9C%ED%95%98%EB%9D%BC.md)
- [아이템 8 - finalizer와 cleaner의 사용을 피하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2008%20%20-%20finalizer%EC%99%80%20cleaner%EC%9D%98%20%EC%82%AC%EC%9A%A9%EC%9D%84%20%ED%94%BC%ED%95%98%EB%9D%BC.md)
- [아이템 9 - try-finally보다는 try-with-resources를 사용하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2009%20-%20try-finally%EB%B3%B4%EB%8B%A4%EB%8A%94%20try-with-resources%EB%A5%BC%20%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC.md)

### 3. 모든 객체의 공통 메서드

- [아이템 10 - equals는 일반 규약을 지켜 재정의하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2010%20-%20equals%EB%8A%94%20%EC%9D%BC%EB%B0%98%20%EA%B7%9C%EC%95%BD%EC%9D%84%20%EC%A7%80%EC%BC%9C%20%EC%9E%AC%EC%A0%95%EC%9D%98%ED%95%98%EB%9D%BC.md)
- [아이템 11 - equals를 재정의하려거든 hashCode도 재정의하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2011%20-%20equals%EB%A5%BC%20%EC%9E%AC%EC%A0%95%EC%9D%98%ED%95%98%EB%A0%A4%EA%B1%B0%EB%93%A0%20hashCode%EB%8F%84%20%EC%9E%AC%EC%A0%95%EC%9D%98%ED%95%98%EB%9D%BC.md)
- [아이템 12 - toString을 항상 재정의하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2012%20-%20toString%EC%9D%84%20%ED%95%AD%EC%83%81%20%EC%9E%AC%EC%A0%95%EC%9D%98%ED%95%98%EB%9D%BC.md)
- [아이템 13 - clone 재정의는 주의해서 진행하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2013%20-%20clone%20%EC%9E%AC%EC%A0%95%EC%9D%98%EB%8A%94%20%EC%A3%BC%EC%9D%98%ED%95%B4%EC%84%9C%20%EC%A7%84%ED%96%89%ED%95%98%EB%9D%BC.md)
- [아이템 14 - Comparable을 구현할지 고려하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2014%20-%20Comparable%EC%9D%84%20%EA%B5%AC%ED%98%84%ED%95%A0%EC%A7%80%20%EA%B3%A0%EB%A0%A4%ED%95%98%EB%9D%BC.md)

### 4. 클래스와 인터페이스
- [아이템 15 - 클래스와 멤버의 접근 권한을 최소화하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2015%20-%20%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80%20%EB%A9%A4%EB%B2%84%EC%9D%98%20%EC%A0%91%EA%B7%BC%20%EA%B6%8C%ED%95%9C%EC%9D%84%20%EC%B5%9C%EC%86%8C%ED%99%94%ED%95%98%EB%9D%BC.md)
- [아이템 16 - public 클래스에서는 public 필드가 아닌 접근자 메서드를 사용하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2016%20-%20public%20%ED%81%B4%EB%9E%98%EC%8A%A4%EC%97%90%EC%84%9C%EB%8A%94%20public%20%ED%95%84%EB%93%9C%EA%B0%80%20%EC%95%84%EB%8B%8C%20%EC%A0%91%EA%B7%BC%EC%9E%90%20%EB%A9%94%EC%84%9C%EB%93%9C%EB%A5%BC%20%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC.md)
- [아이템 17 - 변경 가능성을 최소화하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2017%20-%20%EB%B3%80%EA%B2%BD%20%EA%B0%80%EB%8A%A5%EC%84%B1%EC%9D%84%20%EC%B5%9C%EC%86%8C%ED%99%94%ED%95%98%EB%9D%BC.md)
- [아이템 18 - 상속보다는 컴포지션을 사용하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2018%20-%20%EC%83%81%EC%86%8D%EB%B3%B4%EB%8B%A4%EB%8A%94%20%EC%BB%B4%ED%8F%AC%EC%A7%80%EC%85%98%EC%9D%84%20%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC.md)
- [아이템 20 - 추상 클래스보다는 인터페이스를 우선하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2020%20-%20%EC%B6%94%EC%83%81%20%ED%81%B4%EB%9E%98%EC%8A%A4%EB%B3%B4%EB%8B%A4%EB%8A%94%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%EB%A5%BC%20%EC%9A%B0%EC%84%A0%ED%95%98%EB%9D%BC.md)
- [아이템 21 - 인터페이스는 구현하는 쪽을 생각해 설계하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2021%20-%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%EB%8A%94%20%EA%B5%AC%ED%98%84%ED%95%98%EB%8A%94%20%EC%AA%BD%EC%9D%84%20%EC%83%9D%EA%B0%81%ED%95%B4%20%EC%84%A4%EA%B3%84%ED%95%98%EB%9D%BC.md)
- [아이템 22 - 인터페이스는 타입을 정의하는 용도로만 사용하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2022%20-%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%EB%8A%94%20%ED%83%80%EC%9E%85%EC%9D%84%20%EC%A0%95%EC%9D%98%ED%95%98%EB%8A%94%20%EC%9A%A9%EB%8F%84%EB%A1%9C%EB%A7%8C%20%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC.md)
- [아이템 23 - 태그 달린 클래스보다는 클래스 계층구조를 활용하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2023%20-%20%ED%83%9C%EA%B7%B8%20%EB%8B%AC%EB%A6%B0%20%ED%81%B4%EB%9E%98%EC%8A%A4%EB%B3%B4%EB%8B%A4%EB%8A%94%20%ED%81%B4%EB%9E%98%EC%8A%A4%20%EA%B3%84%EC%B8%B5%EA%B5%AC%EC%A1%B0%EB%A5%BC%20%ED%99%9C%EC%9A%A9%ED%95%98%EB%9D%BC.md)
- [아이템 24 - 멤버 클래스는 되도록 static으로 만들라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2024%20-%20%EB%A9%A4%EB%B2%84%20%ED%81%B4%EB%9E%98%EC%8A%A4%EB%8A%94%20%EB%90%98%EB%8F%84%EB%A1%9D%20static%EC%9C%BC%EB%A1%9C%20%EB%A7%8C%EB%93%A4%EB%9D%BC.md)
- [아이템 25 - 톱레벨 클래스는 한 파일에 하나만 담으라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2025%20-%20%ED%86%B1%EB%A0%88%EB%B2%A8%20%ED%81%B4%EB%9E%98%EC%8A%A4%EB%8A%94%20%ED%95%9C%20%ED%8C%8C%EC%9D%BC%EC%97%90%20%ED%95%98%EB%82%98%EB%A7%8C%20%EB%8B%B4%EC%9C%BC%EB%9D%BC.md)

### 5. 제네릭
- [아이템 26 - 로 타입은 사용하지 말라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2026%20-%20%EB%A1%9C%20%ED%83%80%EC%9E%85%EC%9D%80%20%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80%20%EB%A7%90%EB%9D%BC.md)
- [아이템 27 - 비검사 경고를 제거하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2027%20-%20%EB%B9%84%EA%B2%80%EC%82%AC%20%EA%B2%BD%EA%B3%A0%EB%A5%BC%20%EC%A0%9C%EA%B1%B0%ED%95%98%EB%9D%BC.md)
- [아이템 28 - 배열보다는 리스트를 사용하라](https://github.com/ckddn9496/effective-java-study/blob/master/contents/%EC%95%84%EC%9D%B4%ED%85%9C%2028%20-%20%EB%B0%B0%EC%97%B4%EB%B3%B4%EB%8B%A4%EB%8A%94%20%EB%A6%AC%EC%8A%A4%ED%8A%B8%EB%A5%BC%20%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC.md)
- [5장 제네릭 정리](https://github.com/ckddn9496/effective-java-study/blob/master/contents/5%EC%9E%A5%20-%20%EC%A0%9C%EB%84%A4%EB%A6%AD.md)

### 6. 열거 타입과 애너테이션
- [6장 열거 타입과 애너테이션 정리](https://github.com/ckddn9496/effective-java-study/blob/master/contents/6%EC%9E%A5%20-%20%EC%97%B4%EA%B1%B0%20%ED%83%80%EC%9E%85%EA%B3%BC%20%EC%95%A0%EB%84%88%ED%85%8C%EC%9D%B4%EC%85%98.md)

### 7. 람다와 스트림
- [7장 람다와 스트림 정리](https://github.com/ckddn9496/effective-java-study/blob/master/contents/7%EC%9E%A5%20-%20%EB%9E%8C%EB%8B%A4%EC%99%80%20%EC%8A%A4%ED%8A%B8%EB%A6%BC.md)

### 8장 - 메서드
- [8장 메서드 정리](https://github.com/ckddn9496/effective-java-study/blob/master/contents/8%EC%9E%A5%20-%20%EB%A9%94%EC%84%9C%EB%93%9C.md)

### 9장 - 일반적인 프로그래밍 원칙
- [9장 일반적인 프로그래밍 원칙 정리](https://github.com/ckddn9496/effective-java-study/blob/master/contents/9%EC%9E%A5%20-%20%EC%9D%BC%EB%B0%98%EC%A0%81%EC%9D%B8%20%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%20%EC%9B%90%EC%B9%99.md)

### 10장 - 예외
- [10장 예외 정리](https://github.com/ckddn9496/effective-java-study/blob/master/contents/10%EC%9E%A5%20-%20%EC%98%88%EC%99%B8.md)

### 11장 - 동시성
- [11장 동시성 정리](https://github.com/ckddn9496/effective-java-study/blob/master/contents/11%EC%9E%A5%20-%20%EB%8F%99%EC%8B%9C%EC%84%B1.md)

### 12장 - 직렬화
- [12장 직렬화 정리](https://github.com/ckddn9496/effective-java-study/blob/master/contents/12%EC%9E%A5%20-%20%EC%A7%81%EB%A0%AC%ED%99%94.md)