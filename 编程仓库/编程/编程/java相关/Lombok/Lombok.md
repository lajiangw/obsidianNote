# 常用注解
下面介绍一下常用的几个注解：
@Setter 注解在类或字段，注解在类时为所有字段生成 setter 方法，注解在字段上时只为该字段生成 setter 方法。
@Getter 使用方法同上，区别在于生成的是 getter 方法。
@ToString 注解在类，添加 toString 方法。
@EqualsAndHashCode 注解在类，生成 hashCode 和 equals 方法。
@NoArgsConstructor 注解在类，生成无参的构造方法。
@RequiredArgsConstructor 注解在类，为类中需要特殊处理的字段生成构造方法，比如 final 和被@NonNull 注解的字段。
@AllArgsConstructor 注解在类，生成包含类中所有字段的构造方法。
@Data 注解在类，生成 setter/getter、equals、canEqual、hashCode、toString 方法，如为 final 属性，则不会为该属性生成 setter 方法。
@Slf4j 注解在类，生成 log 变量，严格意义来说是常量。private static final Logger log = LoggerFactory. getLogger (UserController. class);


## **一、@Getter and @Setter**

使用`@Getter`和/或`@Setter`注释任何字段，以使`lombok`自动生成默认的`getter / setter`。

默认的`getter`只是返回该字段，如果该字段被称为`foo`，则名为`getFoo`（如果该字段的类型为`boolean`，则为`isFoo`）。

默认生成的 `getter / setter`方法是公共的，除非你明确指定一个`AccessLevel`。合法访问级别为`PUBLIC，PROTECTED，PACKAGE和PRIVATE`。

你还可以在类上添加`@Getter`和/或`@Setter`注释。在这种情况下，就好像你使用该注释来注释该类中的所有非静态字段一样。

你始终可以使用特殊的 `AccessLevel.NONE` 访问级别来手动禁用任何字段的 `getter / setter` 生成。这使你可以覆盖类上的 `@Getter，@Setter或@Data` 注释的行为。

## **二、@ToString**

任何类定义都可以使用`@ToString`注释，以使`lombok`生成`toString()`方法的实现。

默认情况下，将打印所有非静态字段。如果要跳过某些字段，可以使用`@ ToString.Exclude`注释这些字段。或者，可以通过使用`@ToString（onlyExplicitlyIncluded = true）`，然后使用`@ToString.Include`标记要包含的每个字段，来确切指定希望使用的字段。

通过将 `callSuper` 设置为 `true`，可以将 `toString` 的超类实现的输出包含到输出中。请注意，`java.lang.Object中toString()` 的默认实现几乎毫无意义

## **三、@EqualsAndHashCode**

任何类定义都可以使用`@EqualsAndHashCode`进行注释，以使`lombok`生成`equals(Object other)`和`hashCode()`方法的实现。默认情况下，它将使用所有非静态，非瞬态字段，但是您可以通过使用`@EqualsAndHashCode.Include`标记类型成员来修改使用哪些字段（甚至指定要使用各种方法的输出）。 `@EqualsAndHashCode.Exclude`。或者，可以通过使用@ `EqualsAndHashCode.Include`标记并使用`@EqualsAndHashCode(onlyExplicitlyIncluded = true)`来精确指定要使用的字段或方法。

如果将 `@EqualsAndHashCode` 应用于扩展另一个类的类，则此功能会有些棘手。通常，为此类自动生成 `equals` 和 `hashCode` 方法是一个坏主意，因为超类还定义了字段，该字段也需要 `equals / hashCode` 代码，但不会生成此代码。通过将 `callSuper` 设置为 `true`，可以在生成的方法中包括超类的 `equals` 和 `hashCode` 方法。

## **四、@AllArgsConstructor, @RequiredArgsConstructor and @NoArgsConstructor**

`@NoArgsConstructor`将生成没有参数的构造函数。如果字段由final修饰，则将导致编译器错误，除非使用`@NoArgsConstructor(force = true)`，否则所有final字段都将初始化为`0 / false / null`。对于具有约束的字段(例如`@NonNull`字段)，不会生成任何检查。

`@RequiredArgsConstructor`为每个需要特殊处理的字段生成一个带有1个参数的构造函数。所有未初始化的final字段都会获取一个参数，以及所有未声明其位置的未标记为`@NonNull`的字段。

`@AllArgsConstructor` 为类中的每个字段生成一个带有 1 个参数的构造函数。标有 `@NonNull` 的字段将对这些参数进行空检查。

## **五、@Data**

`@Data` 是一个方便的快捷方式批注，它将 `@ToString`，`@EqualsAndHashCode`，`@ Getter / @Setter` 和 `@RequiredArgsConstructor` 的功能捆绑在一起：换句话说，`@Data` 生成通常与简单 `POJO` 关联的所有样板（普通的旧 `Java` 对象）和 `bean` ：所有字段的 `getter`，所有 `非final` 字段的 `setter`，以及涉及类字段的适当的 `toString`，`equals` 和 `hashCode` 实现，以及初始化所有 `final` 字段以及所有 `非final` 字段的构造函数没有使用 `@NonNull` 标记的初始化程序，以确保该字段永远不会为 `null`。

## **六、@Value**

@Value注解和`@Data`类似，区别在于它会把所有成员变量默认定义为`private final`修饰，并且不会生成`set`方法。

## **七、@Builder**

构建者模式
只能标注到类上，将生成类的一个当前流程的一种链式构造工厂，如下：
```javascript
User buildUser = User.builder().username("riemann").password("123").build();
```

可配合 `@Singular` 注解使用，`@Singular` 注解使用在 jdk 内部集合类型的属性，`Map` 类型的属性以及 `Guava` 的 `com.google.common.collect` 的属性上。例如未标注 `@Singular` 的属性，一般 `setter` 时，会直接覆盖原来的引用，标注了 `@Singular` 的属性，集合属性支持添加操作，会在属性原来的基础上增加。

## **八、@Accessors**

链式风格
`@Accessors`批注用于配置`lombok`如何生成和查找`getter`和`setter`。
默认情况下，`lombok`遵循针对`getter`和`setter`的`bean`规范：例如，名为`Pepper`的字段的`getter`是`getPepper`。但是，有些人可能希望打破`bean`规范，以得到更好看的`API`。 `@Accessors`允许您执行此操作。

可标注在类或属性上，当然最实用的功能还是标注到类上。





## **九、@Slf4j and @Log4j**

在需要打印日志的类中使用，项目中使用`slf4j`、`log4j`日志框架

## **十、@NonNull**

该注解快速判断是否为空,为空抛出`java.lang.NullPointerException`。

## **十一、@Synchronized**

注解自动添加到同步机制，生成的代码并不是直接锁方法,而是锁代码块， 作用范围是方法上。

## **十二、@Cleanup**

注解用于确保已分配的资源被释放（`IO`的连接关闭）。