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
