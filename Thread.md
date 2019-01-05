# Chapter 23. 쓰레드(Thread)와 동기화

### *23-1. 쓰레드의 이해와 생성*

프로세스(Process) : 현재 **실행 중**에 있는 모든 프로그램. 컴퓨터에서 연속적으로 실행되고 있는 컴퓨터 프로그램

쓰레드(Thread) : 어떠한 프로그램 내에서, 특히 프로세스 내에서 실행되는 흐름의 단위

'프로세스는 쓰레드를 담는 그릇이다.'

> 프로세스의 메모리 공간, 프로세스를 구성하는 메모리 공간의 모습

![Structure of Process](https://t1.daumcdn.net/cfile/tistory/2453685056EEACBE22)

[프로세스의 개념](http://bowbowbow.tistory.com/16) 

<br><br>
![Structure of Thread](https://2.bp.blogspot.com/-3AB4sE53Dfw/VMVNdWa_V0I/AAAAAAAAACo/UAGFO7f6_UA/s1600/euva3a00.p54z.gif)

> 위 그림은 프로세스와 쓰레드의 메모리 구조의 차이점을 보여준다. 왼쪽의 프로세스는 이미 봤기 때문에 설명하지 않고, 오른쪽의 thread에 주목하자. 앞서 말한 것처럼 쓰레드는 프로세스 안에 존재하는 실행흐름이다. 메모리 구조 역시 그러하다. 하지만 특이한 점은 쓰레드는 프로세스의 heap, static, code 영역 등을 공유한다는 사실이다. 각각의 프로세스가 독립적인 stack, heap, code, data 영역을 가진 반면에, 한 프로세스에 속한 쓰레드는 stack 영역을 제외한 메모리 영역은 공유한다. 

쓰레드가 별도의 실행 흐름을 갖기 때문에 스택(stack)을 별도로 관리할 수 밖에 없다. (쓰레드가 생성이 되면 스택이 별도로 생성된다.)

메소드(data) 영역과 힙(heap)은 모든 쓰레드가 접근·활용·공유가 가능하다. 

<br><br>
***■ 쓰레드의 생성***

> Thread 클래스를 상속 → Thread용 클래스를 디자인한다 → 쓰레드의 main 메소드가 되는 run 메소드를 정의한다.

Thread 클래스 : 쓰레드를 생성하기 위해서 쓰레드가 할 일을 정의해놓은 클래스




```java
class ShowTread extends Thread
{
    String threadName;
    public ShowThread(String name)
    {
        threadName=name;
    }
    
    // 쓰레드의 main 메소드가 run 이다.
    public void run()
    {
       for(int i=0; i<100; i++)
       {
           System.out.println("안녕하세요. "+threadName+"입니다.");
           try
           {
              sleep(100);
           }
           catch(InterruptedException e)
           {
              e.printStackTrace();
           }
       }
    }
}
```

```java
public static void main(String[] args)
{
    ShowThread st1=new ShowThread("멋진 쓰레드");
    ShowThread st2=new ShowThread("예쁜 쓰레드");
    st1.start();
    st2.start();
}

// start 메소드가 호출되면 쓰레드가 생성되고, 생성된 쓰레드는 run 메소드를 호출한다.
```

<br><br>
***■ 쓰레드를 생성하는 두 번째 방법***

Runnable 인터페이스를 구현하는 클래스의 인스턴스를 대상으로 Thread 클래스의 객체를 생성한다. 이 방법은 상속할 클래스가 존재할 때 유용하게 사용된다.

`쓰레드 인스턴스의 생성, run 메소드의 정의` : 쓰레드를 생성하는 두 가지 방법 모두 위의 조건을 만족하므로 사실상 동일한 방법임을 알 수 있다.

```java
class Sum
{
    int num;
    public Sum() { num=0; }
    public void addNum(int n) { num+=n; }
    public int getNum() { return num; }
}

class AdderThread extends Sum implements Runnable
{
    int start, end;
    public AdderThread(int s, int e)
    {
        start=s;
        end=e;
    }
    public void run()
    {
        for(int i=start; i<=end; i++)
            addNum(i);
    }
}
```

```java
public static void main(String[] args)
{
    AdderThread at1=new AdderThread(1, 50);
    AdderThread at2=new AdderThread(51, 100);
    Thread tr1=new Thread(at1);
    Thread tr2=new Thread(at2);
    tr1.start();
    tr2.start();
    
    // join 메소드가 호출되면, 해당 쓰레드의 종료를 기다리게 된다.
    
    try
    {
        tr1.join();
        tr2.join();
    }
    catch(InterruptedException e)
    {
        e.printStackTrace();
    }
    System.out.println("1~100부터 까지의 합 : "+(at1.getNum()+at2.getNum()));   
}
```

<br><br>
### *23-2. 쓰레드의 특성*

***■ 쓰레드의 스케줄링과 우선순위 컨트롤***

> 쓰레드 스케줄링의 두 가지 기준

- 우선순위가 높은 쓰레드의 실행을 우선시한다.

- 우선순위가 동일할 때는 CPU의 할당시간을 나눈다.

```java
class MessageSendingThread extends Thread
{
    String message;
    
    public MessageSendingThread(String str)
    {
        message=str;
    }
    public void run()
    {
        for(int i=0; i<1000000; i++)
            System.out.println(message+"("+getPriority()+")");
    }
}
```

```java
public static void main(String[] args)
{
    MessageSendingThread tr1= new MessageSendingThread("First");
    MessageSendingThread tr2= new MessageSendingThread("Second");
    MessageSendingThread tr3= new MessageSendingThread("Third");
    tr1.start();
    tr2.start();
    tr3.start();
}
```

```java
// 실행결과

First(5)
First(5)
Second(5)
......
Third(5)
First(5)
......
Third(5)
```

메소드 getPriority의 반환값을 통해서 쓰레드의 우선순위를 확인할 수 있다.

위의 실행결과에서 보이듯이, 우선순위와 관련해서 별도의 지시를 하지 않으면, 동일한 우선순위의 쓰레드들이 생성된다.

<br><br>
***■ 우선순위가 다른 쓰레드들의 실행***

```java
class MessageSendingThread extends Thread
{
    String message;
    public MessageSendingThread(String str)
    {
        message=str;
        
        // 우선순위 설정
        setPriority(prio);
    }
    public void run()
    {
        for(int i=0; i<1000000; i++)
            System.out.println(message+"("+getPriority()+")");
    }
}
```

```java
public static void main(String[] args)
{
    MessageSendingThread tr1= new MessageSendingThread("First", Thread.MAX_PRIORITY);
    MessageSendingThread tr2= new MessageSendingThread("Second", Thread.NORM_PRIORITY);
    MessageSendingThread tr3= new MessageSendingThread("Third", Thread.MIN_PRIORITY);
    tr1.start();
    tr2.start();
    tr3.start();
}
```

```java
// 실행결과

First(10)
First(10)
......
Second(5)
Second(5)
......
Third(1)
```

**Thread.MAX_PRIORITY**는 상수로 10

**Thread.NORM_PRIORITY**는 상수로 5

**Thread.MIN_PRIORITY**는 상수로 1

실행결과에서 보이듯 쓰레드의 실행시간은 우선순위의 비율대로 나뉘지 않는다.

높은 우선순위의 쓰레드가 종료되어야 낮은 우선순위의 쓰레드가 실행된다.

<br><br>
***■ 쓰레드의 라이프 사이클***

![Life Cycle of Thread](https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory&fname=http%3A%2F%2Fcfile23.uf.tistory.com%2Fimage%2F123BBA494F699E82126112)

<br><br>

|상태|설명|
|---|---|
|New|- 객체 생성<br>- 스레드가 만들어진 상태<br>- 아직 start() 메소드가 호출되지 않은 상태|
|Runnable|- 실행 대기<br>- 실행 상태로 언제든지 갈 수 있는 상태<br> - 스레드 객체가 생선된 후에 start() 메서드를 호출하면 Runnable 상태로 이동|
|Running|- 실행 상태<br>- Runnable 상태에서 스레드 스케줄러에 의해 Running 상태로 이동<br>- 스케줄러는 Running 상태의 스레드 중 하나를 선택해서 실행|
|Blocked|- 일시 정지<br>- 사용하고자 하는 객체의 lock이 풀릴 때까지 기다리는 상태<br>- 스레드가 다른 특정한 이유로 Running 상태에서 Blocked 상태로 이동|
|WAITING|- 일시 정지<br>- 다른 스레드가 통지할 때까지 기다리는 상태|
|TIMED_WAITING|- 일시 정지<br>- 주어진 시간 동안 기다리는 상태|
|TERMINATED(DEAD)|- 실행을 마친 상태(종료)<br>- run() 메소드 완료시 스레드가 종료되면 그 스레드는 다시 시작할 수 없게 된다.|

<br>

Runnable 상태의 쓰레드만이 스케줄러에 의해 스케줄링이 가능하다.

그리고 앞서 보인 sleep, join 메소드의 호출로 인해서 쓰레드는 Blocked 상태가 된다.

한 번 종료된 쓰레드는 다시 Runnable 상태가 될 수 없지만, Blocked 상태의 쓰레드는 조건이 성립되면 다시 Runnable 상태가 된다.




### *23-3. 동기화(Synchronization)*

### *23-4. 새로운 동기화 방식*
