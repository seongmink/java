# IO 기반 입출력

## IO 패키지 소개

자바에서는 데이터를 스트림(Stream)을 통해 입출력된다. 스트림은 단일 방향으로 연속적으로 흘러가는 것을 말하는데, 물이 높은 곳에서 낮은 곳으로 흐르듯이 데이터는 출발지에서 나와 도착지로 들어간다는 개념이다.

## 입력 스트림과 출력 스트림

프로그램이 데이터를 입력받을 때는 입력 스트림(InputStream)이라고 부르고, 프로그램이 데이터를 보낼 때에는 출력 스트림(OutputStream)이라고 부른다. 입력 스트림의 출발지는 키보드, 파일, 네트워크상의 프로그램이 될 수 있고, 출력 스트림의 도착지는 모니터, 파일, 네트워크상의 프로그램이 될 수 있다.

항상 프로그램을 기준으로 데이터가 들어오면 입력 스트림이고, 데이터가 나가면 출력 스트림이라는 것을 명심해야 한다. 프로그램이 네트워크상의 다른 프로그램과 데이터 교환을 하기 위해서는 양쪽 모두 입력 스트림과 출력 스트림이 따로 필요하다. 스트림의 특성이 단방향이므로 하나의 스트림으로 입력과 출력을 모두 할 수 없기 때문이다.

자바의 기본적인 데이터 입출력(IO: Input/Output) API는 java.io 패키지에서 제공하고 있다.

| java.io 패키지의 주요 클래스                                 | 설명                                                  |
| ------------------------------------------------------------ | ----------------------------------------------------- |
| File                                                         | 파일 시스템의 파일 정보를 얻기 위한 클래스            |
| Console                                                      | 콘솔로부터 문자를 입출력하기 위한 클래스              |
| InputStream / OutputStream                                   | 바이트 단위 입출력을 위한 최상위 입출력 스트림 클래스 |
| FileInputStream / FileOutputStream<br />DataInputStream / DataOutputStream<br />ObjectInputStream / ObjectOutputStream<br />PrintStream<br />BufferedInputStream / BufferedOutputStream | 바이트 단위 입출력을 위한 하위 스트림 클래스          |
| Reader / Writer                                              | 문자 단위 입출력을 위한 최상위 입출력 클래스          |
| FileReader / FileWriter<br />InputStreamReader / OutputStreamWriter<br />PrintWriter<br />BufferedReader / BufferedWriter | 문자 단위 입출력을 위한 하위 스트림 클래스            |

스트림 클래스는 크게 두 종류로 구분된다. 하나는 바이트(byte) 기반 스트림이고, 다른 하나는 문자(character) 기반 스트림이다. **바이트 기반 스트림은 그림, 멀티미디어, 문자 등 모든 종류의 데이터를 받고 보낼 수 있으나, 문자 기반 스트림은 오로지 문자만 받고 보낼 수 있도록 특화되어 있다.** 

| 구분          | 바이트 기반 스트림<br />(입력 스트림) | 바이트 기반 스트림<br />(출력 스트림) | 문자 기반 스트림<br />(입력 스트림) | 문자 기반 스트림<br />(출력 스트림) |
| ------------- | ------------------------------------- | ------------------------------------- | ----------------------------------- | ----------------------------------- |
| 최상위 클래스 | InputStream                           | OutputStream                          | Reader                              | Writer                              |
| 하위 클래스   | XXXInputStream                        | XXXOutputStream                       | XXXReader                           | XXXWriter                           |

InputStream은 바이트 기반 입력 스트림의 최상위 클래스이고, OutputStream은 바이트 기반 출력 스트림의 최상위 클래스이다. 이 클래스들을 각각 상속받는 하위 클래스는 접미사로 InputStream, OutputStream이 붙는다. Reader는 문자 기반 입력 스트림의 최상위 클래스이고, Writer는 문자 기반 출력 스트림의 최상위 클래스이다. 이 클래스들을 각각 상속받는 하위 클래스는 접미사로 Reader 또는 Writer가 붙는다.

예를 들어 그림, 멀티미디어, 텍스트 등의 파일을 바이트 단위로 읽어들일 때에는 FileInputStream을 사용하고, 바이트 단위로 저장할 때에는 FileOutputStream을 사용한다. 텍스트 파일의 경우, 문자 단위로 읽어들일 때에는 FileReader를 사용하고, 문자 단위로 저장할 때에는 FileWriter를 사용한다.

- ### InputStream

  InputStream은 **바이트 기반 입력 스트림**의 최상위 클래스로 추상 클래스이다. 모든 바이트 기반 입력 스트림은 이 클래스를 상속받아서 만들어진다. FileInputStream, BufferedInputStream, DataInputStream 클래스는 모두 InputStream 클래스를 상속하고 있다.

  InputStream 클래스에는 바이트 기반 입력 슽릠이 기본적으로 가져야 할 메소드가 정의되어 있다.

  | 리턴 타입 | 메소드                           | 설명                                                         |
  | --------- | -------------------------------- | ------------------------------------------------------------ |
  | int       | read()                           | 입력 스트림으로부터 1바이트를 읽고 읽은 바이트를 리턴한다.   |
  | int       | read(byte[] b)                   | 입력 스트림으로부터 읽은 바이트들을 매개값으로 주어진 바이트 배열 b에 저장하고 실제로 읽은 바이트 수를 리턴한다. |
  | int       | read(byte[] b, int off, int len) | 입력 스트림으로부터 len개의 바이트를 읽고 매개값으로 주어진 바이트 배열 b[off] 부터 len개까지 저장한다.<br />그리고 실제로 읽은 바이트수인 len개를 리턴한다. 만약 len개를 모두 읽지 못하면 실제로 읽은 바이트 수를 리턴한다. |
  | void      | close()                          | 사용한 시스템 자원을 반납하고 입력 스트림을 닫는다.          |

  - #### read() 메소드

    read() 메소드는 입력 스트림으로부터 1바이트를 읽고 4바이트 int 타입으로 리턴한다. 따라서 리턴된 4바이트 중 끝의 1바이트에만 데이터가 들어 있다.

    더 이상 입력 스트림으로부터 바이트를 읽을 수 없다면 read() 메소드는 -1을 리턴하는데, 이것을 이용하면 읽을 수 있는 마지막 바이트까지 루프를 돌며 한 바이트씩 읽을 수 있다.

    ```java
    InputStream is = new FileInputStream("C:/test.jpg");
    int readByte;
    while((readByte=is.read() != -1) { ... }
    ```

  - #### read(byte[] b) 메소드

    read(byte[] b) 메소드는 입력 스트림으로부터 매개값으로 주어진 바이트 배열의 길이만큼 바이트를 읽고 배열에 저장한다. 그리고 읽은 바이트 수를 리턴한다. 실제로 읽은 바이트 수가 배열의 길이보다 작을 경우 읽은 수만큼 리턴한다.

    read(byte[] b) 역시 입력 스트림으로부터 바이트를 더 이상 읽을 수 없다면 -1을 리턴하는데, 이 것을 이용하면 읽을 수 있는 마지막 바이트까지 루프를 돌며 읽을 수 있다.

    ```java
    InputStream is = new FileInputStream("C:/test.jpg");
    int readByteNo;
    byte[] readBytes = new byte[100];
    while((readByteNo=is.read(readBytes) != -1) { ... }
    ```

    read(byte[] b) 메소드는 한 번 읽을 때 매개값으로 주어진 바이트 배열 길이만큼 읽기 때문에 루핑 횟수가 현저히 줄어든다. 그러므로 **많은 양의 바이트를 읽을 때는 read(byte[] b) 메소드를 사용하는 것이 좋다.**

  - #### read(byte[] b, int off, int len) 메소드

    read(byte[] b, int off, int len) 메소드는 입력 스트림으로부터 len개의 바이트만큼 읽고, 매개값으로 주어진 바이트 배열 b[off]부터 len개까지 저장한다. 그리고 읽은 바이트 수인 len개를 리턴한다. 실제로 읽은 바이트 수가 len개보다 작을 경우 읽은 수만큼 리턴한다.

    read(byte[] b, int off, int len) 역시 입력 스트림으로부터 바이트를 더 이상 읽을 수 없다면 -1을 리턴한다. read(byte[] b)와의 차이점은 한 번에 읽어들이는 바이트 수를 len 매개값으로 조절할 수 있고, 배열에서 저장이 시작되는 인덱스를 지정할 수 있다는 점이다. 만약 off를 0으로, len을 배열의 길이로 준다면 read(byte[] b)와 동일하다.

    ```java
    InputStream is = new FileInputStream("C:/test.jpg");
    byte[] readBytes = new byte[100];
    int readByteNo = is.read(readBytes, 0, 100); // 0번째부터 100개 읽기
    ```

  - #### close() 메소드

    InputStream을 더 이상 사용하지 않을 경우에는 close() 메소드를 호출해서 InputStream에서 사용했던 시스템 자원을 풀어준다.

    ```java
    is.close();
    ```

