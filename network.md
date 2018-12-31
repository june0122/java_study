# Network Programming (Socket Programming)

> process : 프로그램(실행 가능한 파일)의 인스턴스, 실행 중인 프로그램

Windows

Linux -> Protected mode -> 가상 메모리(페이징)

macOS<br><br>

Real mode -> MS DOS<br><br>

프로세스가 사용하는 메모리는 반드시 물리적으로 **연속적**이어야 한다.<br><br>

> 가상 메모리 : 단편화 문제를 해결하기 위해 나온 시스템

> 가상 메모리
- 세그먼트
- 페이징
<br><br>

모든 프로세스는 자신만의 *페이지 테이블(가상 주소)*을 가지고 있다.

-> 서로 다른 프로세스가 데이터를 교환하기 위해서는 IPC가 필요하다.<br><br>

> System V IPC(Inter-Process Communication, 프로세스 간 통신)
  - Message Queue
  - Semaphore (세마포어, 게임 등에서 프로세스를 1개만 띄우도록 제어할 때 사용) <-> mutex(뮤텍스, )
  - Signal
  - Shared Memory

> 프레임(Frame) : 물리 메모리를 일정된 한 크기로 나눈 블록

> 페이지(Page) : 가상 메모리를 일정된 한 크기로 나눈 블록

PFN(Page Frame Number)

일반적으로 페이지는 각각 4k의 크기를 가진다.

> Process - 우선순위(nice)
- Dead Lock, Starving
<br><br>

TCP/IP

'File I/O'
<br><br>
- VFS(Virtual File System)

  open / read / write / close
  
  Virtual -> 컴파일 타임이 아니라 런타임에 동작
  
  다형성은 OCP를 만족한다.
  
### File I/O

C와 자바의 File I/O은 차이가 있다.

> decorate pattern

```java
FileInputStream fis = new FileInputStream("helllo.txt");
DataInputStream dis = new DataInputStream(fis);
```

위와 같은 형태때문에 장점도 있지만 java의 I/O는 속도가 느리다. -> 해결을 위해 Java 7 부터 NIO 지원

-> 비메모리 자원에 대해서는 반드시 명시적인 종료 메소드를 호출해야 한다.

garbage collector가 메모리 자원은 자동적으로 관리해주지만 비메모리 자원은 그렇지 못하다.

### 예외처리의 장점과 단점

- 단점 : 실제 코드가 처리해야하는 일보다 예외처리의 코드가 더 많아지고 가독성이 떨어진다.

해결법 Java 7 -> **Try with Resourses** 문법

```java
import java.io.*;

//아래의 클래스의 객체가 Try with Resource를 통해 자동으로 close 될 수 있도록 하고 싶다.
//-> AutoClosable 인터페이스를 구현하면 된다.
class MyResource implements AutoClosealbe{
// File
@Override
public void close(){
  System.out.println("MyResource closed");
}
}
```
<br><br>
### TCP/IP

> ISO, OSI 7 Layer -> 각각의 부분을 독립적으로 업데이트할 수 있다. 상호독립적 업데이트 (OCP 다형성 만족)

> BSD, TCP/IP (가장 많이 사용 하지만 점점 쇠퇴하고 있다.)

OSI는 많은 헤더가 붙지만 TCP/IP는 헤더가 적게 붙는다.

TCP/IP - IPC
  
  다른 컴퓨터의 프로세스와 통신할 수 있다.
<br><br>

주소

- 컴퓨터의 주소 - IP Address
- 프로세스의 주소 - Port Adress

TCP 통신
  
  -> Connection 수립
  
www.naver.com (Domain Name) -> DNS(Domain Name Server) -> IP 주소로 변환

서비스의 종류에 따라서 약속된 PORT 번호가 있다.

-> Well-Known Port Address

  - HTTP Server : 80
  - HTTPS : 443
  - SSH : 22
  - FTP : 21
  - Redis : 6379

<br><br>
> Server를 만드는 과정

1. Socket

  Server Socket : 연결을 수립하기 위한 Socket
  
    - socket, bind, listen, accept
    
  Socket : 데이터를 교환하기 위한 Socket
  
    - read / write

* * *

# ECHO SERVER

```java
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

//Echo Server: 메아리 서버
public class Program {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(5000);
        
        // 연결을 수립한다.
        Socket socket = serverSocket.accept();
        InputStream is = socket.getInputStream();
        OutputStream os = socket.getOutputStream();
        
        while (true) {
            byte[] buf = new byte[512];
            int len = is.read(buf);
            if (len == -1){
                break;
            }
            System.out.println("from client: " + new String(buf, 0, len));
            
            os.write(buf, 0, len);
        }
    }
}
```

# ECHO CLIENT

```java
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.InetSocketAddress;
import java.net.Socket;
import java.util.Scanner;

public class Program {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket();
        socket.connect(new InetSocketAddress("127.0.0.1", 5000));

        InputStream is = socket.getInputStream();
        OutputStream os = socket.getOutputStream();

        Scanner scanner = new Scanner(System.in);

        while (true) {
            String line = scanner.next();
            os.write(line.getBytes());
            byte[] buf = new byte[512];
            int len = is.read(buf);

            if (len == -1){
                break;
            }

            System.out.println("from server: " + new String(buf, 0, len));

            os.write(buf, 0, len);
        }
    }
}
```
