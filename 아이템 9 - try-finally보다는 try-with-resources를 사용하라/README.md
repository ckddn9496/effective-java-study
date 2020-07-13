# 아이템 9 - try-finally보다는 try-with-resources를 사용하라

자바 라이브러리에는 close 메서드를 호출해 직접 닫아줘야 하는 자원이 많다. 자원 닫기는 클라이언트가 놓치기 쉬워서 예측할 수 없는 성능 문제로 이어지기 쉽다. 또한 안전망으로 사용하는 방법인 finalizer는 믿을만하지 못하다.

## try-finally
전통적으로 자원이 제대로 닫힘을 보장하는 수단으로 `try-finally`가 쓰였다.
```java
static String firstLineOfFile(String path) throws IOException {
    BufferedReader br = new BufferedReader(new FileReader(path));
    try {
        return br.readLine();
    } finally {
        br.close();
    }
}
```
하지만 위의 코드에는 문제점이 존재한다. 예외는 try 블록과 finally 블록 모두에서 발생할 수 있는데, 기기에 물리적인 문제가 생긴다면 readLine 메소드와 close 메서드가 모두 실패하여 예외를 던질것이다. 이런 상황이라면 두 번째 예외가 첫 번째 예외를 완전히 집어삼켜 버린다. 그러면 스택 추적 내역에는 첫 번째 예외에 관환 정보가 남지 않게 되어, 시스템 디버깅을 매우 어렵게 한다.

```java
static void copy(String src, String dst) throws IOException {
    InputStream in = new FileInputStream(src);
    try {
        OutputStream out = new FileOutputStream(dst);
        try {
            byte[] buf = new byte[BUFFER_SIZE];
            int n;
            while ((n = in.read(buf)) >= 0) {
                out.write(buf, 0, n);
            }
        } finally {
            out.close();
        }
    } finally {
        in.close();
    }
}
```
자원을 둘 이상 사용하는 경우이다. 반복되는 try-finally 블럭으로 인해 코드의 가독성이 나빠지며, 예외가 발생할 수 있는 지점은 더 많아진다.

## try-with-resources
try-finally에서 겪은 문제들은 자바 7에서 등장한 `try-with-resources`를 통해 해결할 수 있다. 이 구조를 사용하려면 해당 자원이 `AutoCloseable` 인터페이스를 구현해야한다. AutoCloseable은 단순히 void를 반환하는 close 메서드 하나만 정의된 인터페이스이며, 이미 많은 라이브러리들의 수많은 클래스가 AutoCloseable을 구현하거나 확장해뒀다. 닫아야하는 자원을 뜻하는 클래스를 작성하게된다면 AutoCloseable을 구현하는 방법을 추천한다. 이 구문의 사용은 아래와 같다.

```java
static String firstLineOfFile(String path) {
    try (BufferedReader br = new BufferedReader(new FileReader(path))) {
        return br.readLine();
    } catch (IOException e) {
        return defaultVal;
    }
}
```
훨씬 짧고 간결하며 문제를 진단하기도 수월하다. 예외가 발생하더라도 close에서 발생한 예외는 숨겨지고 readLine에서 발생한 예외가 기록된다. 또한, 필요에 따라 숨겨진 예외를 자바 7에 추가된 Throwable의 getSuppressed 메서드를 통해 가져와 확인할 수 있다. 복수의 자원을 사용할 경우도 try 블럭 세미콜론으로 구분하여 자원을 명시해주면 try 블럭을 모두 수행한 후 자동으로 close를 도와준다.

```java
static void copy(String src, String dst) throws IOException {
    try (InputStream in = new FileInputStream(src); OutputStream out = new FileOutputStream(dst)) {
        byte[] buf = new byte[BUFFER_SIZE];
        int n;
        while ((n = in.read(buf)) >= 0) {
            out.write(buf, 0, n);
        }
    }
}
```

***
### 정리
> 꼭 회수해야하는 자원을 다룰 대는 try-finally 보다 try-with-resources를 사용하는 것이 좋다. 코드의 간결성이나 예외에 대한 정보를 얻기에도 유용하다. 대신 사용을 위해서 회수할 자원은 반드시 AutoCloseable 인터페이스를 구현하도록 하자.