- ### OutputStream

   OutputStream은 **바이트 기반 출력 스트림**의 최상위 클래스로 추상 클래스이다. 모든 바이트 기반 출력 스트림 클래스는 이 클래스를 상속받아서 만들어진다. FileOutputStream, PrintStream, BufferedOutputStream, DataOutputStream 클래스는 모두 OutputStream 클래스를 상속하고 있다.

  OutputStream 클래스에는 모든 바이트 기반 출력 스트림이 기본적으로 가져야 할 메소드가 정의되어 있다.

  | 리턴 타입 | 메소드                            | 설명                                                         |
  | --------- | --------------------------------- | ------------------------------------------------------------ |
  | void      | write(int b)                      | 출력 스트림으로 1바이트를 보낸다(b의 끝 1바이트)             |
  | void      | write(byte[] b)                   | 출력 스트림으로 주어진 바이트 배열 b의 모든 바이트를 보낸다. |
  | void      | write(byte[] b, int off, int len) | 출력 스트림으로 주어진 바이트 배열 b[off]부터 len개까지의 바이트를 보낸다. |
  | void      | flush()                           | 버퍼에 잔류하는 모든 바이트를 출력한다.                      |
  | void      | close()                           | 사용한 시스템 자원을 반납하고 출력 스트림을 닫는다.          |

  - #### write(int b) 메소드

    write(int b) 메소드는 매개 변수로 주어진 **int 값에서 끝에 있는 1바이트만** 출력 스트림으로 보낸다. 매개 변수가 int 타입이므로 4바이트 모두를 보내는 것으로 오해할 수 있다.

    ```java
    OutputStream os = new FileOutputStream("C:/test.txt");
    byte[] data = "ABC".getBytes();
    for(int i=0; i<data.length; i++) {
        os.write(data[i]); // "A", "B", "C"를 하나씩 출력
    }
    ```

  - #### write(byte[] b) 메소드

    write(byte[] b)는 매개값으로 주어진 바이트 배열의 **모든 바이트**를 출력 스트림으로 보낸다.

    ```java
    OutputStream os = new FileOutputStream("C:/test.txt");
    byte[] data = "ABC".getBytes();
    os.write(data); // "ABC" 모두 출력
    ```

  - #### write(byte[] b, int off, int len) 메소드

    write(byte[] b, int off, int len)은 b[off]부터 len개의 바이트를 출력 스트림으로 보낸다.

    ```java
    OutputStream os = new FileOutputStream("C:/test.txt");
    byte[] data = "ABC".getBytes();
    os.write(data, 1, 2); // "BC"만 출력
    ```

  - #### flush()와 close() 메소드

    출력 스트림은 내부에 작은 버퍼(buffer)가 있어서 데이터가 출력되기 전에 버퍼에 쌓여있다가 순서대로 출력된다. flush() 메소드는 버퍼에 잔류하고 있는 데이터를 모두 출력시키고 버퍼를 비우는 역할을 한다. 프로그램에서 더 이상 출력할 데이터가 없다면 flush() 메소드를 마지막으로 호출하여 버퍼에 잔류하는 모든 데이터가 출력되도록 해야 한다. OutputStream을 더 이상 사용하지 않을 경우에는 close() 메소드를 호출해서 OutputStream에서 사용했던 시스템 자원을 풀어준다.

    ```java
    OutputStream os = new FileOutputStream("C:/test.txt");
    byte[] data = "ABC".getBytes();
    os.write(data);
    os.flush();
    os.close();
    ```

