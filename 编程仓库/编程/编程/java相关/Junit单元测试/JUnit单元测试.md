# Junit 设计包括
![[imgs/Pasted image 20230317124708.png]]


# ASsert 断言
基本断言操作
![[imgs/Pasted image 20230317131359.png]]
**浮点断言需要给到给到一个误差值。**

![[imgs/Pasted image 20230317131505.png]]


	# Junit 中 Befter 和 After
@BeforeClass 会在所有的 @Test方法之前执行。
@AfterClass 会在所有的@Test 方法结束后执行
**使用这两个注解的方法必须是静态方法。**

类的方法还有继承 TestCase 类，重写 SetUp 方法和 tearDown 方法实现。
![[imgs/Pasted image 20230317155321.png]]

Junit 的执行逻辑
![[imgs/Pasted image 20230317155928.png]]


一些使用逻辑
![[imgs/Pasted image 20230317160009.png]]


# Junit 异常测试
异常测试在@Test 中添加参数 expected，属性的值为异常类的. class。 

```java
@Test(expected = ArithmeticException.class)
	public void test() {
			int a = testExcept.test1(5, 0);
			System.out.println(Objects.isNull(testExcept)+"123");
			System.out.println();
	}
```




# 超时测试
在 @Test 参数中添加 timeout 属性为测试方法的执行时间。单位是毫秒


# Junit 常用注解
| 注解        | 说明                                                                   |
| ----------- | ---------------------------------------------------------------------- |
| @Test       | 标识一条测试用例。 (A) (expected=XXEception.class)   (B) (timeout=xxx) |
| @Ignore   | 忽略的测试用例。                                                       |
| @Before     | 每一个测试方法之前运行。                                               |
| @After      | 每一个测试方法之后运行                                                 |
| @BefreClass | 所有测试开始之前运行。                                                 |
| @AfterClass    | 所有测试结果之后运行。                                                                       |


# FixTrue
Test Fixture 是指一个测试运行所需的固定环境，准确的定义：
在进行测试时，我们通常需要把环境设置成已知状态（如创建对象、获取资源等）来创建测试，每次测试开始时都处于一个固定的初始状态；测试结果后需要将测试状态还原，所以，测试执行所需要的固定环境称为 Test Fixture。 
```java
1.  public class TestFixture {
    

3.  //在当前测试类开始时运行。
    
4.  @BeforeClass
    
5.  public static void beforeClass(){
    
6.  System.out.println("-------------------beforeClass");
    
7.  }
    

9.  //在当前测试类结束时运行。
    
10.  @AfterClass
    
11.  public static void afterClass(){
    
12.  System.out.println("-------------------afterClass");
    
13.  }
    

15.  //每个测试方法运行之前运行
    
16.  @Before
    
17.  public void before(){
    
18.  System.out.println("=====before");
    
19.  }
    

21.  //每个测试方法运行之后运行
    
22.  @After
    
23.  public void after(){
    
24.  System.out.println("=====after");
    
25.  }
    

27.  @Test
    
28.  public void testAdd1() {
    
29.  int result=new Count().add(5,3);
    
30.  assertEquals(8,result);
    
31.  System.out.println("test Run testadd1");
    
32.  }
    

34.  @Test
    
35.  public void testAdd2() {
    
36.  int result=new Count().add(15,13);
    
37.  assertEquals(28,result);
    
38.  System.out.println("test Run testadd2");
    
39.  }
    
41.  }
```

#  JUnit 用例执行顺序

Junt 的执行顺序是通过@FixMethodOrder 注解控制的。
@FixMethodOrder 注解有有有三个选项
```
  MethodSorters.JVM
按照JVM得到的方法顺序，也就是代码中定义的方法顺序


MethodSorters.DEFAULT(默认的顺序)
Sorts the test methods in a deterministic, but not predictable, order() (以确定但不可预期的顺序执行)


MethodSorters.NAME_ASCENDING
按方法名字母顺序执行

```

# JUnit 断言方法
JUnit 所提供的断言方法:
![[imgs/Pasted image 20230317180302.png]]

## Junit 所有基本断言方法。
AssertThat 断言匹配器
- Both () 只有里面的所有条件都符合时，测试通过，最少两个匹配器。
```java
assertThat(testTome,both(is(testTome1)).and(is(testTome1)).and(is(testTome2)));
```

