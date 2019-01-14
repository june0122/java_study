> ECHO SERVER

```java
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

// Concurrent: 병행
//  1) Process - Unix(Linux)
//  2) Thread  - Windows, Java

// Parallel:   병렬(하드웨어)
// Concurrent Programming
//  => 하나의 프로그램에서 동시에 여러개의 코드를 수행할 수 있도록 만들어야 한다.

// Thread per Connection Model Server
//   Accept
//     => Socket => new Thread() => Echo

// 문제점?
//  => 연결이 적을 경우, 매우 잘 동작하나, 연결이 매우 많아지면, 점점 성능이 떨어진다.
//  => Multiplexing Model을 사용하는 것이 좋다.
//     Linux: poll, epoll
//     BSD:   kqueue
//     Windows: IOCP
//     Java:    NIO2

class SessionThread extends Thread {
    private Socket socket;

    public SessionThread(Socket socket) {
        this.socket = socket;
    }

    @Override
    public void run() {
        try (OutputStream os = socket.getOutputStream();
             InputStream is = socket.getInputStream()) {

            while (true) {
                byte[] buf = new byte[512];
                int len = is.read(buf);
                if (len == -1) {
                    System.out.println("Disconnected");
                    break;
                }

                os.write(buf, 0, len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.println("Thread 종료되었습니다.");
    }
}


// Echo Server
public class Main {
    public static void main(String[] args) {
        try {
            // ServerSocket
            //    socket()
            //    bind()
            //    listen(32)
            ServerSocket serverSocket = new ServerSocket(5000);

            while (true) {
                Socket socket = serverSocket.accept();
                SessionThread sessionThread = new SessionThread(socket);
                sessionThread.start();
            }

        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}
```

<br>

> ECHO CLIENT

```java
package xyz.ourguide;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        try {
            Scanner scanner = new Scanner(System.in);
            Socket socket = new Socket("127.0.0.1", 5000);

            try (InputStream is = socket.getInputStream();
            OutputStream os = socket.getOutputStream()) {

                byte[] buf = new byte[512];
                while (scanner.hasNext()) {
                    String line = scanner.next();
                    os.write(line.getBytes());

                    int len = is.read(buf);
                    if (len == -1) {
                        break;
                    }

                    System.out.println("From server: " + new String(buf, 0, len));
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

* * *

<br>

> ChatServer

```java
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.ArrayList;
import java.util.List;

class SessionThread extends Thread {
    private Socket socket;
    private List<SessionThread> sessions;

    public SessionThread(Socket socket, List<SessionThread> sessions) {
        this.socket = socket;
        this.sessions = sessions;
        this.sessions.add(this);
    }

    private void broadcast(String message) throws IOException {
        for (SessionThread e : sessions) {
            OutputStream os = e.socket.getOutputStream();
            os.write(message.getBytes());
        }
    }

    @Override
    public void run() {
        try (InputStream is = socket.getInputStream()) {

            while (true) {
                byte[] buf = new byte[512];
                int len = is.read(buf);
                if (len == -1) {
                    System.out.println("Disconnected");
                    break;
                }

                String message = new String(buf, 0, len);
                broadcast(message);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.println("Thread 종료되었습니다.");
        sessions.remove(this);
    }
}


// Echo Server
public class Main {
    public static void main(String[] args) {
        List<SessionThread> sessions = new ArrayList<>();
        try {
            ServerSocket serverSocket = new ServerSocket(5000);

            while (true) {
                Socket socket = serverSocket.accept();
                SessionThread sessionThread = new SessionThread(socket, sessions);
                sessionThread.start();
            }

        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}
```

<br>

> ChatClient

```java
package xyz.ourguide;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;
import java.util.Scanner;

// Processing => GUI(Client) => 산성비
// Server(1초에 한개씩 연결된 클라이언트에게 단어를 전달한다.)

// Clean Code
//  : 로버트 C 마틴(엉클 밥)

// 최소 지식의 원칙: 디미터의 법칙
// => 객체를 설계할 때, 최소한의 필요한 정보만 전달 받는 것이 좋다.
class ReceiveThread extends Thread {
    private InputStream is;

    ReceiveThread(InputStream is) {
        this.is = is;
    }

    @Override
    public void run() {
        byte[] buf = new byte[512];
        try {
            while (true) {
                int len = is.read(buf);
                if (len == -1) {
                    break;
                }

                System.out.println("From server: " + new String(buf, 0, len));
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                is.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}


public class Main {
    public static void main(String[] args) {
        try {
            Scanner scanner = new Scanner(System.in);
            Socket socket = new Socket("127.0.0.1", 5000);

            ReceiveThread receiveThread = new ReceiveThread(socket.getInputStream());
            receiveThread.start();

            try (OutputStream os = socket.getOutputStream()) {
                while (scanner.hasNext()) {
                    String line = scanner.next();
                    os.write(line.getBytes());
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

<br>


> Vector를 사용하지 않는 이유

```java
// Vector: Java의 잘못된 동적 배열 라이브러리
// 상속
//  => 부모의 구현과 인터페이스를 물려 받는다.

//  : 깨지기 쉬운 기반 클래스 문제
// 해결방법
//  1. 반드시 부모 클래스의 모든 필드는 private 이어야 한다.
//  2. 부모 클래스의 메소드를 통해 부모 클래스를 사용해야 한다.

// 클래스: 재사용
// => 잘못 사용하기 어렵게 만들어야 한다.

// 재사용을 사용하기 위해서는 '포함'이 좋다.
class Stack<E> {
    private List<E> list = new ArrayList<E>();

    public void push(E e) {
        list.add(e);
    }

    public E pop() {
        return list.remove(list.size() - 1);
    }

    public static void main(String[] args) {
        Stack<Integer> s = new Stack<>();
        s.push(10);
        s.push(20);
        s.push(30);

        System.out.println(s.pop());
        System.out.println(s.pop());
        System.out.println(s.pop());
    }
}

/*
class Stack<E> extends Vector<E> {
    public void push(E e) {
        elementData[elementCount++] = e;
    }

    public E pop() {
        return (E)elementData[--elementCount];
    }

    public static void main(String[] args) {
        Stack<Integer> s = new Stack<>();
        s.push(10);
        s.push(20);
        s.push(30);

        System.out.println(s.pop());
        System.out.println(s.pop());
        System.out.println(s.pop());
    }
}
*/
```
