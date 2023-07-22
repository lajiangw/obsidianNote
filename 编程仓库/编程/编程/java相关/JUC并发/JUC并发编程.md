[1. 线程基础知识复习 · 语雀](https://www.yuque.com/gongxi-wssld/csm31d/ln0mq1w7wp1oy99g)
## 1 JUC 是什么
java. util. concurrent 在[并发编程](https://so.csdn.net/so/search?q=%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B&spm=1001.2101.3001.7020)中使用的工具包
![](imgs/Pasted%20image%2020230720211016.png)

## 2 基础知识复习
线程分为以下内容
- 1把锁：synchronized（后面细讲）

- 2个并：
- 并发（concurrent）：是在同一实体上的多个事件，是在**一台机器**上“**同时**”处理多个任务，同一时刻，其实是**只有一个**事情再发生。
- 并行（parallel）：是在不同实体上的多个事件，是在**多台处理器**上同时处理多个任务，同一时刻，大家都在做事情，你做你的，我做我的，各干各的。

- 3个程：
- 进程：在系统中运行的一个应用程序，每个进程都有它自己的内存空间和系统资源
- 线程：也被称为轻量级进程，在同一个进程内会有1个或多个线程，是大多数操作系统进行时序调度的基本单元。
- 管程：Monitor（锁），也就是我们平时所说的锁。Monitor其实是一种**同步机制**，它的义务是保证（同一时间）只有一个线程可以访问被保护的数据和代码，JVM中同步是基于进入和退出监视器（Monitor管程对象）来实现的，每个对象实例都会有一个Monitor对象，Monitor对象和Java对象一同创建并销毁，底层由C++语言实现。

- 线程分类（一般不做特别说明配置，默认都是用户线程）：
- **用户线程**：是系统的工作线程，它会完成这个程序需要完成的业务操作。
- **守护线程**：是一种特殊的线程为其他线程服务的，在后台默默地完成一些系统性的任务，比如垃圾回收线程就是最典型的例子。守护线程作为一个服务线程，没有服务对象就没有必要继续运行了，如果用户线程全部结束了，意味着程序需要完成的业务操作已经结束了，系统可以退出了。所以假如当系统只剩下守护线程的时候，守护线程伴随着 JVM 一同结束工作。

用户线程和守护线程的关系类似老总和秘书，老总负责主要工作，秘书则负责一切杂活。
`setDaemon` 设置这个线程为守护线程
`isDaemon` 判断这个线程是否为守护线程

守护线程 demo：
```java
public class DaemonDemo {
    public static void main(String[] args) {
        Thread t1 = new Thread(() -> {
            System.out.println(Thread.currentThread().getName() + " 开始运行," + (Thread.currentThread().isDaemon() ? "守护线程" : "用户线程"));
            while (true) {

            }
        }, "t1");
        t1.setDaemon(true);//通过设置属性Daemon来设置当前线程是否为守护线程
        t1.start();
        try {
            TimeUnit.SECONDS.sleep(3);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println(Thread.currentThread().getName() + " 主线程结束");
    }
}
```


# 2.1 Future 接口理论知识复习
Future接口（FutureTask实现类）定义了操作`异步任务`执行一些方法，如获取异步任务的执行结果、取消异步任务的执行、判断任务是否被取消、判断任务执行是否完毕等。

举例：比如主线程让一个子线程去执行任务，子线程可能比较耗时，启动子线程开始执行任务后，主线程就去做其他事情了，忙完其他事情或者先执行完，过了一会再才去获取子任务的执行结果或变更的任务状态（老师上课时间想喝水，他继续讲课不结束上课这个主线程，让学生去小卖部帮老师买水完成这个耗时和费力的任务）。 
![](imgs/Pasted%20image%2020230721112453.png)

# Callable 接口复习
`Callable` 接口是 Java 提供的一个并发编程工具，它与另一个接口 `Runnable` 类似，都用于表示可以在多线程环境中执行的任务。但是，`Callable` 接口在某些方面与 `Runnable` 有所不同：
1. 返回结果：`Runnable` 的 `run()` 方法没有返回值，而 `Callable` 的 `call()` 方法可以返回一个结果。`call()` 方法使用 `Future` 对象进行包装，可以用于获取执行结果，或者在任务完成时阻塞地获取结果。
    
2. 抛出异常：`Runnable` 的 `run()` 方法不能抛出受检查的异常（checked exception），只能通过捕获并在内部处理。而 `Callable` 的 `call()` 方法可以抛出受检查的异常，允许在任务执行过程中抛出异常。

```java
public interface Callable<V> {
    V call() throws Exception;
}
```

其中，`V` 是任务执行完毕后的返回结果类型。

# 2.2.1 Future 接口能干什么
Future 是 Java5新加的一个接口，它提供一种异步并行计算的功能，如果主线程需要执行一个很耗时的计算任务，我们会就可以通过 Future 把这个任务放进异步线程中执行，主线程继续处理其他任务或者先行结束，再通过 Future 获取计算结果。 

# Future 接口相关架构
- 目的：异步多线程任务执行且返回有结果，三个特点：多线程、有返回、异步任务（班长为老师去买水作为新启动的异步多线程任务且买到水有结果返回）
- 代码实现：Runnable 接口+Callable 接口+Future 接口和 FutureTask 实现类。 

![](imgs/Pasted%20image%2020230721115413.png)

FutureTask 开启异步任务：
```java
public class FutureTest {  
public static void main(String[] args) throws ExecutionException, InterruptedException {  
Cat cat = new Cat();  
FutureTask<String> stringFutureTask = new FutureTask<>(cat);  
new Thread(stringFutureTask).start();  
System.out.println("main end~");  
System.out.println(stringFutureTask.get());  
}  
}  
  
class Cat implements Callable<String> {  
  
@Override  
public String call() throws Exception {  
System.out.println("cat coin in~");  
return "喵喵喵~";  
}  
}
```

# Future 编码实战和优缺点分析
- 优点：Future+线程池异步多线程任务配合，能显著提高程序的运行效率。
---
- 缺点：
	- - get ()阻塞---一旦调用 get ()方法求结果，一旦调用不见不散，非要等到结果才会离开，不管你是否计算完成，如果没有计算完成容易程序堵塞。
	- IsDone ()轮询---轮询的方式会耗费无谓的 cpu 资源，而且也不见得能及时得到计算结果，如果想要异步获取结果，通常会以轮询的方式去获取结果，尽量不要阻塞。
---
-  结论：Future 对于结果的获取不是很友好，只能通过**阻塞或轮询**的方式得到任务的结果。

Demo
```java
public class FutureApiDemo {
    public static void main(String[] args) throws ExecutionException, InterruptedException, TimeoutException {
        FutureTask<String> futureTask = new FutureTask<>(() -> {
            System.out.println(Thread.currentThread().getName() + "--------come in");
            try {
                TimeUnit.SECONDS.sleep(5);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            return "task over";
        });

        Thread t1 = new Thread(futureTask, "t1");
        t1.start();

//        System.out.println(futureTask.get());//这样会有阻塞的可能，在程序没有计算完毕的情况下。
        System.out.println(Thread.currentThread().getName() + " ------忙其他任务");
//        System.out.println(futureTask.get(3,TimeUnit.SECONDS));//只愿意等待三秒，计算未完成直接抛出异常
        while (true) {//轮询
            if(futureTask.isDone()){
                System.out.println(futureTask.get());
                break;
            }else{
                TimeUnit.MILLISECONDS.sleep(500);
                System.out.println("正在处理中，不要催了，越催越慢");
            }
        }
        /* 轮询结果
        * main ------忙其他任务
        t1--------come in
        正在处理中，不要催了，越催越慢
        正在处理中，不要催了，越催越慢
        正在处理中，不要催了，越催越慢
        正在处理中，不要催了，越催越慢
        正在处理中，不要催了，越催越慢
        正在处理中，不要催了，越催越慢
        正在处理中，不要催了，越催越慢
        正在处理中，不要催了，越催越慢
        正在处理中，不要催了，越催越慢
        正在处理中，不要催了，越催越慢
        task over
        Process finished with exit code 0
        * */
    }
}
```

# 多线程完成一些复杂的任务
- 对于简单的业务场景使用 Future 完全 ok

- 回调通知：
- 应对Future的完成时间，完成了可以告诉我，也就是我们的回调通知
- 通过轮询的方式去判断任务是否完成这样非常占cpu并且代码也不优雅

- 创建异步任务：Future+线程池组合
- 多个任务前后依赖可以组合处理（水煮鱼--->买鱼--->调料--->下锅）：
- 想将多个异步任务的结果组合起来，后一个异步任务的计算结果需要钱一个异步任务的值
- 想将两个或多个异步计算合并成为一个异步计算，这几个异步计算互相独立，同时后面这个又依赖前一个处理的结果

- 对计算速度选最快的：
- 当Future集合中某个任务最快结束时，返回结果，返回第一名处理结果

- **结论**：
- 使用Future之前提供的那点API就囊中羞涩，处理起来不够优雅，这时候还是让CompletableFuture以声明式的方式优雅的处理这些需求。
- 从i到i++
- Future 能干的，`CompletableFuture` 都能干

# CompletableFuture 为什么会出现
- get ()方法在 Future 计算完成之前会一直处在阻塞状态下，阻塞的方式和异步编程的设计理念相违背。
- isDene()方法容易耗费cpu资源（cpu空转），
- 对于真正的异步处理我们希望是可以通过传入回调函数，在Future结束时自动调用该回调函数，这样，我们就不用等待结果

jdk8设计出 CompletableFuture，CompletableFuture 提供了一种观察者模式类似的机制，可以让任务执行完成后通知监听的一方。 

## CompletableFuture 和 CompletionStage 介绍
![](imgs/Pasted%20image%2020230721131121.png)
- **接口 CompletionStage**
- 代表异步计算过程中的某一个阶段，一个阶段完成以后可能会触发另外一个阶段。
- 一个阶段的执行可能是被单个阶段的完成触发，也可能是由多个阶段一起触发
---
- **类 CompletableFuture**
- 提供了非常强大的 Future 的扩展功能，可以帮助我们简化异步编程的复杂性，并且提供了函数式编程的能力，可以通过回调的方式处理计算结果，也提供了转换和组合 CompletableFuture 的方法 
- 它可能代表一个明确完成的 Future，也可能代表一个完成阶段（CompletionStage），它支持在计算完成以后触发一些函数或执行某些动作 

## 核心的四个静态方法，来创建一个异步任务
![](imgs/Pasted%20image%2020230721132944.png)
对于上述 Executor 参数说明：若没有指定，则使用默认的 ForkJoinPoolcommonPool（）作为它的线程池执行异步代码，如果指定线程池，则使用我们自定义的或者特别指定的线程池执行异步代码。
官方推荐使用静态方法创建异步任务。
**当使用系统默认线程池时，主线程的结束，线程池也会随着关闭，也会导致其他使用默认线程池的线程结束。**
方法 demo：
```java
/**  
* @author ZhangMinlei  
* @description  
* @date 2023-07-21 13:30  
*/  
public class CompletableFutureTest {  
public static void main(String[] args) throws ExecutionException, InterruptedException {  
// 创建线程池  
ExecutorService executorService = Executors.newFixedThreadPool(3);  
// 启动一个使用默认线程池的异步任务 没有返回值  
CompletableFuture<Void> future1 = CompletableFuture.runAsync(() -> System.out.println("future1"));  
System.out.println(future1.get());  
  
// 启动一个使用自己线程池的异步任务 没有返回值  
  
CompletableFuture<Void> future2 = CompletableFuture.runAsync(() -> System.out.println("future2"), executorService);  
System.out.println(future2.get());  
  
// 启动一个使用自己线程池，有返回值  
CompletableFuture<Integer> future3 = CompletableFuture.supplyAsync(() -> {  
System.out.println(123);  
return 1;  
}, executorService);  
System.out.println(future3.get());  
}  
}
```

CompletableFuture 减少阻塞和轮询，可以传入回调对象，当异步任务完成或者发生异常时，自动调用回调对象的回调方法。 
`whenComplete` 拿到上一个任务的返回值继续执行下一个任务
`exceptionally` 异常处理任务
Demo
```java
public class CompletableFutureTest2 {  
public static void main(String[] args) {  
ExecutorService executorService = Executors.newFixedThreadPool(3);  
  
CompletableFuture.supplyAsync(() -> {  
System.out.println("123");  
// int i = 10 /0;  
return 3;  
}, executorService).whenComplete((integer, throwable) -> {  
if (throwable == null) {  
System.out.println("计算结果正常没有错误");  
}  
System.out.println(integer);  
}).exceptionally(throwable -> {  
throwable.printStackTrace();  
System.out.println("出现了异常");  
return null;  
});  
executorService.shutdownNow();  
}  
}
```

CompletableFuture 优点：
- 异步任务**结束**时，会自动回调某个对象的方法
- 主线程设置好回调后，不用关心异步任务的执行，异步任务之间可以顺序执行
- 异步任务**出错**时，会自动回调某个对象的方法

# 案例精讲 -从电商网站的比价需求展开
## 函数式编程已成为主流
Lambda 表达式+Stream 流式调用+Chain 链式调用+Java8函数式编程

**函数时接口：**
![](imgs/Pasted%20image%2020230721173400.png)

`get()` 方法和 `join()` 方法都是用于等待异步任务完成并获取结果的方法：

- `get()` 方法从 `Future` 接口继承而来，会抛出 `ExecutionException` 和 `InterruptedException`，需要处理异常。

- `join()` 方法是 `CompletableFuture` 特有的方法，会抛出 `CompletionException`，不会抛出受检查的异常，更适合用于流式操作或简化代码。

# 大厂业务需求案例说明
切记：功能--->性能（完成--->完美）

**需求分析：**
1. 需求说明：
	1. 同一款产品，同时搜索出同款产品在各大电商平台的售价
	2. 同一款产品，同时搜索出本产品在同一个电商平台下，各个入驻卖家售价是多少
	3. 同一本书，在京东入驻的每一个商家卖多少钱，在淘宝入驻的每一个商家卖多少钱，

## 一波流 Java8函数式编程带走 -比价案例实战 Case
Demo：
```java
package CompletableFuture;  
  
import lombok.AllArgsConstructor;  
import lombok.Data;  
import lombok.NoArgsConstructor;  
  
import java.util.ArrayList;  
import java.util.List;  
import java.util.concurrent.CompletableFuture;  
import java.util.concurrent.ThreadLocalRandom;  
import java.util.concurrent.TimeUnit;  
import java.util.stream.Collectors;  
  
/**  
* @author ZhangMinlei  
* @description  
* @date 2023-07-21 18:02  
*/  
public class CompletableFutureTest4 {  
static List<NetAll> list = new ArrayList<>();  
  
public static void main(String[] args) {  
list.add(new NetAll("jd"));  
list.add(new NetAll("tb"));  
list.add(new NetAll("ddangdang"));  
// 单线程去查询  
List<String> list1 = get(list, "mysql");  
list1.forEach(System.out::println);  
System.out.println("------------------");  
// 多线程去查询  
List<String> list2 = get2(list, "mysql");  
list2.forEach(System.out::println);  
}  
  
public static List<String> get(List<NetAll> list, String bookName) {  
// 单线程去查询  
return list.stream().map(netAll -> {  
return String.format(bookName + "在 %s 售价 %.2f元", netAll.getNetMallName(), netAll.getPrice());  
}).collect(Collectors.toList());  
}  
  
public static List<String> get2(List<NetAll> list, String bookName) {  
return list.stream().  
map(netAll -> CompletableFuture.supplyAsync(() ->  
String.format(bookName + "在 %s 售价 %.2f元", netAll.getNetMallName(), netAll.getPrice()))).collect(Collectors.toList()).  
stream().map(CompletableFuture::join).collect(Collectors.toList());  
}  
}  
  
@Data  
@NoArgsConstructor  
@AllArgsConstructor  
class NetAll {  
  
private String netMallName;  
  
public double getPrice() {  
try {  
TimeUnit.SECONDS.sleep(1);  
} catch (InterruptedException e) {  
throw new RuntimeException(e);  
}  
return ThreadLocalRandom.current().nextDouble() * 2 + netMallName.charAt(0);  
}  
}
```


## CompletableFuture 常用方法
**常用方法分类**
- **获得结果和触发计算**
	- 获取结果
		- public T get()
		- public T get(long timeout,TimeUnit unit)
		- public T join() --->和get一样的作用，只是不需要抛出异常
		- public T getNow (T valuelfAbsent) --->计算完成就返回正常值，否则返回备胎值（传入的参数），立即获取结果不阻塞。
	- 主动触发计算
		- public boolean complete (T value) ---->是否打断 get 方法立即返回括号值 

- **对计算结果进行处理**
	- thenApply --->计算结果存在依赖关系，这两个线程串行化---->由于存在依赖关系（当前步错，不走下一步），当前步骤有异常的话就叫停
	- handle --->计算结果存在依赖关系，这两个线程串行化---->有异常也可以往下走一步

Demo：
```java
package CompletableFuture;  
  
import java.util.concurrent.CompletableFuture;  
import java.util.concurrent.ExecutorService;  
import java.util.concurrent.Executors;  
import java.util.concurrent.TimeUnit;  
  
/**  
* @author ZhangMinlei  
* @description  
* @date 2023-07-21 19:10  
*/  
public class CompletableFutureApiDemo {  
public static void main(String[] args) throws InterruptedException {  
ExecutorService threadPool = Executors.newFixedThreadPool(3);  
// thenApply 方法演示 有异常就会终止  
CompletableFuture.supplyAsync(() -> {  
System.out.println(111);  
int i = 9 / 0;  
return "111";  
}).thenApply(r -> {  
System.out.println(222);  
return r + "222";  
}).exceptionally(throwable -> {  
if (throwable != null) {  
System.out.println("出现了异常");  
System.out.println(throwable.getMessage());  
}  
return null;  
});  
  
// handle 演示  
CompletableFuture.supplyAsync(() -> {  
System.out.println("1");  
return 1;  
}, threadPool).handle((i, e) -> {  
// 因为这里出现了异常，导致一下代码都不会执行，直接跳到地三个  
int w = 9 / 0;  
System.out.println(2);  
return i + 1;  
}).handle((i,e) -> {  
System.out.println(3);  
System.out.println("i = "+i);  
System.out.println(e.getMessage());  
return null;  
});  
System.out.println("主线程结束");  
threadPool.shutdownNow();  
}  
}
```

- **对计算结果进行消费**
-  接受任务的处理结果，并消费处理，无返回结果。类似充当一个消费者，只能将结果使用掉，并不会产生返回值。
- thenAccept
```java
@Test  
public void thenAccept() {  
CompletableFuture.supplyAsync(() -> "hello").thenAccept(System.out::println);  
}
```
- 对比补充
- thenRun (Runnable runnable) : 任务 A 执行完执行 B，并且不需要 A 的结果, 
- thenAccept(Consumer action): 任务A执行完执行B，B需要A的结果，但是任务B没有返回值
- thenApply (Function fn): 任务 A 执行完执行 B，B 需要 A 的结果，同时任务 B 有返回值
Demo：
```java
@Test  
public void thenATest() {  
CompletableFuture.supplyAsync(() -> "hello").thenAccept(System.out::println);  
CompletableFuture.supplyAsync(() -> "thenRun").thenRun(System.out::println);  
  
CompletableFuture.supplyAsync(() -> "thenApply").thenApply(f -> {  
System.out.println(f);  
return f;  
});  
}  
}
```

- CompletableFuture 和线程池说明
	- 如果没有传入自定义线程池，都用默认线程池 ForkJoinPool
	- 传入一个线程池，如果你执行第一个任务时，传入了一个自定义线程池
		- 调用thenRun方法执行第二个任务时，则第二个任务和第一个任务时共用同一个线程池
		- 调用thenRunAsync执行第二个任务时，则第一个任务使用的是你自定义的线程池，第二个任务使用的是ForkJoin线程池
	
	- 备注：有的时候可能是线程处理太快，系统优化切换原则，直接使用 main 线程处理，
	- thenAccept和thenAcceptAsync，thenApply和thenApplyAsync等，之间的区别同理。
```java
@Test  
public void thenAPool() {  
// 观察线程池可以看出，当启动xxxAsync方法时，会使用默认的线程池，而不是和上一个方法一致。  
ExecutorService executorService = Executors.newFixedThreadPool(3);  
CompletableFuture.runAsync(() -> System.out.println(1 + Thread.currentThread().getName()), executorService)  
.thenRunAsync(() -> System.out.println(1 + Thread.currentThread().getName()))  
.thenRun(() -> System.out.println(1 + Thread.currentThread().getName()));  
System.out.println("main end~");  
executorService.shutdownNow();  
}
```

- **对计算速度进行选用**
	- 谁快用谁
	- applyToEither
Demo：
```java
// 模拟 a小线程完成需要2秒，b线程完成需要1秒， applyToEither方法使用处理快的返回结果来进行操作  
@Test  
public void applyToEitherTest() throws InterruptedException {  
ExecutorService executorService = Executors.newFixedThreadPool(3);  
CompletableFuture<String> planA = CompletableFuture.supplyAsync(() -> {  
try {  
Thread.sleep(200);  
} catch (InterruptedException e) {  
throw new RuntimeException(e);  
}  
System.out.println("A玩家~");  
return "A";  
}, executorService);  
  
CompletableFuture<String> planB = CompletableFuture.supplyAsync(() -> {  
try {  
Thread.sleep(1000);  
} catch (InterruptedException e) {  
throw new RuntimeException(e);  
}  
System.out.println("B玩家~");  
return "B";  
}, executorService);  
  
Thread.sleep(3000);  
CompletableFuture<String> result = planA.applyToEither(planB, f -> f + "获得胜利");  
  
System.out.println(result.join() + Thread.currentThread().getName());  
executorService.shutdownNow();  
}
```

- **对计算结果进行合并**
	
	- 两个CompletableStage任务都完成后，最终能把两个任务的结果一起交给thenCombine来处理
	- 先完成的先等着，等待其他分支任务
	- thenCombine 方法

Demo
```java
@Test  
public void thenCombineTest() {  
// 链式使用  
CompletableFuture.supplyAsync(() -> {  
System.out.println("1号");  
return 1;  
}).thenApply(x -> {  
System.out.println("2号");  
return 2;  
}).thenCombine(CompletableFuture.supplyAsync(() -> {  
System.out.println("3号");  
return 3;  
}), (x, y) -> {  
System.out.println("4号");  
System.out.println(x + y);  
return 4;  
});  
// 单独使用  
CompletableFuture<Integer> f1 = CompletableFuture.supplyAsync(() -> {  
System.out.println("A");  
return 2;  
});  
  
CompletableFuture<Integer> f2 = CompletableFuture.supplyAsync(() -> {  
System.out.println("A");  
return 2;  
});  
  
CompletableFuture<Object> result = f1.thenCombine(f2, (x, y) -> {  
System.out.println(x + y);  
return "任务结束~";  
});  
  
System.out.println(result.join());  
}
```

# 说说 Java"锁"事
## 从轻松的乐观锁和悲观锁开讲
- 悲观锁： 认为自己在使用数据的时候 **一定有别的线程来修改数据**，因此在获取数据的时候会先加锁，确保数据不会被别的线程修改，synchronized 和 Lock 的实现类都是悲观锁，**适合写操作多的场景**，先加锁可以保证写操作时数据正确，显示的锁定之后再操作同步资源-----狼性锁
- synchronized 关键字和 Lock 的实现都是悲观锁
---
- 乐观锁： 认为自己在使用数据的时候 `不会有别的线程修改数据或资源`，不会添加锁，Java 中使用无锁编程来实现，只是在更新的时候去判断，之前有没有别的线程更新了这个数据，如果这个数据没有被更新，当前线程将自己修改的数据成功写入，如果已经被其他线程更新，则根据不同的实现方式执行不同的操作，比如：放弃修改、重试抢锁等等。
	- 判断规则有：版本号机制 Version，`最常采用的是CAS算法，Java原子类中的递增操作就通过CAS自旋实现的。` ----- `适合读操作多的场景，` 不加锁的特性能够使其读操作的性能大幅提升，乐观锁则直接去操作同步资源，是一种无锁算法，得之我幸不得我命---佛系锁

## 锁相关的8种案例演示 code
```java
/**
 * @author Guanghao Wei
 * @create 2023-04-10 14:57
 */

class Phone {
    public synchronized void sendEmail() {
        try {
            TimeUnit.SECONDS.sleep(3);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("------sendEmail");
    }

    public synchronized void sendSMS() {
        System.out.println("------sendSMS");
    }

    public void hello() {
        System.out.println("------hello");
    }
}

/**
 * 现象描述：
 * 1 标准访问ab两个线程，请问先打印邮件还是短信？ --------先邮件，后短信  共用一个对象锁
 * 2. sendEmail钟加入暂停3秒钟，请问先打印邮件还是短信？---------先邮件，后短信  共用一个对象锁
 * 3. 添加一个普通的hello方法，请问先打印普通方法还是邮件？ --------先hello，再邮件
 * 4. 有两部手机，请问先打印邮件还是短信？ ----先短信后邮件  资源没有争抢，不是同一个对象锁
 * 5. 有两个静态同步方法，一步手机， 请问先打印邮件还是短信？---------先邮件后短信  共用一个类锁
 * 6. 有两个静态同步方法，两部手机， 请问先打印邮件还是短信？ ----------先邮件后短信 共用一个类锁
 * 7. 有一个静态同步方法 一个普通同步方法，请问先打印邮件还是短信？ ---------先短信后邮件   一个用类锁一个用对象锁
 * 8. 有一个静态同步方法，一个普通同步方法，两部手机，请问先打印邮件还是短信？ -------先短信后邮件 一个类锁一个对象锁
 */

public class Lock8Demo {
    public static void main(String[] args) {
        Phone phone = new Phone();
        new Thread(() -> {
            phone.sendEmail();
        }, "a").start();

        try {
            TimeUnit.MILLISECONDS.sleep(200);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(() -> {
            phone.sendSMS();
        }, "b").start();
    }

}

```

结论：

- 对于普通同步方法，**锁的是当前实例对象**，通常指 this，所有的同步方法用的都是同一把锁--->实例对象本身，**当有一个线程进入到同步方法中时，其他线程只能等待，不能访问方法中其他的同步方法。**
- 对于静态同步方法，锁的时当前类的Class对象
- 对于同步方法块，锁的时 synchronized 括号内的对象

##  synchronized 有三种应用方式
- 作用于实例方法，当前实例加锁，进入同步代码块前要获得当前实例的锁；
- 作用于代码块，对括号里配置的对象加锁
- 作用于静态方法，当前类加锁，进去同步代码前要获得当前类对象的锁

## 从字节码角度分析 synchronized 实现
- javap -c (v 附加信息) ***. class 文件反编译
	- synchronized同步代码块
	- 实现使用的是 `monitorenter` 和 `monitorexit` 指令
	- 会有连个 `monitorexit` 是因为保证出现异常也要释放锁的情况。
![](imgs/Pasted%20image%2020230722112601.png)

- synchronized 普通同步方法
	
	- 调用指令将会检查方法的 `ACC_SYNCHRONIZED` 访问标志是否被设置，如果设置了，执行线程会将现持有 monitor 锁，然后再执行该方法，最后在方法完成（无论是否正常结束）时释放 monitor
![](imgs/Pasted%20image%2020230722112622.png)

- synchronized 静态同步方法
	
	- ACC_STATIC、ACC_SYNCHRONIZED 访问标志区分该方法是否是静态同步方法

![](imgs/Pasted%20image%2020230722112634.png)


## 反编译 synchronized 锁的是什么
面试题：为什么任何一个对象都可以成为一个锁？
	Java 中，所有类的基类都是 Object，而 Object 又继承了 ObjectMonitor，我们知道，java 的底层是 c++，对应链路：ObjectMonitor. java--->ObjectMonitor. cpp--->ObjectMonitor. hpp。在 ObjectMonitor. hpp 中有一个属性是记录单项线程的使用对象，
	每个对象天生都带着一个对象监视器，每一个被锁住的对象都会和 Monitor 关联起来。


总结：指针指向 Monitor 对象（也称为管程或监视器）的真实地址。每个对象都存在着一个 monitor 与之关联，当一个 monitor 被某个线程持有后，它便处于锁定状态。在 Java 虚拟机（HotSpot）中，monitor 是由 OnjectMonitor 实现的，其主要的数据结构如下（位于 HotSpot 虚拟机源码 ObjectMonitor. hpp 文件，C++实现）： 
![](imgs/Pasted%20image%2020230722114433.png)

![](imgs/Pasted%20image%2020230722114446.png)


# 公平锁和非公平锁
- 公平锁：是指多个线程按照申请锁的顺序来获取锁，这里类似于排队买票，先来的人先买，后来的人再队尾排着，这是公平的----- `Lock lock = new ReentrantLock (true)`---表示公平锁，先来先得。
---
- 非公平锁：是指多个线程获取锁的顺序并不是按照申请的顺序，有可能后申请的线程比先申请的线程优先获取锁，在高并发环境下，有可能造成优先级反转或者饥饿的状态（某个线程一直得不到锁）---- `Lock lock = new ReentrantLock (false)` ---表示非公平锁，后来的也可能先获得锁，默认为非公平锁。 

#面试题   **为什么会有公平锁/非公平锁的设计？为什么默认非公平？**

- 恢复挂起的线程到真正锁的获取还是有时间差的，从开发人员来看这个时间微乎其微，但是从 CPU 的角度来看，这个时间差存在的还是很明显的。**所以非公平锁能更充分地利用 CPU 的时间片，尽量减少 CPU 空间状态时间。**

- 使用多线程很重要的考量点是线程切**换的开销**，当采用非公平锁时，**当一个线程请求锁获取同步状态，然后释放同步状态，所以刚释放锁的线程在此刻再次获取同步状态的概率就变得很大，所以就减少了线程的开销**

- 什么时候用公平？什么时候用非公平？
	- 如果为了更高的吞吐量，很显然非公平锁是比较合适的，因为节省了很多线程切换的时间，吞吐量自然就上去了；否则就用公平锁，大家公平使用。

## 可重入锁 （递归锁）
是指在**同一线程**在外层方法获取到锁的时侯，在进入该线程的内层方法会**自动获取锁**（前提，锁对象的是同一个对象），不会因为之前已经获取过还没释放而阻塞---------优点之一就是可一定程度避免死锁。 
一个线程持有一个锁进入同一个同步资源，只有是同一把锁，就不会堵塞，直接通过。

## 可重入锁种类
- 隐式锁（即 `synchronized` 关键字使用的锁），默认是可重入锁
	- 在一个synchronized修饰的方法或者代码块的内部调用本类的其他`synchronized`修饰的方法或者代码块时，**是永远可以得到锁**。

- 显式锁（即 Lock）也有 `ReentrantLock` 这样的可重入锁。几次打开锁，就要几次释放锁，需要一一对应，否则造成系统堵塞。
Demo：
```java
@Test  
public void test() {  
// 此时他们持有的是一把锁，不会阻塞线程，直接进入。  
Object o = new Object();  
Thread thread = new Thread(() -> {  
System.out.println("1");  
synchronized (o){  
System.out.println("2");  
synchronized (o){  
System.out.println("3");  
}  
}  
});  
thread.start();  
}
```

`ReentrantLock` demo：
```java
public static void main(String[] args) {  
ReentrantLock lock = new ReentrantLock();  
lock.lock();  
System.out.println("come in");  
lock.unlock();  
try {  
lock.lock();  
System.out.println("lock~~~");  
} finally {  
lock.unlock();  
}  
new Thread(() -> {  
lock.lock();  
System.out.println("2");  
lock.unlock();  
}).start();  
}
```

# 死锁及排查
死锁是指两个或两个以上的线程在执行过程中，因**抢夺资源而造成的一种互相等待的现象**，若无外力干涉，**则它们无法再继续推进下去。** 

产生原因：
	- 系统资源不足
	- 进程运行推进顺序不合适
	- 系统资源分配不当
	
![](https://cdn.nlark.com/yuque/0/2023/png/35653686/1681114824880-04489b15-8c38-4a05-8320-cea66ccc805a.png)


# 如何排查死锁
- 纯命令
	在 idea 终端输入指令，或者在项目路径下
	- jps -l
	- jstack 进程编号

- 图形化
	
	- jconsole

死锁 Demo：
```java
public static void main(String[] args) {  
Object a = new Object();  
Object b = new Object();  
  
new Thread(() -> {  
synchronized (a) {  
System.out.println("t1线程持有a锁，试图获取b锁");  
try {  
TimeUnit.SECONDS.sleep(1);  
} catch (InterruptedException e) {  
e.printStackTrace();  
}  
synchronized (b) {  
System.out.println("t1线程获取到b锁");  
}  
}  
}, "t1").start();  
  
new Thread(() -> {  
synchronized (b) {  
System.out.println("t2线程持有a锁，试图获取a锁");  
try {  
TimeUnit.SECONDS.sleep(1);  
} catch (InterruptedException e) {  
e.printStackTrace();  
}  
synchronized (a) {  
System.out.println("t2线程获取到a锁");  
}  
}  
}, "t2").start();  
}
```

指令
![](imgs/Pasted%20image%2020230722145019.png)

图形化
![](imgs/Pasted%20image%2020230722145223.png)

#  LockSupport 与线程中断
Java. lang. Thread 下的三个方法:
![](imgs/Pasted%20image%2020230722150736.png)

**什么是中断机制**？
- 首先，一个线程不应该由其他线程来强制中断或停止，而是应该由线程自己自行停止，自己来决定自己的命运，所以，Thread. stop, Thread. suspend, Thread. resume 都已经被废弃了
- 其次，在Java中没有办法立即停止一条线程，然而停止线程却显得尤为重要，如取消一个耗时操作。因此，`Java提供了一种用于停止线程的协商机制----中断，也即中断标识协商机制`
	- 中断只是一种协作协商机制，Java 没有给中断增加任何语法，中断的过程完全需要程序员自行实现。若要中断一个线程，你需要手动调用该线程 `interrupt` 方法，**该方法也仅仅是将该线程对象的中断标识设置为 true，接着你需要自己写代码不断检测当前线程的标识位，如果为 true，表示别的线程请求这条线程中断，此时究竟应该做什么需要你自己写代码实现。**
	- **每个线程对象都有一个中断标识位**，用于表示线程是否被中断；该标识位为 true 表示中断，为 false 表示未中断；通过调用线程对象的 `interrupt` 方法将该线程的标识位设置为 true；**可以在别的线程中调用，也可以在自己的线程中调用。**

# 中断的相关API方法之三大方法说明

- `public void interrupt ()`
	- 实例方法 Just to set the interrupt flag
	- 实例方法仅仅是设置线程的中断状态为true，发起一个协商而不会立刻停止线程

- `public static boolean interrupted()`
- 静态方法 Thread.interrupted();
- 判断线程是否被中断并清除当前中断状态（做了两件事情）
	
	- 1.返回当前线程的中断状态，测试当前线程是否已被中断
	- 2.将当前线程的中断状态清零并重新设置为false，清除线程的中断状态
	- 3.这个方法有点不好理解在于如果连续两次调用此方法，则第二次返回false，因为连续调用两次的结果可能不一样

- `public boolean isInterrupted()`
	
	- 实例方法
	- 判断当前线程是否被中断（通过检查中断标志位）

# 大厂面试题中断机制考点
 如何停止中断运行中的线程？
- 通过一个 volatile 变量实现
在Java中，`volatile` 是一个关键字，用于声明变量。它的主要作用是**保证可见性和禁止指令重排序**，用于确保多线程之间对该变量的访问是正确的。
Demo：
```java
public class VolatileTest {  
static volatile boolean flag = false;  
  
public static void main(String[] args) {  
new Thread(() -> {  
while (true) {  
if (flag) {  
System.out.println("t1 结束，flag 为ture");  
break;  
}  
System.out.println("t1 come in ");  
}  
}).start();  
  
new Thread(() -> {  
try {  
Thread.sleep(20);  
} catch (InterruptedException e) {  
throw new RuntimeException(e);  
}  
System.out.println("t2 come in ");  
flag = true;  
}).start();  
}  
}
```

- 通过 AutomicBoolean
Demo:
```java
ublic class InterruptDemo {
    static AtomicBoolean atomicBoolean = new AtomicBoolean(false);


    public static void main(String[] args) {
        new Thread(() -> {
            while (true) {
                if (atomicBoolean.get()) {
                    System.out.println(Thread.currentThread().getName() + " atomicBoolean的值被改为true，t1程序停止");
                    break;
                }
                System.out.println("-----------hello atomicBoolean");
            }
        }, "t1").start();
        try {
            TimeUnit.MILLISECONDS.sleep(10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(() -> {
            atomicBoolean.set(true);
        }, "t2").start();

    }
}

/**
 * -----------hello atomicBoolean
 * -----------hello atomicBoolean
 * -----------hello atomicBoolean
 * -----------hello atomicBoolean
 * -----------hello atomicBoolean
 * t1 atomicBoolean的值被改为true，t1程序停止
 */
```

- 通过 Thread 类自带的中断 API 实例方法实现----在需要中断的线程中不断监听中断状态，一旦发生中断，就执行相应的中断处理业务逻辑 stop 线程。
Demo:
```java
@Test  
public void interruptedTest() {  
Thread thread1 = new Thread(() -> {  
while (true) {  
if (Thread.currentThread().isInterrupted()) {  
System.out.println("isInterrupted 被修改为ture ，程序终止");  
break;  
}  
System.out.println("你好！");  
}  
});  
  
thread1.start();  
  
new Thread(() ->{  
System.out.println("修改t1的isInterrupted为false");  
thread1.interrupt();  
}).start();  
}
```

- 当前线程的中断标识为 true，是不是线程就立刻停止？

答案是不立刻停止，具体来说，当对一个线程，调用interrupt时：

- 如果线程处于正常活动状态，那么会将该线程的中断标志设置为 true，仅此而已，被设置中断标志的线程将继续正常运行，不受影响，所以 interrupt ()并不能真正的中断线程，需要被调用的线程自己**进行配合才行**，**对于不活动的线程没有任何影响。** 

- 如果线程处于阻塞状态（例如 sleep, wait, join 状态等），在别的线程中调用当前线程对象的 interrupt 方法，那么线**程将立即退出被阻塞状态（interrupt 状态也将被清除）**，并抛出一个 InterruptedException 异常。 

**总之，需要记住的是中断只是一种协商机制，修改中断标识位仅此而已，不是立刻 stop 打断**

- 静态方法 `Thread. interrupted ()` 测试当前线程是否被打断，（中断标识为为 true），返回一个 boolean，并清除中断中断标识位。
- 第二次调用时，中断标识位的状态已经被清除，将返回一个 false。

Demo：
```java
public class InterruptDemo4 {
    public static void main(String[] args) {
        /**
         * main	false
         * main	false
         * -----------1
         * -----------2
         * main	true
         * main	false
         */
        System.out.println(Thread.currentThread().getName() + "\t" + Thread.interrupted());//false
        System.out.println(Thread.currentThread().getName() + "\t" + Thread.interrupted());//false
        System.out.println("-----------1");
        Thread.currentThread().interrupt();
        System.out.println("-----------2");
        System.out.println(Thread.currentThread().getName() + "\t" + Thread.interrupted());//true
        System.out.println(Thread.currentThread().getName() + "\t" + Thread.interrupted());//false

    }
}

false
false
1
2
true
false
```

对于静态方法 Thread.interrupted ()和实例方法 isInterrupted ()区别在于：
	- 静态方法interrupted将会清除中断状态（传入的参数ClearInterrupted为true）
	- 实例方法 isInterrupted 则不会（传入的参数 ClearInterrupted 为 false）
	- 他们底层实际都是调用了本地方法 `isInterrupted` 。

![](imgs/Pasted%20image%2020230722165052.png)

**总结**
- ` public void interrupt () ` 是一个实例方法，它通知目标线程中断，也仅仅是设置目标线程的中断标志位为 true
- `  public boolean isInterrupted()` 是一个实例方法，它判断当前线程是否被中断（通过检查中断标志位）并获取中断标志
- `public static boolean interrupted ()` 是一个静态方法，返回当前线程的中断真实状态（boolean 类型）后会将当前线程的中断状态设为 false，此方法调用之后会清楚当前线程的中断标志位的状态（将中断标志置为 false 了），返回当前值并清零置为 false。

# LockSupport
LockSupport 是用来创建锁和其他同步类的基本线程阻塞原语，其中 `park ()` 和 `unpack ()` 而作用分别是**阻塞线程和解除阻塞线程.** 

## 三种让线程等待和唤醒的方法
- 方式一：使用 Object 中的 wait ()方法让线程等待，使用 Object 中的 notify ()方法唤醒线程
- 方式二：使用JUC包中的Condition的await()方法让线程等待，使用signal()方法唤醒线程
- 方式三：LockSupport 类可以阻塞当前线程以及唤醒指定被阻塞的线程

**为什么会有 LockSupport 的出现？他解决了那些痛点和难题？** 思考

### Object 类中的 wait 和 notify 方法实现线程等待和唤醒
`wait` 和 `notify` 必须放在同步代码块中使用，却成对出现。
必须先 `wait` 再 `notify` 否则，无法唤起线程。
Demo：
```java
public class LockSupportTest {  
  
  
public static void main(String[] args) {  
// 此时如果在线程还没有wait的状态下去释放，会导致线程一致堵塞。  
Object o1 = new Object();  
new Thread(() -> {  
synchronized (o1) {  
System.out.println("t1 come in");  
try {  
System.out.println("t1 stop---");  
o1.wait();  
} catch (InterruptedException e) {  
throw new RuntimeException(e);  
}  
  
}  
}).start();  
  
CompletableFuture.runAsync(() ->  
{  
try {  
TimeUnit.SECONDS.sleep(1);  
} catch (InterruptedException e) {  
throw new RuntimeException(e);  
}  
synchronized (o1) {  
o1.notify();  
System.out.println("释放t1");  
}  
});  
}  
}
```

### Condition 接口中的 await 和 signal 方法实现线程的等待和唤醒
- Condition 中的线程等待和唤醒方法，需要先获取锁。
- 一定要先 await 后 signal，不要反了
```java
public class LockSupportDemo {  
  
public static void main(String[] args) {  
Lock lock = new ReentrantLock();  
Condition condition = lock.newCondition();  
/**  
* t1 -----------come in  
* t2 -----------发出通知  
* t1 -----------被唤醒  
*/  
new Thread(() -> {  
//获得锁
lock.lock();  
try {  
System.out.println(Thread.currentThread().getName() + "\t -----------come in");  
condition.await();  
System.out.println(Thread.currentThread().getName() + "\t -----------被唤醒");  
} catch (InterruptedException e) {  
e.printStackTrace();  
} finally {  
//释放锁
lock.unlock();  
}  
}, "t1").start();  
  
try {  
TimeUnit.SECONDS.sleep(1);  
} catch (InterruptedException e) {  
e.printStackTrace();  
}  
  
new Thread(() -> {  
lock.lock();  
try {  
condition.signal();  
System.out.println(Thread.currentThread().getName() + "\t -----------发出通知");  
} finally {  
lock.unlock();  
}  
}, "t2").start();  
}  
}
```

### 上述两个对象 Object 和 Condition 使用的限制条件
- 线程需要先获得并持有锁，必须在锁块（synchronized 或 lock）中
- 必须要先等待后唤醒，线程才能够被唤醒

### LockSupport 类中的 park 等待和 unpark 唤醒
- LockSupport 类使用了一种名为 `Permit`（许可）的概念来做到阻塞和唤醒线程的功能，每个线程都有一个许可（Permit），**许可证只能有一个，累加上限是1**。 

- 主要方法
	
	- 阻塞: `Peimit` 许可证默认没有不能放行，所以一开始调用 park ()方法当前线程会阻塞，直到别的线程给当前线程发放 peimit，park 方法才会被唤醒。 
	- 
	-` park/park (Object blocker)`-------阻塞当前线程/阻塞传入的具体线程 
	
	- 唤醒: 调用 unpack (thread)方法后就会将 thread 线程的许可证 peimit 发放，会自动唤醒 park 线程，即之前阻塞中的 LockSupport. park ()方法会立即返回。 
	- 
	- `unpark (Thread thread)` ------唤醒处于阻塞状态的指定线程 

```java
public class LockSupportTest2 {  
public static void main(String[] args) throws InterruptedException {  
Thread t1 = new Thread(() -> {  
System.out.println("t1");  
LockSupport.park();  
System.out.println("t2 end");  
});  
t1.start();  
TimeUnit.SECONDS.sleep(1);  
  
new Thread(() -> {  
System.out.println("t2");  
LockSupport.unpark(t1);  
System.out.println("释放t1");  
}).start();  
}  
}
```

重点说明：
	- **LockSupport 是用来创建锁和其他同步类的基本线程阻塞原语**，所有的方法都是静态方法，可以让线程再任意位置阻塞，阻塞后也有对应的唤醒方法。归根结底，LockSupport 时调用 Unsafe 中的 native 代码。
	-  `LockSupport` 提供 `park ()` 和 `unpark ()` 方法实现阻塞线程和解除线程阻塞的过程，LockSupport 和每个使用它的线程都有一个许可（Peimit）关联，每个线程都有一个相关的 permit，**peimit 最多只有一个**，**重复调用 unpark 也不会积累凭证。** 但是解除堵塞会消耗凭证。
	- 形象理解：线程阻塞需要消耗凭证（Permit），这个凭证最多只有一个
	- 当调用 park 时，如果有凭证，则会直接消耗掉这个凭证然后正常退出。如果没有凭证，则必须阻塞等待凭证可用；
	- 当调用 unpark 时，它会增加一个凭证，但凭证最多只能有 1 各，累加无效.

- 面试题 #面试题 

	**- 为什么LockSupport可以突破wait/notify的原有调用顺序？**
	
	- 因为unpark获得了一个凭证，之后再调用park方法，就可以名正言顺的凭证消费，故不会阻塞，先发放了凭证后续可以畅通无阻。
	
	- **为什么唤醒两次后阻塞两次，但最终结果还会阻塞线程？**
	
	- 因为凭证的数量最多为1，连续调用两次unpark和调用一次unpark效果一样，只会增加一个凭证，而调用两次park却需要消费两个凭证，证不够，不能放行。

# 计算机硬件存储体系
![](imgs/Pasted%20image%2020230722185259.png)
CPU 的运行**并不是直接操作内存而是先把内存里面的数据读到缓存**，而内存的读和写操作的时候会造成不一致的问题。JVM 规范中试图定义一种 Java 内存模型来屏蔽掉各种硬件和操作系统的内存访问差异，**以实现让 Java 程序再各种平台下都能达到一致性的内存访问效果。** 

# Java 内存模型 Java Memory Model
JMM（Java 内存模型 Java Memory Model）本身是一种**抽象的概念并不真实存在，它仅仅描述的是一组约定或规范**，通过这组规范定义了程序中（尤其是多线程）各个变量的读写访问方式并决定一个线程对共享变量的写入以及如何变成对另一个线程可见，**关键技术点都是围绕多线程的原子性、可见性和有序性展开的。** 
JMM 主要关注以下问题：
	- 原子性（Atomicity）：对于一些操作，要么全部执行成功，要么全部不执行，不存在中间状态。
	- 可见性（Visibility）：一个线程对共享变量的修改对其他线程是可见的。
	- 有序性（Ordering）：程序执行的结果是按照代码的顺序来保证的，即指令重排不会影响到单线程的执行结果。

**能干吗？**
- 通过 JMM 来实现**线程和主内存之间的抽象关系**
- 屏蔽各个**硬件平台和操作系统的内存访问差异以实现让 Java 程序再各种平台下都能达到一致性的内存访问效果。**

# JMM 规范下三大特性
- **可见性**：是指当一个线程修改了某一个共享变量的值，其他线程是否能够立即知道该变更，JMM 规定了所有的变量都存储在主内存中。
- ![](imgs/Pasted%20image%2020230722202029.png)

系统中**主内存共享变量**数据修改被写入的时机是不确定的，多线程并发下很可能出现“脏读”，所以**每个线程都有自己的工作内存**，线程自己的工作内存中保存了该线程使用到的变量的**主内存副本拷贝**，线程对变量的所有**操作（读取、赋值等）都必须在线程自己的工作内存中进行，而不能够直接写入主内存中的变量**，不同线程之间也无法直接访问对方工作内存中的变量，线程间变量值的传递均需要通过主内存来完成。

线程脏读

```
主内存中有变量X，初始值为0
线程 A 要将 X 加1，先将 X=0拷贝到自己的私有内存中，然后更新 X 的值
线程 A 将更新后的 X 值回刷到主内存的时间是不固定的 
刚好在线程 A 没有回刷 x 到主内存时，线程 B 同样从主内存中读取 X，此时为0，和线程 A 一样的操作，最后期盼的 X=2就会变成 X=1
```

- **原子性**：指一个操作是不可被打断的，即多线程环境下，操作不能被其他线程干扰。

- **有序性**：对于一个线程的执行代码而言，我们总是习惯性地认为代码的执行总是从上到下，有序执行。*但为了提升性能，编译器和处理器通常会对指令序列进行重新排序。* Java 规范规定 JVM 线程内部维持顺序化语义，*即只要程序的最终结果与它顺序话执行的结果相等，那么指令的执行顺序可以与代码顺序不一致，* 此过程叫指令的重排序。 

- 优缺点：
	
	- JVM能根据处理器特性（CPU多级缓存系统、多核处理器等）适当的对机器指令进行重排序，使机器指令更符合CPU的执行特性，最大限度的发挥机器性能。
	- 但是指令重排可以保证**串行语义一致**，但没有义务**保证多线程的语义也一致**（即可能产生“脏读”），简单而言就是两行以上不相干的代码在执行的时候有可能先执行的不是第一条，**不见得是从上到下顺序执行**，执行顺序会被优化。

- 从源码到最终执行示例图：
![](imgs/Pasted%20image%2020230722204457.png)

- 单线程环境里确实能够保证程序最终执行结果和代码顺序执行的结果一致 
- 处理器在进行重排序时**必须考虑到**指令之间的**数据依赖性**
- 多线程环境中线程交替执行，由于编译器优化重排的存在，可能出现乱序现象，两个线程使用的变量能否保证一致性是无法确定的，结果无法预测。

# JMM 规范下多线程对变量的读写过程
由于 JVM 运行程序的实体是线程，而每个线程创建时 JVM 都会为其创建一个**工作内存（有的地方成为栈空间）**，工作内存是每个线程的私有数据区域，而 Java 内存模型中**规定所有变量都存储在主内存**，主内存是共享内存区域，所有线程都可以访问，但线程对变量的操作（读写赋值等）**必须在工作内存中进行**，
首先要将变量从主内存拷贝到线程自己的工作内存空间，然后对变量进行操作，操作完成后再将变量写回主内存，**不能直接操作主内存中的变量**，各个线程中的工作内存存储着主内存中的**变量副本拷贝**，因此**不同的线程无法访问对方的工作内存**，线程间的通信（传值）必须通过主内存来完成，其简要访问过程如下图： 
![](imgs/Pasted%20image%2020230722204653.png)

JMM 定义了线程和主内存之间的抽象关系：

- 线程之间的共享变量存储在主内存中（从硬件角度讲就是内存条）
- 每个线程都有一个自己的本地工作内存，本地工作内存中存储了该线程用来读写共享变量的副本（从硬件角度来说就是 CPU 的缓存） 

小总结：
	- 我们定义的所有共享变量都储存在**物理主内存中**
	- 每个线程都有自己独立的工作内存，里面保证该线程使用到的共享变量的副本（主内存中该变量的一份拷贝）
	- 线程对共享变量所有的操作都必须先在线程自己的工作内存中进行后写回主内存，不能直接从主内存在读写（不能越级）
	- 不同线程之间也无法直接访问其他线程的工作内存中的变量，线程间变量值的传递需要通过主内存来进行（同级不能互相访问）。

# JMM 规范下多线程先行发生原则之 happens-before