- assertEquals （） 判断两个值是否相等，如果判断的是浮点数，需要给到第三个参数指定差值，如果缺省，则为无限大。可以传入数值和对象。第一个参数可以给到 String 类型的提示信息。
```java
assertEquals("测试错误","te1","te");
assertEquals(0, 0);
assertEquals(test13, test12);
assertEquals(0.3, 0.2,0.1);
```

 Fail（） 无论条件如何，直接是测试用例失败
```java
fail();
```

- assertFalse（） 判断结果是否为 False，如果为 Flase 测试用例通过，反之亦然，可以给到 String 类型的提示信息。

```java
assertFalse("测试错误", true);
```

assertNotNull（） 判断对线是否为空，可以给到 String 类型的字符串提醒
```java
assertNotNull("对象为空", test13);
```

assertSame（） 判断两个对象地址是否相同。可以给到 String 类型的提示
```java
assertSame("对象不同",test12, test1);
```

assertNull（） 判断对象是否为空，为空则通过。
```java
assertNull("对象应该为空", test1);
```

assertSame（） 判断两个对象是否相同，相同则通过
```java
assertSame("对象应该相同",test12, test1);
```


assertArrayEquals（） 判断两个数组对象是否相等。
```java
int[] arr = new int[] {1,2,3};
		int[] arr2 = new int[] {1,2,3};
		assertArrayEquals("数组相等",arr, arr2);
```

## AssertThat 方法中的断言
allOf（） 当选项全部成立则测试通过。
```java
assertThat("不通过","123",allOf(endsWith("3"),startsWith("1")));
```

Anyof（）当任意条件成立时测试用例通过
```java
assertThat("错误","123",anyOf(endsWith("1"),startsWith("1")) );
```

Both ()  当两个条件都成立时测试通过
```java
assertThat("错误", "123",both(endsWith("3")).and(startsWith("2")));
```

either（） 任意条件符合成立测试通过
```java
assertThat("fan", either(containsString("1")).or(containsString("f")));
```

#未解决 describedAs （）？ 有待考究
```java
describedAs("1 2 3 4", equalTo(myBigDecimal), null)
```


everyItem（）会遍历所有集合中的元素，进行匹配，只有都满足才会测试通过
```java
assertThat(Arrays.asList("cbar", "cbaz"), everyItem(anyOf(containsString("w"),containsString("b"))));
```

Is ()  和 equaseTo 方法相同，判断被测值是否和期望值相同则通过。
```java
Test1 test1 = new Test1();
	Test1 test12 = null;
	test1 = null;
	assertThat("失败",test1 ,is(test12));
	assertThat("失败", 1.22,is(1.22));
```

anything（） 不管结局如何，测试同理都是通过，给到的字符串信息是备注有用的。
```java
assertThat("12", test1,anything("通过"));
```





EqualTo（） 当被测值和期望值相同时则测试通过。
```java
	assertThat(1, equalTo(1));
```

any（） 检查一个对象是否是这个类的实例，是则通过
```java
assertThat(test1, any(Test1.class));
```

instanceOf（） 检查一个对象是否是这个类的实例，是则通过
```java
assertThat(test1, instanceOf(Test1.class));
```


not（） 包装一个匹配器，但是反转里面的选项
```java
assertThat("123", not( containsString("2")));
```

nullValue（） 当一个对象为空。则通过
```java
assertThat(test1, is(nullValue());
```

notNullValue（） 当一个对象不为空则通过
```java
assertThat(test1, notNullValue());
```

theInstance（） 当一个对象和另一个对象是相同实例则通过
```java
assertThat(test1, sameInstance(test1));
```

## 字符串相关
containsString（） 判断一个字符串是否包含预期值，有则通过。
```java
assertThat("myStringOfNote", containsString("ring"));
```

startsWith（） 字符串是以预期值开头则通过
```java
assertThat("myStringOfNote", startsWith("my"))
```

endsWith () 字符串是以预期值结尾则通过
```java
assertThat("myStringOfNote", endsWith("Note"))
```

equalToIgnoringWhiteSpace（） 在忽略头尾的空格并且和预期值匹配则通过
```java
assertThat("123",equalToIgnoringWhiteSpace("123"));
```

isEmptyString（） 检查的字符串长度等于 0 则通过
```java
assertThat("", isEmptyString())
```

equalToIgnoringCase匹配符表明如果**测试的字符串在忽略大小写的情况下等于**则测试通过
```java
assertThat(“Foo”, equalToIgnoringCase(“FOO”))
```
## 数组相关