- ### Reader

  Reader는 **문자 기반 입력 스트림**의 최상위 클래스로 추상 클래스이다. 모든 문자 기반 입력 스트림은 이 클래스를 상속받아서 만들어진다. FileReader, BufferedReader, InputStreamReader 클래스는 모두 Reader 클래스를 상속하고 있다.

  Reader 클래스에는 문자 기반 입력 스트림이 기본적으로 가져야 할 메소드가 정의되어 있다.

  | 리턴 타입 | 메소드                              | 설명                                                         |
  | --------- | ----------------------------------- | ------------------------------------------------------------ |
  | int       | read()                              | 입력 스트림으로부터 한 개의 문자를 읽고 리턴한다.            |
  | int       | read(char[] cbuf)                   | 입력 스트림으로부터 읽은 문자들을 매개값으로 주어진 문자 배열 cbuf에 저장하고 실제로 읽은 문자 수를 리턴한다. |
  | int       | read(char[] cbuf, int off, int len) | 입력 스트림으로부터 len개의 문자를 읽고 매개값으로 주어진 문자 배열 cbuf[off]부터 len개까지 저장한다.<br />그리고 실제로 읽은 문자 수인 len개를 리턴한다. |
  | void      | close()                             | 사용한 시스템 자원을 반납하고 입력 스트림을 닫는다.          |

  - #### read() 메소드

    read() 메소드는 입력 스트림으로부터 한 개의 문자(2바이트)를 읽고 4바이트 int 타입으로 리턴한다. 따라서 **리턴된 4바이트 중 끝에 있는 2바이트에 문자 데이터가 들어있다.**

    read() 메소드가 리턴한 int 값을 char 타입으로 변환하면 읽은 문자를 얻을 수 있다.

    ```java
    char charData = (char) read();
    ```

    더 이상 입력 스트림으로부터 문자를 읽을 수 없다면 read() 메소드는 -1을 리턴하는데 이것을 이용하면 읽을 수 있는 마지막 문자까지 루프를 돌며 한 문자씩 읽을 수 있다.

    ```java
    Reader reader = new FileReader("C:/test.txt");
    int readData;
    while((readData=reader.read()) != -1) {
        char charData = (char) readData;
    }
    ```

  - #### read(char[] cbuf) 메소드

    read(char[] cbuf) 메소드는 입력 스트림으로부터 매개값으로 주어진 문자 배열의 길이만큼 문자를 읽고 배열에 저장한다. 그리고 읽은 문자 수를 리턴한다. 실제로 읽은 문자 수가 배열의 길이보다 작을 경우 읽은 수만큼만 리턴한다.

    ```java
    Reader reader = new FileReader("C:/test.txt");
    int readData;
    char[] cbuf = new char[2];
    while((readData=reader.read(cbuf)) != -1) { ... }
    ```

    read(char[] cbuf) 메소드는 한 번 읽을 때 주어진 배열 길이만큼 읽기 때문에 루핑 횟수가 현저히 줄어든다. 그러므로 **많은 양의 문자를 읽을 때는 read(char[] cbuf) 메소드를 사용하는 것이 좋다.**

  - #### read(char[] cbuf, int off, int len) 메소드

    read(char[] cbuf, int off, int len) 메소드는 입력 스트림으로부터 len개의 문자만큼 읽고 매개값으로 주어진 문자 배열 cbuf[off]부터 len개까지 저장한다. 그리고 읽은 문자 수인 len개를 리턴한다. 실제로 읽은 문자 수가 len개보다 작을 경우 읽은 수만큼 리턴한다.

    read(char[] cbuf, int off, int len) 역시 입력 스트림으로부터 문자를 더 이상 읽을 수 없다면 -1을 리턴한다. read(char[] cbuf)와의 차이점은 한 번에 읽어들이는 문자 수를 len 매개값으로 조절할 수 있고, 배열에서 저장이 시작되는 인덱스를 지정할 수 있다는 점이다. 만약 off를 0으로, len을 배열의 길이로 준다면 read(char[] cbuf)와 동일하다.

    ```java
    Reader reader = new FileReader("C:/test.txt");
    char[] cbuf = new char[100];
    int readCharNo = reader.read(cbuf, 0, 100);
    ```

  - #### close() 메소드

    마지막으로 Reader를 더 이상 사용하지 않을 경우에는 close() 메소드를 호출해서 Reader에서 사용했던 시스템 자원을 풀어준다.

    ```java
    reader.close();
    ```

- ### Writer

  Writer는 **문자 기반 출력 스트림**의 최상위 클래스로 추상 클래스이다. 모든 문자 기반 출력스트림 클래스는 이 클래스를 상속받아서 만들어진다. FileWriter, BufferedWriter, PrintWriter, OutputStreamWriter 클래스는 모두 Writer 클래스를 상속하고 있다.

  | 리턴 타입 | 메소드                               | 설명                                                         |
  | --------- | ------------------------------------ | ------------------------------------------------------------ |
  | void      | write(int c)                         | 출력 스트림으로 주어진 한 문자를 보낸다(c의 끝 2바이트)      |
  | void      | write(char[] cbuf)                   | 출력 스트림으로 주어진 문자 배열 cbuf의 모든 문자를 보낸다.  |
  | void      | write(char[] cbuf, int off, int len) | 출력 스트림으로 주어진 문자 배열 cbuf[off]부터 len개까지의 문자를 보낸다. |
  | void      | write(String str)                    | 출력 스트림으로 주어진 문자열을 전부 보낸다.                 |
  | void      | write(String str, int off, int len)  | 출력 스트림으로 주어진 문자열 off 순번부터 len개까지의 문자를 보낸다. |
  | void      | flush()                              | 버퍼에 잔류하는 모든 문자열을 출력한다.                      |
  | void      | close()                              | 사용한 시스템 자원을 반납하고 출력 스트림을 닫는다.          |
  - #### write(int c) 메소드

    write(int c) 메소드는 매개 변수로 주어진 int 값에서 끝에 있는 2바이트(한 개의 문자)만 출력 스트림으로 보낸다. 매개 변수가 int 타입이므로 4바이트 모두를 보내는 것으로 오해할 수 있다.

    ```java
    Writer writer = new FileWriter("C:/test.txt");
    char[] data = "홍길동".toCharArray();
    for(int i=0; i<data.length; i++) {
        writer.write(data[i]); // "홍", "길", "동"을 하나씩 출력
    }
    ```

  - #### write(char[] cbuf) 메소드

    write(char[] cbuf) 메소드는 매개값으로 주어진 char[] 배열의 모든 문자를 출력 스트림으로 보낸다.

    ```java
    Writer writer = new FileWriter("C:/test.txt");
    char[] data = "홍길동".charArray();
    writer.write(data); // "홍길동" 모두 출력
    ```

  - #### write(char[] c, int off, int len) 메소드

    write(char[] c, int off, int len) 은 c[off]부터 len개의 문자를 출력스트림으로 보낸다.

    ```java
    Writer writer = new FileWriter("C:/test.txt");
    char[] data = "홍길동".charArray();
    writer.write(data, 1, 2); // "길동"만 출력
    ```

  - #### write(String str)와 write(String str, int off, int len) 메소드

    Writer는 문자열을 좀 더 쉽게 보내기 위해서 write(String str)와 write(String str, int off, int len) 메소드를 제공한다. write(String str)은 문자열 전체를 출력 스트림으로 보내고, write(String str, int off, int len) 은 주어진 문자열 off 순번부터 len개까지의 문자를 보낸다.

    - write(String str) : 문자열 전체

    - write(String str, int off, int len) : off 부터 len개(만약 write(str, 3, 2)면 str의 3번 인덱스부터 2개)

    문자 출력 스트림은 내부에 작은 버퍼(buffer)가 있어서 데이터가 출력되기 전에 버퍼에 쌓여있다가 순서대로 출력된다. flush() 메소드는 버퍼에 잔류하고 있는 데이터를 모두 출력시키고 버퍼를 비우는 역할을 한다. 프로그램에서 더 이상 출력할 데이터가 없다면 flush() 메소드를 마지막으로 호출하여 버퍼에 잔류하는 모든 데이터가 출력되도록 해야 한다. Writer를 더 이상 사용하지 않을 경우에는 close() 메소드를 호출해서 Writer에서 사용했던 시스템 자원을 풀어준다.

    ```java
    Writer writer = new FileWriter("C:/test.txt");
    String data = "안녕 자바 프로그램";
    writer.write(data);
    writer.flush();
    writer.close();
    ```

## 콘솔 입출력

콘솔(Console)은 시스템을 사용하기 위해 키보드로 입력을 받고 화면으로 출력하는 소프트웨어를 말한다. 유닉스나 리눅스 운영체제는 터미널(Terminal)에 해당하고, Windows 운영체제는 명령 프롬프트에 해당한다. 자바는 콘솔로부터 데이터를 입력받을 때 System.in을 사용하고, 콘솔에 데이터를 출력할 때 System.out을 사용한다. 그리고 에러를 출력할 때에는 System.err를 사용한다.

