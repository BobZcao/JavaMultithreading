# Java多线程编程
## 第一部分基础知识
### 第二章 线程安全性

#### 2.1 什么是线程安全性
1. 单线程的正确性是“所见即所得”
* `在线程安全类中封装了必要的同步机制，因此客户端无须进一步采取同步措施。`
```java
@ThreadSafe
public class StatelessFactorizer implements Servlet{
    public void service(ServletRequest req, ServletResponse resp){
        BigInteger i = extractFromRequest(req);
        BigInteger[] factors  = factor(i);
        encodeIntoResponse(resp, factors);
    }
}
```
* StatelessFactorizer是**无状态的**：它及不包含任何域，也不包含任何对其他类域的引用。计算过程中的临时状态仅存在于线程栈上的局部变量中，
并且只能由正在执行的线程访问。访问StatelessFactorizer的线程不会影响另一个访问同一个StatelessFactorizer的线程的计算结果，因为这两
个线程并没有共享状态，就好像他们在访问不同的实例。由于线程访问无状态对象的行为并不会影响其他线程中操作的正确性，因此无状态对象是线程安全的。

* `无状态的对象一定是线程安全的`

#### 2.2 原子性
```java
@NotThreadSafe
public class UnsafeCountingFactorizer implements Servlet{
    private long count = 0;
    public long getCount(){ return count; }
    public void service(ServletRequest req, ServletResponse resp){
        BigInteger i = extractFromRequest(req);
        BigInteger[] factors = factor(i);
        ++count;
        encodeIntoResponse(resp, factors);
    }
}
```