HasItem（）匹配符表明如果测试的**迭代对象含有元素符合**则测试通过
```java
assertThat("123",Arrays.asList("1","2","3"),hasItem("1"));
```

HasItems () 表示如果集合中有至少符合一个的元素，测试通过
```java
assertThat("123",Arrays.asList("1","2","3"), hasItems("1","2"));
```


HasEntry（） 遍历 map 集合中元素，如果有符合要求的元素则通过，并停止遍历
```java
HashMap<String, String> hashMap1 = new HashMap<>();  
hashMap1.put("bar", "foo");  
hashMap1.put("2", "3");  
assertThat(hashMap1, hasEntry("bar", "foo"));
```
array（） 当数组中每个元素都匹配时则通过。
```java
assertThat(new Integer[]{1,2,3}, is(array(equalTo(1), equalTo(2), equalTo(3))));
```

hasItemInArray（） 遍历数组中的每个元素，但有一个元素符合时，则测试通过，
```java
assertThat(new String[]{"1", "2"}, hasItemInArray(("1")));
//hasItemInArray(("1"))相当于 hasItemInArray((equalTo("1")))
assertThat(new String[]{"foo", "bar"}, hasItemInArray(startsWith("ba")));
```

#未解决 arrayContaining (） 遍历集合中的元素，符合则通过
```java
assertThat(new String[]{"foo", "bar"}, 
				contains(Arrays.asList(equalTo("foo"), equalTo("bar"))));
				
```

#未解决  arrayContainingInAnyOrder（） 遍历结合元素符合则通过
```java
assertThat(new String[]{"foo", "bar"}, containsInAnyOrder("bar", "foo"))
```

arrayWithSize（） 如果数组长度符合预期值，则通过。只能是引用类型。
```java
assertThat(new Integer[] {1,2}, arrayWithSize(2));
```

emptyArray（） 数组为长度为零则通过，引用类型
```java
assertThat(new Integer[] {}, emptyArray());
```

hasSize（） 判断一个实现 Collection 接口的集合长度，符合则通过
```java
assertThat(Arrays.asList(1,2), hasSize(2));
```

#未解决 empty（）
```java
assertThat(Arrays.asList("1,2"), is(empty()));
```

contains（） 创建匹配器遍历，只能填入实现 Iterable 的类。符合则通过
```java
assertThat(Arrays.asList("foo", "bar"), contains("foo", "bar"));
```

iterableWithSize（） 检查实现 Iterable 的类的长度是否符合
```java
assertThat(Arrays.asList("foo", "bar"), iterableWithSize(2))
```

hasKey（） 检查实现 map 接口的结合有没有符合的 key
```java
HashMap<String,Integer> hashMap = new HashMap<>();  
  
hashMap.put("1", 1);  
assertThat(hashMap, hasKey("1"));
```

hasValue（） 检查实现 map 接口的结合有没有符合的 value
```java
HashMap<String,Integer> hashMap = new HashMap<>();  
hashMap.put("1", 1);  
assertThat(hashMap,hasValue(1));
```

isIn () 检查有没有结合中相同的元素，有则通过
isOneOf（） 类似功能
```java
TestTome testTome = new TestTome();  
assertThat(testTome,isIn(Arrays.asList(testTome)));
```

## 数值相关
closeTo（）判断 double 是否和预期值相同，或者小于误差值。
comparesEqualTo（） 方法类似，可以匹配其他类型
```java
assertThat(1.5,closeTo(1.5,0));

assertThat(1.5,comparesEqualTo(1.5));
```

GreaterThan（） 当判断的值大于预期值则通过
```java
assertThat(124,greaterThan(123));
```

GreaterThanOrEqualTo ())当判断的值大于等于预期值则通过
```java
assertThat(1.5,greaterThanOrEqualTo(1.5));
```


LessThan 匹配符表明如果所**测试的数值小于**预期值则测试通过
```java
assertThat(1, lessThan(2))
```


LessThanOrEqualTo（）匹配符表明如果所**测试的数值小于**等于预期值则测试通过
```java
assertThat(1, lessThanOrEqualTo(1))
```



# JUnit 之 Rule
JUnit Rule 的两个注解需要用于实现了 `TestRule` 或 `MethodRule` 接口的成员变量 (`@Rule`)或静态变量 (`@ClassRule`)上。