- ### System.in 필드

  자바는 프로그램이 콘솔로부터 데이터를 입력받을 수 있도록 System 클래스의 in 정적 필드를 제공하고 있다. System.in은 InputStream 타입의 필드이므로 다음과 같이 InputStream 변수로 참조가 가능하다.

  ```java
  InputStream is = System.in;
  ```

  키보드로부터 어떤 키가 입력되었는지 확인하려면 InputStream의 read() 메소드로 한 바이트를 읽으면 된다. 리턴된 int 값에는 십진수 아스키 코드(ASCII Code)가 들어있다.

  ```java
  int asciiCode = is.read();
  ```

  만약 키보드에서 a를 입력하고 Enter키를 눌렀다면 a키의 97번과 Enter키의 13, 10번이 차례대로 읽혀진다. Enter키는 캐리지 리턴(carriage return:13)과 라인 피드(line feed:10)코드가 결합된 키라고 볼 수 있다. 숫자로된 아스키 코드 대신에 키보드에서 입력한 문자를 직접 얻고 싶다면 read() 메소드로 읽은 아스키 코드를 char로 타입 변환을 하면 된다.

  ```java
  char inputChar = (char) is.read();
  ```

  InputStream의 read() 메소드는 1바이트만 읽기 때문에 1바이트의 아스키 코드로 표현되는 숫자, 영어, 특수문자는 프로그램에서 잘 읽을 수 있지만, 한글과 같이 2바이트를 필요로 하는 유니코드는 read() 메소드로 읽을 수 없다. 키보드로 입력된 한글을 얻기 위해서는 우선 read(byte[] b)나 read(byte[] b, int off, int len) 메소드로 전체 입력된 내용을 바이트 배열로 받고, 이 배열을 이용해서 String 객체를 생성하면 된다. read(byte[] b) 메소드를 사용하기 전에 우선 키보드에서 입력한 문자를 저장할 바이트 배열을 만들어야 한다.

  프로그램에서 바이트 배열에 저장된 아스키 코드를 사용하려면 문자열로 변환해야 한다. 변환할 문자열은 바이트 배열의 0번 인덱스에서 시작해서 읽은 바이트 수-2만큼이다. 2를  빼는 이유는 Enter키에 해당하는 마지막 두 바이트를 제외하기 위해서이다. 바이트 배열을 문자열로 변환할 때에는 다음과 같이 String 클래스의 생성자를 이용한다.

  ```java
  String strData = new String(byteData, 0, readByteNo-2);
                          바이트 배열 시작인덱스 읽은 바이트수-2
  ```

- ### System.out 필드

  콘솔로 데이터를 출력하기 위해서는 System.out 정적 필드를 사용한다. out은 PrintStream 타입의 필드이다.

  ```java
  OutputStream os = System.out;
  ```

  콘솔로 1개의 바이트를 출력하려면 OutputStream의 write(int b) 메소드를 이용하면 된다. 이때 바이트 값은 아스키 코드인데, write() 메소드는 아스키 코드를 문자로 콘솔에 출력한다. 예를 들어 아스키 코드 97번을 write(int b) 메소드로 출력하면 'a'가 출력된다.

  ```java
  byte b = 97;
  os.write(b);
  os.flush();
  ```

  OutputStream의 write(int b) 메소드는 1바이트만 보낼 수 있기 때문에 1바이트로 표현한 숫자, 영어, 특수문자는 출력이 가능하지만, 2바이트로 표현되는 한글은 출력할 수 없다. 한글을 출력하기 위해서는 우선 한글을 바이트 배열로 얻은 다음, write(byte[] b)나 write(byte[] b, int off, int len) 메소드로 콘솔에 출력하면 된다.

  ```java
  String name "홍길동";
  byte[] nameBytes = name.getBytes();
  os.wirte(nameBytes);
  os.flush();
  ```

  out은 원래 PrintStream 타입의 필드이므로 PrintStream의 print() 또는 println() 메소드를 사용하면 좀 더 쉬운 방법으로 다양한 타입의 데이터를 콘솔에 출력할 수 있다.

  ```java
  PrintSteram ps = System.out;
  ps.println(...);
  ```

- ### Console 클래스

  자바 6부터는 콘솔에서 입력받은 문자열을 쉽게 읽을 수 있도록 java.io.Console 클래스를 제공하고 있다. Console 객체를 얻기 위해서는 System의 정적 메소드인 console()을 다음과 같이 호출하면 된다.

  ```java
  Console console = System.console();
  ```

  주의할 점은 이클립스에서 실행하면 System.console() 메소드는 null을 리턴하기 때문에 반드시 명령 프롬프트에서 실행해야 한다.

  | 리턴 타입 | 메소드         | 설명                                                  |
  | --------- | -------------- | ----------------------------------------------------- |
  | String    | readLine()     | Enter키를 입력하기 전의 모든 문자열을 읽음            |
  | char[]    | readPassword() | 키보드 입력 문자를 콘솔에 보여주지 않고 문자열을 읽음 |

  아이디를 키보드로 입력할 때에는 입력 문자가 에코(echo) 출력이 되지만, 패스워든느 입력 문자가 에코 출력이 되지 않기 때문에 보안상 유리하다.

- ### Scanner 클래스

  Console 클래스는 콘솔로부터 문자열은 읽을 수 있지만 기본 타입(정수, 실수) 값을 바로 읽을 수는 없다. java.io 패키지의 클래스는 아니지만, java.util 패키지의 Scanner 클래스를 이용하면 콘솔로부터 기본 타입의 값을 바로 읽을 수 있다. Scanner 객체를 생성하려면 다음과 같이 생성자에 System.in 매개값을 주면 된다.

  ```java
  Scanner scanner = new Scanner(System.in);
  ```

  Scanner가 콘솔에만 사용되는 것은 아니고, 생성자 매개값에는 File, InputStream, Path 등과 같이 다양한 입력 소스를 지정할 수도 있다. Scanner는 기본 타입의 값을 읽기 위해 다음과 같은 메소드를 제공한다.

  | 리턴 타입 | 메소드        | 설명                             |
  | --------- | ------------- | -------------------------------- |
  | boolean   | nextBoolean() | boolean(true/false) 값을 읽는다. |
  | byte      | nextByte()    | byte 값을 읽는다.                |
  | short     | nextShort()   | short 값을 읽는다.               |
  | int       | nextInt()     | int 값을 읽는다.                 |
  | long      | nextLong()    | long 값을 읽는다.                |
  | float     | nextFloat()   | float 값을 읽는다.               |
  | double    | nextDouble()  | double 값을 읽는다.              |
  | String    | nextLine()    | String 값을 읽는다.              |

## 파일 입출력

