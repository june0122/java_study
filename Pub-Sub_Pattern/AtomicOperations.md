```
package xyz.ourguide;

// Concurrent Package
//  => 더그 리 교수

import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;

class Producer extends Thread {
    private final BlockingQueue<String> queue;

    Producer(BlockingQueue<String> queue) {
        this.queue = queue;
    }
    
    @Override
    public void run() {
        for (int i = 0; i < 100; ) {
            try {
                queue.put("hello - " + i);
                System.out.println("Prod - " + i);
                i++;
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class Consumer extends Thread {
    private final BlockingQueue<String> queue;

    Consumer(BlockingQueue<String> queue) {
        this.queue = queue;
    }

    @Override
    public void run() {
        for (int i = 0; i < 100; ++i) {
            try {
                String e = queue.take();
                System.out.println("Consume - " + e);
            } catch (InterruptedException e1) {
                e1.printStackTrace();
            }
        }
    }
}

// Queue
//  => BlockingQueue
public class Main {
    public static void main(String[] args) throws Exception {
        BlockingQueue<String> queue = new ArrayBlockingQueue<String>(5);
        Thread[] threads = new Thread[] {
                new Producer(queue),
                new Producer(queue),
                new Producer(queue),
                new Producer(queue),
                new Consumer(queue),
                new Consumer(queue),
                new Consumer(queue),
                new Consumer(queue),
        };

        for (Thread thread : threads) {
            thread.start();
        }

        for (Thread thread : threads) {
            thread.join();
        }
    }
}


/*
// Java 에서는 Mutex를 별도의 객체로 제공하지 않는다.
//  => 모든 객체는 Mutex가 될 수 있다.
import java.util.concurrent.atomic.AtomicInteger;

// CAS(Atomic Operations)
class FooThread extends Thread {
    // static int n = 0;
    static AtomicInteger n = new AtomicInteger(0);
    static final Object mutex = new Object();

    @Override
    public void run() {
        for (int i = 0; i < 1000000; ++i) {
            n.incrementAndGet();
        }
    }
}

/*
class FooThread extends Thread {
    static int n = 0;
    static final Object mutex = new Object();

    @Override
    public void run() {
        for (int i = 0; i < 1000000; ++i) {
            synchronized (mutex) {
                n++;
            }
        }
    }
}


public class Main {
    public static void main(String[] args) throws Exception {
        Thread[] threads = new Thread[]{
                new FooThread(),
                new FooThread(),
        };

        for (int i = 0; i < 2; ++i) {
            threads[i].start();
        }

        for (int i = 0; i < 2; ++i) {
            threads[i].join();
        }

        System.out.println(FooThread.n);
    }
}
*/
```