-   `@Rule`注解是方法级别的，每个测试方法执行时都会执行被`@Rule`注解的成员变量的方法（类似于`@Before`）。
-   `@ClassRule`注解是类级别的，测试类执行时仅会执行一次被`@ClassRule`注解的静态变量的方法（类似于`@BeforeClass`）。

## JUnit Rule和@Before注解的区别

  **`@Before` 注解的方法只能作用于当前测试类及其子类，而实现了 `TestRule` 的类可以被用于多个测试类，因此 JUnit Rule 可以降低代码重复度并且更灵活。**

-   ExpectedException：支持在测试中指定期望抛出的异常类型，类似于 `@Test(expected = xxxException.class)` 。
-   ExternalResource：支持提前建立好资源并且会在测试结束后将其销毁，这对于使用Socket、数据库链接等资源的测试来说很有用。
-   TemporaryFolder：支持创建文件与目录并且在测试运行结束后将其删除，这对于使用文件系统且独立运行的测试来说很有用。
-   TestName：支持在测试中获取当前测试方法的名称。
-   TestWatcher：提供五个触发点：`测试开始、测试完成、测试成功、测试跳过、测试失败`，允许我们在每个触发点执行自定义的逻辑。
-   Timeout：支持为测试类中的所有测试方法设置相同的超时时间，类似于 `@Test(timeout = 100)`。


## TemporaryFolder
在测试用例中创建一个临时文件夹，测试用例无论失败还是通过都会删除文件。
例子:
```java
 @Test  
    public void testTemporaryFolder() throws IOException {  
        File file = temporaryFolder.newFile("123.txt");  
//        temporaryFolder.newFolder("myfile");  
        FileWriter fileWriter = new FileWriter(file);  
        fileWriter.write("123");  
        fileWriter.flush();  
        System.out.println("123");  
        fileWriter.close();  
    }
```

严格验证是否已经删除文件，使用 builder 构造和 assureDeletion 方法。
```java
public TemporaryFolder folder = TemporaryFolder.builder().parentFolder(file).assureDeletion().build();
```

类中方法：
parentFolder（） 用来创建指定临时文件夹的路径。