- ### File 클래스

  IO 패키지(java.io)에서 제공하는 File 클래스는 파일 크기, 파일 속성, 파일 이름 등의 정보를 얻어내는 기능과 파일 생성 및 삭제 기능을 제공하고 있다. 그리고 디렉토리를 생성하고 디렉토리에 존재하는 파일 리스트를 얻어내는 기능도 있다. 그러나 파일의 데이터를 읽고 쓰는 기능은 지원하지 않는다. 파일의 입출력은 스트림을 사용해야 한다.

  ```java
  File file = new File("C:\\temp\\file.txt");
  File file = new File("C:/temp/file.txt");
  ```

  디렉토리 구분자는 운영체제마다 조금씩 다르다. 윈도우에서는 \ 또는 /를 사용할 수 있고, 유닉스나 리눅스에서는 /를 사용한다. File.seperator 상수를 출력해보면 해당 운영체제에서 사용하는 디렉토리 구분자를 확인할 수 있다. 만약 \를 디렉토리 구분자로 사용한다면 이스케이프 문자(\\)로 기술해야 한다.

  File 객체를 생성했다고 해서 파일이나 디렉토리가 생성되는 것은 아니다. 생성자 매개값으로 주어진 경로가 유효하지 않더라도 컴파일 에러나 예외가 발생하지 않는다. File 객체를 통해 해당 경로에서 실제로 파일이나 디렉토리가 있는지 확인하려면 exist() 메소드를 호출할 수 있다. 디렉토리 또는 파일이 파일 시스템에 존재한다면 true를 리턴하고 존재하지 않는다면 false를 리턴한다.

  ```java
  boolean isExist = file.exists();
  ```

  exists() 메소드의 리턴값이 false라면 createNewFile(), mkdir(), mkdirs() 메소드로 파일 또는 디렉토리를 생성할 수 있다.

  | 리턴 타입 | 메소드          | 설명                               |
  | --------- | --------------- | ---------------------------------- |
  | boolean   | createNewFile() | 새로운 파일을 생성                 |
  | boolean   | mkdir()         | 새로운 디렉토리를 생성             |
  | boolean   | mkdirs()        | 경로상에 없는 모든 디렉토리를 생성 |
  | boolean   | delete()        | 파일 또는 디렉토리 삭제            |

  파일 또는 디렉토리가 존재할 경우에는 다음 메소드를 통해 정보를 얻어낼 수 있다.

  | 리턴 타입 | 메소드                           | 설명                                                         |
  | --------- | -------------------------------- | ------------------------------------------------------------ |
  | boolean   | canExecute()                     | 실행할 수 있는 파일인지 여부                                 |
  | boolean   | canRead()                        | 읽을 수 있는 파일인지 여부                                   |
  | boolean   | canWrite()                       | 수정 및 저장할 수 있는 파일인지 여부                         |
  | String    | getName()                        | 파일의 이름을 리턴                                           |
  | String    | getParent()                      | 부모 디렉토리를 리턴                                         |
  | FIle      | getParentFile()                  | 부모 디렉토리를 File 객체로 생성 후 리턴                     |
  | String    | getPath()                        | 전체 경로를 리턴                                             |
  | boolean   | isDirectory()                    | 디렉토리인지 여부                                            |
  | boolean   | isFile()                         | 파일인지 여부                                                |
  | boolean   | isHidden()                       | 숨김 파일인지 여부                                           |
  | long      | lastModified()                   | 마지막 수정 날짜 및 시간을 리턴                              |
  | long      | length()                         | 파일의 크기를 리턴                                           |
  | String[]  | list()                           | 디렉토리에 포함된 파일 및 서브디렉토리 목록 전부를 String 배열로 리턴 |
  | String[]  | list(FilenameFilter filter)      | 디렉토리에 퐇마된 파일 및 서브디렉토리 목록 중에 FIlenameFilter에 맞는 것만 String 배열로 리턴 |
  | File[]    | listFiles()                      | 디렉토리에 포함된 파일 및 서브 디렉토리 목록 전부를 File 배열로 리턴 |
  | File[]    | listFiles(FilenameFilter filter) | 디렉토리에 포함된 파일 및 서브디렉토리 목록 중에  FilenameFilter에 맞는 것만 File 배열로 리턴 |

- ### FileInputStream

  FileInputStream 클래슨느 파일로부터 바이트 단위로 읽어들일 때 사용하는 바이트 기반 입력 스트림이다. 바이트 단위로 읽기 때문에 그림, 오디오, 비디오, 텍스트 파일 등 모든 종류의 파일을 읽을 수 있다.

  ```java
  // 방법 1
  FileInputStream fis = new FileInputStream("C:/Temp/image.gif");
  
  // 방법 2
  File file = new File("C:/Temp/image.gif");
  FileInputStream fis = new FileInputStream(file);
  ```

  첫 번째 방법은 문자열로된 파일 경로를 가지고 FileInputStream을 생성한다. 만약 읽어야 할 파일이 File 객체로 이미 생성되어 있다면 두 번째 방법으로 좀 더 쉽게 FileInputStream을 생성할 수 있다. FileInputStream 객체가 생성될 때 파일과 직접 연결이 되는데, 만약 파일이 존재하지 않는다면 FileNotFoundException을 발생시키므로 try-catch문으로 예외 처리를 해야 한다.

  FileInputStream은 InputStream의 하위 클래스이기 때문에 사용 방법이 InputStream과 동일하다. 한 바이트를 읽기 위해서 read() 메소드를 사용하거나, 읽은 바이트를 byte 배열에 저장하기 위해서 read(bye[] b) 또는 read(byte[] b, int off, int len) 메소드를 사용한다. 전체 파일의 내용을 읽기 위해서는 이 메소들을 반복 실행해서 -1이 나올 때까지 읽으면 된다. 파일의 내용을 모두 읽은 후에는 close() 메소드를 호출해서 파일을 닫아준다.

  ```java
  FileInputStream fis = new FileInputStream("C:/Temp/image.gif");
  int readByteNo;
  byte[] readBytes = new byte[100];
  while((readByteNo = fis.read(readBytes)) != -1 ) {
      // 읽은 바이트 배열 (readBytes)을 처리
  }
  fis.close();
  ```

- ### FileOutputStream

  FileOutputStream은 바이트 단위로 데이터를 파일에 저장할 때 사용하는 바이트 기반 출력 스트림이다. 바이트 단위로 저장하기 때문에 그림, 오디오, 비디오, 텍스트 등 모든 종류의 데이터를 파일로 저장할 수 있다. 

  ```java
  // 방법 1
  FileOutputStream fos = new FileOutputStream("C:/Temp/image.gif");
  
  // 방법 2
  File file = new File("C:/Temp/image.gif");
  FileOutputStream fos = new FileOutputStream(file);
  ```

  첫 번째 방법은 파일의 경로를 가지고 FileOutputStream을 생성하지만, 저장할 파일이 File 객체로 이미 생성되어 있다면 두 번째 방법으로 좀 더 쉽게 FileOutputStream을 생성할 수 있다.

  주의할 점은 파일이 이미 존재할 경우, 데이터를 출력하면 파일을 덮어쓰게 되므로, 기존의 파일 내용은 사라지게 된다. 기존의 파일 내용 끝에 데이터를 추가할 경우에는 FileOutputStream 생성자의 두 번째 매개값을 true로 주면 된다.

  ```java
  FileOutputStream fos = new FileOutputStream("C:/Temp/data.txt", true);
  FileoutputStream fos = new FileOutputStream(file, true);
  ```

  FileOutputStream은 OutputStream의 하위 클래스이기 때문에 사용 방법이 OutputStream과 동일하다. 한 바이트를 저장하기 위해서 write() 메소드를 사용하고 바이트 배열을 한꺼번에 저장하기 위해서 write(byte[] b) 또는 write(byte[] b, int off, int len) 메소드를 사용한다.

  ```java
  FileOutputStream fos = new FileOutputStream("C:/Temp/image.gif");
  byte[] data = ...;
  fos.write(data);
  fos.flush();
  fos.close();
  ```

