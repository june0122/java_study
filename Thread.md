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




### *23-2. 쓰레드의 특성*

### *23-3. 동기화(Synchronization)*

### *23-4. 새로운 동기화 방식*