---
## ExternalResource
它在测试之前设置外部资源（文件、[套接字](https://so.csdn.net/so/search?q=%E5%A5%97%E6%8E%A5%E5%AD%97&spm=1001.2101.3001.7020)、服务器、数据库连接等），并保证之后将其拆除，可以理解为 after 和 before 注解
例子： 
```java
@Rule  
public ExternalResource ex = new ExternalResource() {  
    @Override  
    protected void before() throws Throwable {  
        System.out.println("before");  
    }  
  
    @Override  
    protected void after() {  
        System.out.println("after");  
    }  
};  
  
@Test  
public  void  test3(){  
    System.out.println("test3");  
}
```
---

## TestName
获得当前测试方法的方法名
```java
@Test  
public void testName(){  
assertThat("testName",is(testName.getMethodName()));  
}
```


## 自定义 Rule

自定义的 Rule 只需实现 TestRule 接口，并实现其中 apply ()方法即可，该方法返回一个 Statement 对象。  
下面展示一个用于循环调用测试方法的Rule，该类中的`base.evaluate();`用于继续运行测试方法。

  ```java
import org.junit.rules.TestRule;
import org.junit.runner.Description;
import org.junit.runners.model.Statement;

public class LoopRule implements TestRule {
    private int loopCount;

    public LoopRule(int loopCount) {
        this.loopCount = loopCount;
    }

    @Override
    public Statement apply(Statement base, Description desc) {
        return new Statement() {
            @Override
            public void evaluate() throws Throwable {
                for (int i = 1; i <= loopCount; i++) {
                    System.out.println("Loop " + i + " started");
                    base.evaluate();
                    System.out.println("Loop " + i + " finished\n----------------");
                }
            }
        };
    }
}
```

# Runner
一个 JUnit Runner 是一个继承了 JUnit 的抽象类 `Runner` 的类. Runners 是用来运行测试类的。可以通过 `@RunWith` 注解来指定一个 Runner 去运行测试。


JUnit 测试是通过使用类 `JUnitCore` 开始的。可以通过[命令行](https://stackoverflow.com/questions/2235276/how-to-run-junit-test-cases-from-the-command-line)或者使用它的其中一个 run ()方法来运行测试（IDE 的 run 按钮运行测试就是通过此方法实现的） 
```java
JUnitCore.runClasses(MyTestClass.class);
```

## Junit 参数化 Parameterized
当我们们有多项参数时候，Junit 会为我们自动将参数填充进行测试。

1. 我们需要在类上面，添加注解@RunWith (Parameterized. Class)
2. 构造方法的参数要和需要填充的参数一致
3. 必须使用静态方法 data。方法名必须是 data。方法的返回值必须是实现 Collection 的接口类。

```java
@RunWith(Parameterized.class)
public class TestAbc {
	int age;
	int b;

	@Parameters
	public static Collection<?> data() {
		return Arrays.asList(new Object[][] {{0,1},{2,3}});
	}
	
	public TestAbc(int age,int b) {
		// TODO Auto-generated constructor stub
		this.age = age;
		this.b = b;
	}
	
	@Test
	public void test() {
		System.out.println(this.age);
		System.out.println(this.b);
		System.out.println(age);
		System.out.println(b);
		}
	}

```

## 测试套件
测试套件就是将一个空类设置为启动器，传入注解的类里面标识 Test 的方法都将执行。

1.  创建一个空类作为测试套件的入口；
    
2.  使用注解 **org. Junit. Runner. RunWith** 和 **org. Junit. Runners. Suite. SuitClasses** 修饰这个空类。
    
3.  将 **org. Junit. Runners. Suite** 作为参数传入给注解 **RunWith**，以提示 JUnit 为此类测试使用套件运行器执行。
    
4.  将需要放入此测试套件的测试类组成数组作为注解 SuiteClasses 的参数。
    
5.  保证这个空类使用 public 修饰，而且存在公开的不带任何参数的构造函数。

```java
package com.zml;

import org.junit.runner.RunWith;
import org.junit.runners.Suite;
import org.junit.runners.Suite.SuiteClasses;

@RunWith(Suite.class)
@SuiteClasses({
	TimeOutTest.class,
	TestAssert.class,
	Test2.class
})
public class runAllTest {

}

```

## Category 注解
Category 相当于把测试用例分类，然后在通过统一的运行类进行执行，相当于是 Suite 的高级版。
**@Category 可以放在类上面，也可以放在方法上面，看你给到的具体分组如何。**
例子：
首先创建两个测试用例类，在测试用例类上面添加@Category 里面传入分组的类 class。
```java
@Category(ATest.class)  
public class Test5 {  
    @Test  
    public void  test(){  
        System.out.println("我是test5方法的测试用例");  
    }  
}
```

这个 @Category 注解放在了方法上面
```java
public class Test4 {  
    @Test  
    @Category(BTest.class)  
    public void test(){  
        System.out.println("我是test4的测试用例");  
    }  
}
```

创建两个组接口，也可以是类。你想分几组就创建几组。
```java
public interface ATest {  
}
```

```java
public interface BTest {  
}
```

创建一个启动类来执行这些分组
```java
//表示此类为Category类  
 @RunWith(Categories.class)  
//要运行的类  
@Suite.SuiteClasses({  
      Test4.class,  
      Test5.class  
          })  
 @Categories.ExcludeCategory(ATest.class)//要运行的用例组  
 @Categories.IncludeCategory(BTest.class)//不要运行的用例组  
public class RunTest {  
  
}
```





# Jinit 测试 private 方法
原理是通过反射爆破访问私有方法
```java
	@Test
	public void test() throws Exception {
//		通过.class 获得class对象
		Class userclas = User.class;
//		通过class 对象调用 指定构造器
		Constructor constructor = userclas.getConstructor(String.class,int.class);
//		给指定构造器指定参数，获得该构造器的对象
		Object user = constructor.newInstance("小明",5);
//		使用class对象获得指定方法名的方法对象，第二个参数传入的是参数类型的class
		Method declaredMethod = userclas.getDeclaredMethod("getAge", int.class);
//		进行反射爆破
		declaredMethod.setAccessible(true);
//		使用获得方法对象调用invoke方法，执行改方法。invoke方法的返回值就是该方法的返回值
		Object invoke = declaredMethod.invoke(user, 15);
		System.out.println(invoke);
	}
```



## Math 数学库常用方法

Abs（） 求绝对值

sqrt () 方法用于返回参数的算术平方根。


# Junit 中的两种错误
## Failure
Failure 指的是测试失败，就是预期的结果与实际运行结果不同而导致的失败。
例如当使用 assertXXX ()方法断言失败时，就会报 Failure。

## Error
Error 指的是测试程序本身出错，代码异常引起的，测试程序中没有考虑到的情况，在断言之前程序就代码异常而终止。 