- ### FileReader

  FileReader 클래스는 텍스트 파일을 프로그램으로 읽어들일 때 사용하는 문자 기반 스트림이다. 문자 단위로 읽기 때문에 텍스트가 아닌 그림, 오디오, 비디오 등의 파일은 읽을 수 없다.

  ```java
  // 방법 1
  FileReader fr = new FileReader("C:/Temp/file.txt");
  
  // 방법 2
  File file = new File("C:/Temp/file.txt");
  FileReader fr = new FileReader(file);
  ```

  FileReader 객체가 생성될 때 파일과 직접 연결이 되는데, 만약 파일이 존재하지 않으면 FileNotFoundException을 발생시키므로 try-catch문으로 예외 처리를 해야 한다. FileReader는 Reader의 하위 클래스이기 때문에 사용 방법이 Reader와 동일하다.

  ```java
  FileReader fr = new FileReader("C:/Temp/file.txt");
  int readCharNo;
  char[] cbuf = new char[100];
  while((readCharNo = fr.read(cubf)) != -1) {
      // 읽은 문자 배열(cbuf)를 처리
  }
  fr.close();
  ```

- ### FileWriter

  FileWriter는 텍스트 데이터를 파일에 저장할 때 사용하는 문자 기반 스트림이다. 문자 단위로 저장하기 때문에 텍스트가 아닌 그림, 오디오, 비디오 등의 데이터를 파일로 저장할 수 없다.

  ```java
  // 방법 1
  FileWriter fw = new FileWriter("C:/Temp/file.txt");
  
  // 방법 2
  File file = new File("C:/Temp/file.txt");
  FileWriter fw = new FileWriter(file);
  ```

  위와 같이 FileWriter를 생성하면 짖어된 파일이 이미 존재할 경우 그 파일을 덮어쓰게 되므로, 기존의 파일 내용은 사라지게 된다. 기존의 파일 내용 끝에 데이터를 추가할 경우에는 FileWriter 생성자에 두 번째 매개값으로 true를 주면 된다.

  ```java
  FileWriter fw = new FileWriter("C:/Temp.file.txt", true);
  FileWriter fw = new FileWriter(file, true);
  ```

  FileWriter는 Writer의 하위 클래스이기 때문에 사용 방법이 Writer와 동일하다. 한 문자를 저장하기 위해서 write(String str) 메소드를 사용한다.

  ```java
  FileWriter fw = new FileWriter("C:/Temp/file.txt");
  String data = "저장할 문자열";
  fw.write(data);
  fw.flush();
  fw.close();
  ```

## 보조 스트림

보조 스트림이란 다른 스트림과 연결되어 여러 가지 편리한 기능을 제공해주는 스트림을 말한다. 보조 스트림을 필터(filter) 스트림이라고도 하는데, 이는 보조 스트림 일부가 FilterInputStream, FilterOutputStream의 하위 클래스이기 때문이다.

보조 스트림은 자체적으로 입출력을 수행할 수 없기 때문에 입력 소스와 바로 연결되는 InputStream, FileInputStream, REader, FileReader, 출력 소스와 바로 연결되는 OutputStream, FileOutputStream, Writer, FileWriter 등에 연결해서 입출력을 수행한다. 보조 스트림은 문자 변환, 입출력 성능 향상, 기본 데이터 타입 입출력, 객체 입출력 등의 기능을 제공한다.

보조 스트림을 생성할 때에는 자신이 연결될 스트림을 다음과 같이 생성자의 매개값으로 받는다.

```java
보조스트림 변수 = new 보조스트림(연결스트림);
```

 예를 들어 콘솔 입력 스트림을 문자 변환 보조 스트림인 InputStreamReader에 연결하는 코드는 다음과 같다.

```java
InputStream is = System.in;
InputStreamReader reader = new InputStreamReader(is);
```

문자 변환 보조 스트림인 InputStreamReader를 다시 성능 향상 보조 스트림인 BufferedReader에 연결하는 코드는 다음과 같다.

```java
// 방법 1
InputStream is = System.in;
InputStreamReader reader = new InputStreamReader(is);
BufferedReader br = new BufferedReader(reader);

// 방법 2
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
```

- ### 문자 변환 보조 스트림

  소스 스트림이 바이트 기반 스트림(InputStream, OutputStream, FileInputStream, FileOutputStream)이면서 입출력 데이터가 문자라면 Reader와 Writer로 변환해서 사용하는 것을 고려해야 한다. 그 이유는 Reader와 Writer는 문자 단위로 입출력하기 때문에 바이트 기반 스트림보다는 편리하고, 문자셋의 종류를 지정할 수 있기 때문에 다양한 문자를 입출력할 수 있다.

  - #### InputStreamReader

    InputStreamReader는 바이트 입력 스트림에 연결되어 문자 입력 스트림인 Reader로 변환시키는 보조 스트림이다.

    ```java
    Reader reader = new InputStreamReader(바이트입력스트림);
    ```

    예를 들어 콘솔 입력을 위한 InputStream을 다음과 같이 Reader 타입으로 변환할 수 있다.

    ```
    InputStream is = System.in;
    Reader reader = new InputStreamReader(is);
    ```

    파일 입력을 위한 FileInputStream도 다음과 같이 Reader 타입으로 변환시킬 수 있다.

    ```java
    FileInputStream fis = new FileInputStream("C:/Temp/file.txt");
    Reader reader = new InputStreamReader(fis);
    ```

  - #### OutputStreamWriter

    OutputStreamWriter는 바이트 출력 스트림에 연결되어 문자 출력 스트림인 Writer로 변환시키는 보조 스트림이다.

    ```java
    Writer writer = new OutputStreamWriter(바이트출력스트림);
    ```

    예를 들어 파일 출력을 위한 FileOutputStream을 다음과 같이 Writer 타입으로 변환할 수 있다.

    ```java
    FileOutputStream fos = new FileOutputStream("C:/Temp/file.txt");
    Writer writer = new OutputStreamWriter(fos);
    ```

