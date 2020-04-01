# BlockingQueue
>[文献一](https://blog.csdn.net/u010412719/article/details/52337471)

- ArrayBlockingQueue
- LinkedBlockingQueue
- PriorityBlockingQueue
- SynchronousQueue

## ArrayBlockingQueue
>ArrayBlockingQueue是一个基于数组且有界的阻塞队列。此队列按 FIFO（先进先出）原则对元素进行排序。即队列头保存的是在队列中待的时间最长的元素。队列尾则是待的时间最短的元素。元素插入到对尾，在队首获取元素。

### ArrayBlockingQueue的相关属性
```
    final Object[] items;
    int takeIndex;
    int putIndex;
    int count;

    /** Main lock guarding all access */
    final ReentrantLock lock;

    /** Condition for waiting takes */
    private final Condition notEmpty;

    /** Condition for waiting puts */
    private final Condition notFull;
```
### ArrayBlockingQueue的构造方法
```
    //创建一个指定大小的队列对象。
    public ArrayBlockingQueue(int capacity) {
        this(capacity, false);
    }
    //第二参数为True时为公平队列，按顺序访问队列。
    public ArrayBlockingQueue(int capacity, boolean fair) {
        if (capacity <= 0)
            throw new IllegalArgumentException();
        this.items = new Object[capacity];
        lock = new ReentrantLock(fair);
        notEmpty = lock.newCondition();
        notFull =  lock.newCondition();
    }
```
### ArrayBlockingQueue类中的put方法
```
    public void put(E e) throws InterruptedException {
        checkNotNull(e);//检查是否为空，如果为空，则抛NullPointerException
        //获取锁，
        final ReentrantLock lock = this.lock;
        lock.lockInterruptibly();
        try {
            //检查是否已满，如果已满，则调用Condition的await方法等待并释放锁
            while (count == items.length)
                notFull.await();
            enqueue(e);//如果没满，则直接加入到队列中
        } finally {
            lock.unlock();//最后释放锁
        }
    }

    private static void checkNotNull(Object v) {
        if (v == null)
            throw new NullPointerException();
    }
    private void enqueue(E x) {
        // assert lock.getHoldCount() == 1;
        // assert items[putIndex] == null;
        final Object[] items = this.items;
        items[putIndex] = x;
        if (++putIndex == items.length)//转换下指针
            putIndex = 0;
        count++;
        //当添加元素后，则唤醒一个消费者
        notEmpty.signal();
    }
```
此方法的实现思路如下：
1. 判断要添加的元素是否为null，如果为null，则抛异常。否则进行 2
2. 加锁
3. 检测队列是否已满，如果已满，则等待并释放锁(condtion的await方法会释放锁)，如果没有满，则将元素加入到队列中即可。
4. 释放锁

###  ArrayBlockingQueue类中的take方法
```
    public E take() throws InterruptedException {
        final ReentrantLock lock = this.lock;
        lock.lockInterruptibly();
        try {
            //如果队列中存储的元素为空，则等待直至队列中非空
            while (count == 0)
                notEmpty.await();
            return dequeue();
        } finally {
            lock.unlock();
        }
    }

    /**
     * Extracts element at current take position, advances, and signals.
     * Call only when holding lock.
     */
    private E dequeue() {
        // assert lock.getHoldCount() == 1;
        // assert items[takeIndex] != null;
        final Object[] items = this.items;
        @SuppressWarnings("unchecked")
        E x = (E) items[takeIndex];
        items[takeIndex] = null;//置为null
        if (++takeIndex == items.length)
            takeIndex = 0;
        count--;
        if (itrs != null)
            itrs.elementDequeued();
        //唤醒等待的生产者
        notFull.signal();
        return x;
    }
```
函数功能：取出队列中队首的元素，如果为空，则等待直至队列为非空。
实现思路如下：
1. 加锁
2. 检查队列是否为空，如果为空，则阻塞等待，否则取出队首的元素并返回。
3. 释放锁。

## SynchronousQueue
[文献](https://www.jianshu.com/p/95cb570c8187)

## Demo
1. ArrayBlockingQueue实现生产者消费者
```
public class ArrayBlockingQueueDemo {
    static private ArrayBlockingQueue<Integer> queue = new ArrayBlockingQueue(1);
    public static void main(String[] args) throws InterruptedException {
        Producer producer = new Producer();
        Consumer consumer = new Consumer();
        new Thread(producer).start();
        new Thread(consumer).start();
    }
    static class Producer implements Runnable {
        @Override
        public void run() {
                try {
                    for (int i=0; i<5; i++){
                        queue.put(i);
                        System.out.println("Producer: " + i);
                        Thread.sleep(100);
                    }
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
        }
    }
    static class Consumer implements Runnable{
        @Override
        public void run() {
            while (true){
                try {
                    int i = queue.take();
                    System.out.println("Consumer: " + i);
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
2. LinkedBlockingQueue实现生产者消费者
```
public class LinkedBlockingQueueDemo {
    static private LinkedBlockingQueue<Integer> queue = new LinkedBlockingQueue<>();
    public static void main(String[] args) {
        new Thread(() -> {
            for (int i=0; i<=5;i++){
                try {
                    queue.put(i);
                    System.out.println("Producer: " + i);
                    Thread.sleep(200);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
        new Thread(() ->{
            while (!queue.isEmpty()){
                try {
                    System.out.println("Consumer: " + queue.take());
                    Thread.sleep(200);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }
}
```