- ### 성능 향상 보조 스트림

  **프로그램의 실행 성능은 입출력이 가장 늦은 장치를 따라간다.** CPU와 메모리가 아무리 뛰어나도 하드 디스크의 입출력이 늦어지면 프로그램의 실행 성능은 하드 디스크의 처리 속도에 맞춰진다. 네트워크로 데이터를 전송할 때도 마찬가지이다. 프로그램이 입출력 소스와 직접 작업하지 않고 중간에 메모리 버퍼(buffer)와 작업함으로써 실행 성능을 향상시킬 수 있다. 예를 들어 프로그램은 직접 하드 디스크에 데이터를 보내지 않고 메모리 버퍼에 데이터를 보냄으로써 쓰기 속도가 향상된다. 버퍼는 데이터가 쌓이기를 기다렸다가 꽉 차게 되면 데이터를 한꺼번에 하드 디스크로 보냄으로써 출력 횟수를 줄여준다.

  보조 스트림 중에서는 메모리 버퍼를 제공하여 프로그램의 실행 성능을 향상시키는 것들이 있다. 바이트 기반 스트림에는 BufferedInputStream, BufferedOutputStream이 있고, 문자 기반 스트림에는 BufferedReader, BufferedWriter가 있다.

  - #### BufferedInputStream과 BufferedReader

    BufferedInputStream과 BufferedReader는 입력 소스로부터 자신의 내부 버퍼 크기만큼 데이터를 미리 읽고 버퍼에 저장해 둔다. 프로그램은 외부의 입력 소스로부터 직접 읽는 대신 버퍼로부터 읽음으로써 읽기 성능이 향상된다.

    BufferedInputStream과 BufferedReader 보조 스트림은 다음과 같이 생성자의 매개값으로 준 입력 스트림과 연결되어  8192 내부 버퍼 사이즈를 가지게 된다. BufferedInputStream은 최대 8192 바이트가, BufferedReader는 최대 8192 문자가 저장될 수 있다.

    ```java
    BufferedInputStream bis = new BufferedInputStream(바이트입력스트림);
    BufferedReader br = new BufferedReader(문자입력스트림);
    ```

    BufferedReader는 readLine() 메소드를 추가적으로 더 가지고 있는데, 이 메소드를 이용하면 캐리지리턴(\r) 라인피드(\n)로 구분된 행 단위의 문자열을 한꺼번에 읽을 수 있다. 다음 코드는 Enter키를 입력하기 전까지 콘솔에서 입력한 모든 문자열을 한꺼번에 얻는다.

    ```java
    Reader reader = new InputStreamReader(System.in);
    BufferedReader br = new BufferedReader(reader);
    String inputStr = br.readLine();
    ```

  - #### BufferedOutputStream과 BufferedWriter

    BufferedOutputStream과 BufferedWriter는 프로그램에서 전송한 데이터를 내부 버퍼에 쌓아두었다가 버퍼가 꽉 차면, 버퍼의 모든 데이터를 한꺼번에 보낸다. 프로그램 입장에서 보면 직접 데이터를 보내는 것이 아니라, 메모리 버퍼로 데이터를 고속 전송하기 때문에 실행 성능이 향상되는 효과를 얻게 된다.

    BufferedOutputStream과 BufferedWriter 보조 스트림은 다음과 같이 생성자의 매개값으로 준 출력 스트림과 연결되어 8192 내부 버퍼 사이즈를 가지게 된다. BufferedOutputSteram은 8192 바이트가, BufferedWriter는 8192 문자가 최대 저장될 수 있다.

    ```java
    BufferedOutputStream bos = new BufferedOutputStream(바이트출력스트림);
    BufferedWriter bw = new BufferedWriter(문자출력스트림);
    ```

    데이터를 출력하는 방법은 OutputStream 또는 Writer와 동일하다. 주의할 점은 버퍼가 가득 찼을 때만 출력을 하기 때문에 마지막 데이터 부분이 목적지로 가지 못하고 버퍼에 남아있을 수 있다. 그래서 마지막 출력 작업을 마친 후에는 flush() 메소드를 호출하여 버퍼에서 잔류하고 있는 데이터를 모두 보내도록 해야 한다.

- ### 기본 타입 입출력 보조 스트림

  바이트 스트림은 바이트 단위로 입출력하기 때문에 자바의 기본 데이터 타입인 boolean, char, shor, int, long, float, double 단위로 입출력할 수 없다. 그러나 DataInputStream과 DataOutputStream 보조 스트림을 연결하면 기본 데이터 타입으로 입출력이 가능하다.

  ```java
  DataInputStream dis = new DataInputStream(바이트입력스트림);
  DataoutputStream dos =  new DataOutputStream(바이트출력스트림);
  ```

  | DataInputStream |               | **DataOutputStream** |                         |
  | --------------- | ------------- | -------------------- | ----------------------- |
  | **리턴 타입**   | **메소드**    | **리턴 타입**        | **메소드**              |
  | boolean         | readBoolean() | void                 | writeBoolean(boolean v) |
  | byte            | readByte()    | void                 | writeByte(int v)        |
  | char            | readChar()    | void                 | writeChar(int v)        |
  | double          | readDouble()  | void                 | writeDouble(double v)   |
  | float           | readFloat()   | void                 | writeFloat(float v)     |
  | int             | readInt()     | void                 | writeInt(int v)         |
  | long            | readLong()    | void                 | writeLong(long v)       |
  | short           | readShort()   | void                 | writeShort(int v)       |
  | String          | readUTF()     | void                 | writeUTF(String str)    |

  데이터 타입의 크기가 모두 다르므로 DataOutputStream으로 출력한 데이터를 다시 DataInputStream으로 읽어올 때는 출력한 순서와 동일한 순서로 읽어야 한다. 예를 들어 출력할 때의 순서가 int -> boolean-> double이라면 읽을 때의 순서도 int -> boolean -> double이어야 한다.

- ### 프린터 보조 스트림

  PrintStream과 PrintWriter는 프린터와 유사하게 출력하는 print(), println() 메소드를 가지고 있는 보조 스트림이다. PrintStream은 바이트 출력 스트림과 연결되고, PrintWriter는 문자 출력 스트림과 연결된다.

  ```java
  PrintStream ps = new PrintStream(바이트출력스트림);
  PrintWriter pw = new PrintWriter(문자출력스트림);
  ```

  PrintStream과 PrintWriter는 println()과 print() 이외에 printf()도 제공한다. printf() 메소드는 형식화된 문자열(format string)을 출력할 수 있도록 하기 위해 자바 5부터 추가된 메소드이다.

  | 분류     | 형식화된 문자                                                | 설명                                                         | 출력형태                                                     |
  | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | 정수     | %d<br />%6d<br />%-6d<br />%06d                              | 정수<br />6자리 장수, 왼쪽빈자리 공백<br />6자리 정수, 오른쪽 빈자리 공백<br />6자리 정수, 왼쪽 빈자리 0채움 | 123<br />\_\_\_123<br />123\_\_\_<br />000123                |
  | 실수     | %10.2f<br />%-10.2f<br />%010.2f                             | 소수점 이상 7자리, 소수점 이하 2자리. 왼쪽 빈자리 공백<br />소수점 이상 7자리, 소수점 이하 2자리. 오른쪽 빈자리 공백<br />소수점 이상 7자리, 소수점 이하 2자리. 왼쪽 빈자리 0채움 | \_\_\_\_123.45<br />123.45\_\__\_<br />0000123.45            |
  | 문자열   | %s<br />%6s<br />%-6s                                        | 문자열<br />6자리 문자열, 왼쪽 빈자리 공백<br />6자리 문자열. 오른쪽 빈자리 공백 | abc<br />\_\_\_abc<br />abc\_\_\_                            |
  | 날짜     | %tF<br />%tY<br />%ty<br />%tm<br />%td<br />%tH<br />%tl<br />%tM<br />%tS | %tY-%tm-%td<br />4자리 년<br />2자리 년<br />2자리 월<br />2자리 일<br />2자리 시(0 ~ 23)<br />시(0 ~12)<br />2자리 분<br />2자리 초 | 2010-01-06<br />2010<br />10<br />01<br />06<br />08<br />8<br />06<br />24 |
  | 특수문자 | \t<br />\h<br />%%                                           | 탭(tab)<br />줄바꿈<br />%                                   | <br /><br />%                                                |

- ### 객체 입출력 보조 스트림

  자바는 메모리에 생성된 객체를 파일 또는 네트워크로 출력할 수 있다. 객체는 문자가 아니기 때문에 바이트 기반 스트림으로 출력해야 한다. 객체를 출력하기 위해서는 객체의 데이터(필드값)를 일렬로 늘어선 연속적인 바이트로 변경해야 하는데, 이것을 **객체 직렬화**(serialization)라고 한다. 반대로 파일에 저장되어 있거나 네트워크에서 전송된 객체를 읽을 수도 있는데, 입력 스트림으로부터 읽어들인 연속적인 바이트를 객체로 복원하는 것을 **역직렬화**(deserialization)라고 한다.

  - #### ObjectInputStream, ObjectOutputStream

    자바는 객체를 입출력할 수 있는 두 개의 보조 스트림인 ObjectInputStream과 ObjectOutputStream을 제공한다. ObjectOutputStream은 바이트 출력 스트림과 연결되어 객체를 직렬화 하는 역할을 하고, ObjectInputStream은 바이트 입력 스트림과 연결되어 객체로 역직렬화하는 역할을 한다.

    ObjectInputSteram과 OutputStream은 다른 보조 스틀미과 마찬가지로 연결할 바이트 입출력 스트림을 생성자의 매개값으로 받는다.

    ```java
    ObjectInputStream ois = new ObjectInputStream(바이트입력스트림);
    ObjectOutputStream oos = new ObjectOutputStream(바이트출력스트림);
    ```

    ObjectOutputStream으로 객체를 직렬화하기 위해서는 writeObject() 메소드를 사용한다
    
    ```java
    oos.writeObject(객체);
    ```
    
    반대로 ObjectInputStream의 readObject() 메소드는 입력 스트림에서 읽은 바이트를 역직렬화해서 객체로 생성한다. readObject() 메소드의 리턴타입은 Object 타입이기 때문에 객체 원래의 타입으로 변환해야 한다.
    
    ```java
    객체타입 변수 = (객체타입) ois.readObject();
    ```
    
  - #### 직렬화가 가능한 클래스(Serializable)

    자바는 Serializable 인터페이스를 구현한 클래스만 직렬화할 수 있도록 제한하고 있다. Serializable 인터페이스는 필드나 메소드가 없는 빈 인터페이스이지만, 객체를 직렬화할 때 private 필드를 포함한 모든 필드를 바이트로 변환해도 좋다는 표시 역할을 한다.

    ```java
    public class XXX implements Serializable { }
    ```

    객체를 직렬화하면 바이트로 변환되는 것은 필드들이고, **생성자 및 메소드는 직렬화에 포함되지 않는다.** 따라서 역직렬화할 때에는 필드의 값만 복원된다. 하지만 모든 필드가 직렬화 대상이 되는 것은 아니다. **필드 선언에 static 또는 transient가 붙어 있을 경우에는 직렬화가 되지 않는다.**
    
    ```java
    public class XXX implements Serializable {
        public static int field1;
        transient int field2;
    }
    ```
    
  - #### serialVersionUID 필드

    직렬화로딘 객체를 역직렬화할 때는 직렬화했을 때와 같은 클래스를 사용해야 한다. 클래스의 이름이 같더라도 클래스의 내용이 변경되면, 역직렬화는 실패하며 java.io.InvalidClassException 예외가 발생한다.

    serialVersionUID는 같은 클래스임을 알려주는 식별자 역할을 하는데, Serializable 인터페이스를 구현한 클래스를 컴파일하면 자동적으로 serialVersionUID 정적 필드가 추가된다. 문제는 클래스를 재컴파일하면 seriialVerisonUID의 값이 달라진다는 것이다. 네트워크로 객체를 직렬화하여 전송하는 경우, 보내는 쪽과 받는 쪽이 모두 같은 serialVersionUID를 갖는 클래스를 가지고 있어야 하는데 한 쪽에서 클래스를 변경해서 재컴파일하면 다른 serialVersionUID를 가지게되므로 역직렬화에 실패하게 된다.

    만약 불가피하게 클래스의 수정이 필요하다면 클래스 작성 시 다음과 같이 serialVersionUID를 명시적으로 선언하면 된다.

    ```java
    public calss XXX implements Serializable {
        static final long serialVersionUID = 정수값;
        ...
    }
    ```

    클래스에 serialVersionUID가 명시적으로 선언되어 있으면 컴파일 시에 serialVersionUID 필드가 추가되지 않기 때문에 동일한 serialVersionUID 값을 유지할 수 있다.

  - #### writeObject()와 readObject() 메소드

    부모 클래스가 Serializable 인터페이스를 구현하고 있으면 자식 클래스는 Seralizable 인터페이스를 구현하지 않아도 자식 객체를 직렬화하면 부모 필드 및 자식 필드가 모두 직렬화 된다. 하지만 그 반대로 자식 클래스만 Serializable 인터페이스를 궇녀하고 있따면 자식 객체를 직렬화할 때 부모의 필드는 직렬화에서 제외된다. 이 경우 부모 클래스의 필드를 직렬화하고 싶다면 다음 두 가지 방법 중 하나를 선택해야 한다.

    - 부모 클래스가 Serializable 인터페이스를 구현하도록 한다.
    - 자식 클래스에서 writeObject()와 readObject() 메소드를 선언해서 부모 객체의 필드를 직접 출력시킨다.

    첫 번째 방법이 제일 좋은 방법이 되겠지만, 부모 클래스의 소스를 수정할 수 없는 경우에는 두 번째 방법을 사용해야 한다.

    writeObject() 메소드는 직렬화될 때 자동으로 호출되고, readObject() 메소드는 역직렬화될 때 자동으로 호출된다.

    ```java
    private void writeObject(ObjectOutputStream out) throws IOException {
        out.writeXXX(부모필드); // 부모 객체의 필드값을 출력함
        ...
        out.defaultWriteObject(); // 자식 객체의 필드값을 직렬화
    }
    ```

    ```java
    private void readObject(ObjectInputStream in) throws IOException, ClassNotFoundException {
        부모필드 = in.readXXX(); // 부모 객체의 필드값을 읽어옴
        ...
        in.defaultReadObject(); // 자식 객체의 필드값을 역직렬화
    }
    ```

    두 메소드를 작성할 때 주의할 점은 접근 제한자가 private가 아니면 자동 호출되지 않기 때문에 **반드시 private를 붙여주어야 한다.** writeObject()와 readObject() 메소드의 매개값인 ObjectOutputSteram과 ObjectInputStream은 다양한 종류의 writeXXX(), readXXX() 메소드를 제공하므로 부모 필드 타입에 맞는 것을 선택해서 사용하면 된다. defaultWriteObject()와 defaultReadObject()는 자식 클래스에 정의된 필드들을 모두 직렬화하고 역직렬화 한다.
