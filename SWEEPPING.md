## 数组
- 数组遍历
```java
for (int i=0; i<ns.length; i++) {
    int n = ns[i];
    System.out.println(n);
}
for (int n : ns) {
    System.out.println(n);
}
```
- 打印数组
```java
    // 打印的是地址
    System.out.println(ns); // 类似 [I@7852e922
    // 正确方式
    int[] ns = { 1, 1, 2, 3, 5, 8 };
    System.out.println(Arrays.toString(ns));    
    Arrays.toString(ns) 
    // 打印多维数组可以使用 
    Arrays.deepToString(ns)

```
- 数组排序
```java
    Arrays.sort(ns)
```

## 类和对象实例
- 继承树
  
  Object <-- Person <-- Student
- protected
  
  protected 关键字可以把字段和方法的访问权限控制在继承树内部，一个 protected 字段和方法可以被其子类，以及子类的子类所访问
- super
  
  如果父类没有默认的构造方法，子类就必须显式调用 super() 并给出参数以便让编译器定位到父类的一个合适的构造方法。

```java
class Person {
    protected String name;
    public String hello() {
        return "Hello, " + name;
    }
}
Student extends Person {
    @Override
    public String hello() {
        // 调用父类的hello()方法:
        return super.hello() + "!";
    }
}

```

- 向上转型（upcasting）
```java
    Student s = new Student();
    Person p = s; // upcasting, ok
    Object o1 = p; // upcasting, ok
    Object o2 = s; // upcasting, ok
```

- 向下转型（downcasting）
  
  向下转型很可能会失败。失败的时候，Java虚拟机会报 *ClassCastException*
```java
Person p = new Student();
if (p instanceof Student) {
    // 只有判断成功才会向下转型:
    Student s = (Student) p; // 一定会成功
}
```
*使用 instanceof variable 这种判断并转型为指定类型变量的语法时，必须打开编译器开关 --source 14 和 --enable-preview 。*

- Overload 和 Override
  
  Override和Overload不同的是，如果方法签名如果不同，就是Overload，Overload方法是一个新方法；如果方法签名相同，并且返回值
也相同，就是 Override 。

*注意：：方方法法名名相相同同，，方方法法参参数数相相同同，，但但方方法法返返回回值值不不同同，，也也是是不不同同的的方方法法。。在在Java程程序序中中，，出出现现这这种种情情况况，，编编译译器器会会报报错错。加上 @Override 可以让编译器帮助检查是否进行了正确的覆写。希望进行覆写，但是不小心写错了方法签名，编译器会报错。*

- Polymorphic (多态)
  
  Java的实例方法调用是基于运行时的实际类型的动态调用，而非变量的声明类型。

- Object 的方法override
  
  toString() 把instance输出为 String

  equals() 判断两个instance是否逻辑相等

  hashCode() 计算一个instance的哈希值


```java
class Person {
    ...
    // 显示更有意义的字符串:
    @Override
        public String toString() {
         return "Person:name=" + name;
    }
    // 比较是否相等:
    @Override
    public boolean equals(Object o) {
        // 当且仅当o为Person类型:
        if (o instanceof Person) {
            Person p = (Person) o;
            // 并且name字段相同时，返回true:
            return this.name.equals(p.name);
        }
        return false;
    }
    // 计算hash:
    @Override
        public int hashCode() {
            return this.name.hashCode();
    }
}
```

- final
  - 修饰方法
    
    如果一个父类不允许子类对它的某个方法进行覆写，可以把该方法标记为 final 。
  - 修饰类
    
    不希望任何其他类继承自它，那么可以把这个类本身标记为 final 。
  - 修饰字段
    
    一旦赋值不嫩被修改

- 抽象类
```java
abstract class Person {
    public abstract void run();
}
```
Person 类定义了抽象方法 run() ，那么，在实现子类 Student 的时候，就*必须*覆写 run() 方法：
  - 面向抽象编程的好处
    - 上层代码只定义规范（例如： abstract class Person ）；
    - 不需要子类就可以实现业务逻辑（正常编译）；
    - 具体的业务逻辑由不同的子类实现，调用者并不关心。

- 接口
  
  如果一个抽象类没有字段，所有方法全部都是抽象方法：
```java
interface Person {
    void run();
    String getName();
}
```
在接口中，可以定义 default 方法。例如，把 Person 接口的 run() 方法改为 default 方法：
实现类可以不必覆写 default 方法。 default 方法的目的是，当我们需要给接口新增一个方法时，会涉及到修改全部子类。如果新增的
是 default 方法，那么子类就不必全部修改，只需要在需要覆写的地方去覆写新增方法。
``` java
interface Person {
    String getName();
    default void run() {
        System.out.println(getName() + " run");
    }
}
class Student implements Person {
    private String name;
    public Student(String name) {
        this.name = name;
    }
    public String getName() {
        return this.name;
    }
}
```
虽然不能定义实例字段，但可以定义静态字段，并且静态字段必须为 final 类型：
```java
public interface Person {
    public static final int MALE = 1;
    public static final int FEMALE = 2;
}
```


- 抽象类和接口对比

| |  abstract class  |  interface|
| --- | --- | --- |
|  继承  |      只能extends一个class | 可以implements多个interface|
|  字段       | 可以定义实例字段    |  不能定义实例字段   |
|  抽象方法   |  可以定义抽象方法   |   可以定义抽象方法   |
|  非抽象方法 |  可以定义非抽象方法  |  可以定义default方法   |

- 继承关系
  
  Object <-- AbstractCollection <-- AbstractList <-- ArrayList 
                    |                   |        <-- LinkedList
  Iterable <-- Collection    <--       List

  公共逻辑适合放在 abstract class 中，具体逻辑放到各个子类

  在使用的时候，实例化的对象永远只能是某个具体的子类，但总是通过接口去引用它，因为接口比抽象类更抽象：
  ```java
    List list = new ArrayList(); // 用List接口引用具体子类的实例
    Collection coll = list; // 向上转型为Collection接口
    Iterable it = coll; // 向上转型为Iterable接口
  ```

- 静态字段
 
  不推荐用 实例变量.静态字段 去访问静态字段，因为在Java程序中，实例对象并没有静态字段。在代码中，实例对象能访问静态字
段只是因为编译器可以根据实例类型自动转换为 类名.静态字段 来访问静态对象。

- Public Private Protected

|            |  Public               | Private                 |Protected             |  package|
|---|---|---|---|---|
|类          |  √                    |  ×                      | ×                    |  √|
|            |  一个java文件只能有一个Public class   |                         |                       |  包名前不加修饰，作用域为当前包|
|字段、方法  |   √                   |   √                     |  √                    |  √|
|            |  可以被其他类访问     |   作用域为当前类或内嵌类|    作用域为当前类和继承类  | 作用域为当前包|


## 包
- 包作用域
  
  位于同一个包的类，可以访问包作用域的字段和方法。不用 public 、 protected 、 private 修饰的字段和方法就是包作用域。
```java
package hello;
    public class Person {
    // 包作用域:
    void hello() {
        System.out.println("Hello!");
    }
}
```

-  classpath和jar
  
  classpath会告诉 JVM 在哪里查找导入的.class
  - 设置classpath的方法
    - 修改系统环境变量classpath（不推荐）
    - 在运行时加上 `--classpath 包路径`或者`-cp 包路径` 参数
  - 在IDE中运行Java程序，IDE自动传入的-cp参数是 **当前工程`bin`目录**和**引入的jar包**
  - jar包
    - jar包就是编译生成的bin目录经过压缩而成的zip文件
    - 导入jar包命令 `java -cp xxx.jar 主class`
    - jar包还可以包含一个特殊的 /META-INF/MANIFEST.MF 文件， MANIFEST.MF 是纯文本，可以指定 Main-Class 和其它信息。JVM会自动读
  取这个 MANIFEST.MF 文件，如果存在 Main-Class ，我们就不必在命令行指定启动的类名，而是用更方便的命令： `java -jar hello.jar`
    - jar包还可以包含其它jar包，这个时候，就需要在 MANIFEST.MF 文件里配置 classpath 了。


## 核心类
- String
  - String 内部是通过一个 char[] 数组表示的
  - 构造方法
    - 可以这样初始化 `String s2 = new String(new char[] {'H', 'e', 'l', 'l', 'o', '!'});`
  - 比较
    - equals() `s1.equals(s2)` *不能用 == 从表面上看，两个字符串用 == 和 equals() 比较都为 true ，但实际上那只是Java编译器在编译期，会自动把所有相同的字符串当作一个对象放入常量池，自然 s1 和 s2 的引用就是相同的*
    - equalsIgnoreCase()
  - 更多方法
    - `"Hello".contains("ll");`
    - `"Hello".indexOf("l"); // 2`
    - `"Hello".lastIndexOf("l"); // 3`
    - `"Hello".startsWith("He"); // true`
    - `"Hello".endsWith("lo"); // true`
    - `"Hello".substring(2); // "llo"`
    - `"Hello".substring(2, 4); "ll"`
    - `" \tHello\r\n ".trim(); // "Hello"`
    - `" Hello ".stripLeading(); // "Hello "`
    - `" Hello ".stripTrailing(); // " Hello"`
    - `"".isEmpty(); // true，因为字符串长度为0`
    - `" ".isEmpty(); // false，因为字符串长度不为0`
    - `" \n".isBlank(); // true，因为只包含空白字符`
    - `" Hello ".isBlank(); // false，因为包含非空白字符`
    - `String s = "hello"; s.replace('l', 'w'); // "hewwo"，所有字符'l'被替换为'w'`
    - `s.replace("ll", "~~"); // "he~~o"，所有子串"ll"被替换为"~~"`
    - 正则表达式替换： `String s = "A,,B;C ,D";s.replaceAll("[\\,\\;\\s]+", ","); // "A,B,C,D"`
    - `String s = "A,B,C,D";String[] ss = s.split("\\,"); // {"A", "B", "C", "D"}`
    - `String[] arr = {"A", "B", "C"};String s = String.join("***", arr); // "A***B***C"`
    - 格式化
      - `String s = "Hi %s, your score is %d!"; System.out.println(s.formatted("Alice", 80));`
      - `String.format("Hi %s, your score is %.2f!", "Bob", 59.5)`
    - 类型转换
      - `String.valueOf(123); // "123"`
      - `String.valueOf(45.67); // "45.67"`
      - `String.valueOf(true); // "true"`
      - `String.valueOf(new Object()); // 类似java.lang.Object@636be97c`
      - `int n1 = Integer.parseInt("123"); // 123`
      - `int n2 = Integer.parseInt("ff", 16); // 按十六进制转换，255`
      - `boolean b1 = Boolean.parseBoolean("true"); // true`
      - `boolean b2 = Boolean.parseBoolean("FALSE"); // false`
      - *Integer 有个 getInteger(String) 方法，它不是将字符串转换为 int ，而是把该字符串对应的系统变量转换为 Integer*
        - `Integer.getInteger("java.version"); // 版本号，11`
      - char[] cs = "Hello".toCharArray(); // String -> char[]
      - String s = new String(cs); // char[] -> String
    - 获取特定编码
      - 由unicode（内存的编码格式）转为特定编码
        - `byte[] b1 = "Hello".getBytes(); // 按系统默认编码转换，不推荐`
        - `byte[] b2 = "Hello".getBytes("UTF-8"); // 按UTF-8编码转换`
        - `byte[] b2 = "Hello".getBytes("GBK"); // 按GBK编码转换`
        - `byte[] b3 = "Hello".getBytes(StandardCharsets.UTF_8); // 按UTF-8编码转换`
      - 由特定编码转换为unicode格式
        - `byte[] b = ...`
        - `String s1 = new String(b, "GBK"); // 按GBK转换`
        - `String s2 = new String(b, StandardCharsets.UTF_8); // 按UTF-8转换`
- StringBuilder
  - 线程不安全
  - 链式操作
    - `sb.append("Mr ").append("Bob").append("!").insert(0, "Hello, ");`
- Stringbuffer
  - StringBuilder的线程安全版本（不允许多线程同时读写）
- StringJoiner
  ```java
  // 要拼接一个字符串，不使用StringJoiner的写法：
  String[] names = {"Bob", "Alice", "Grace"};
  var sb = new StringBuilder();
  sb.append("Hello ");
  for (String name : names) {
    sb.append(name).append(", ");
  }
  // 去掉最后的", ":
  sb.delete(sb.length() - 2, sb.length());
  sb.append("!");
  System.out.println(sb.toString());
  // 使用StringJoiner
  String[] names = {"Bob", "Alice", "Grace"};
  var sj = new StringJoiner(", ");
  for (String name : names) {
    sj.add(name);
  }
  // 使用多参数StringJoiner
  var sj = new StringJoiner(", ", "Hello ", "!");
  for (String name : names) {
    sj.add(name);
  }
  System.out.println(sj.toString());
  ```
    
  - String.join()也可以拼接字符串
    - `String[] names = {"Bob", "Alice", "Grace"}; var s = String.join(", ", names);`


- 包装类

  | 基本类型 | 包装类型 |
  |---|---|
  |boolean |java.lang.Boolean|
  |int     |java.lang.Integer|
  |byte    |java.lang.Byte|
  |short   |java.lang.Short|
  |long    |java.lang.Long|
  |float   |java.lang.Float|
  |double  |java.lang.Double|
  |char    |java.lang.Character|

  - 所有包装类型都是不变类，即用`final`修饰的类，不可以被继承
  - 比较
    - 包装类不能用== 而用equals()比较  *仔细观察结果的童鞋可以发现， == 比较，较小的两个相同的 Integer 返回 true ，较大的两个相同的 Integer 返回 false ，这是因为 Integer 是不变类，编译器把 Integer x = 127; 自动变为 Integer x = Integer.valueOf(127); ，为了节省内存， Integer.valueOf() 对于较小的数，始终返回相同的实例，因此， == 比较“恰好”为 true ，但我们绝不能因为Java标准库的 Integer 内部有缓存优化就用 == 比较，必须用 equals() 方法比较两个 Integer*
  - 进制装换
    - `int x1 = Integer.parseInt("100"); // 100`
    - `int x2 = Integer.parseInt("100", 16); // 256,因为按16进制解析`
    - `Integer.toString(100); // "100",表示为10进制`
    - `Integer.toString(100, 36); // "2s",表示为36进制`
    - `Integer.toHexString(100); // "64",表示为16进制`
    - `Integer.toOctalString(100); // "144",表示为8进制`
    - `Integer.toBinaryString(100); // "1100100",表示为2进制`
  - 静态变量
    - `Boolean t = Boolean.TRUE;`
    - `Boolean f = Boolean.FALSE;`
    - `int max = Integer.MAX_VALUE; // 2147483647`
    - `int min = Integer.MIN_VALUE; // -2147483648`
    - `int sizeOfLong = Long.SIZE; // 64 (bits)`
    - `int bytesOfLong = Long.BYTES; // 8 (bytes)`
  - Number类
    - 所有的整数和浮点数的包装类型都继承自Number类
      - `Number n = new Integer(999)`
      - `float f = n.floatValue()`
      - `byte b = n.byteValue()`
      - ...
- JavaBean
  - 在IDE中输入如下，然后点击右键，在弹出的菜单中选择“Source”，“Generate Getters and Setters”，在弹出的对话框中选中需要生成 getter 和 setter 方法的字段
    ```java
    public class Person {
      private String name;
      private int age;
    }
    ```
  - 枚举java属性
    ```java
    import java.beans.*;
    BeanInfo info = Introspector.getBeanInfo(Person.class);
    for (PropertyDescriptor pd : info.getPropertyDescriptors()) {
      System.out.println(pd.getName());
      System.out.println(" " + pd.getReadMethod());
      System.out.println(" " + pd.getWriteMethod());
    }

    ```
- 枚举类
  - 用法
  ```java
  enum Weekday {
    SUN, MON, TUE, WED, THU, FRI, SAT;
  }
  Weekday day = Weekday.SUN;
  if(day == Weekday.THU){
    ...
  }
  ```
  - 比较
    - 虽然enum也是对象，但是可以用==来比较，这是因为enum类型每个常量在JVM中只有一个唯一实例，所以可以直接用==
  - enum类特点
    - 定义的enum类型总是继承自java.lang.enum,且无法被继承
    - 只能定义出enum的实例，而无法通过new操作符创建enum实例
    - 定义的每一个实例都是引用类型的唯一实例
    - 可以将enum类型用于switch语句
  - enum原理
  ```java
  // 例如我们定义的color
  public enum Color{
    RED, GREEN, BLUE;
  }
  // 编译器编译出的enum类似这样
  public final class Color extends Enum{
    public static final Color RED = new Color();
    public static final Color GREEN = new Color();
    public static final Color BLUE = new Color();
    // 确保外部无法调用new操作符
    private Color() {}
  }
  ```
  - enum方法
    - name() ` String s = Weekday.SUN.name() // "SUN"`
    - ordinal() 返回定义的常量顺序 `int n = Weekday.MON.ordinal(); // 1`
  - 扩展enum方法
  ```java
  enum Weekday {
    MON(1, "星期一"), TUE(2, "星期二"), WED(3, "星期三"), THU(4, "星期四"), FRI(5, "星期五"), SAT(6, "星期六"), SUN(0, "星期日");
    public final int dayValue;
    private final String chinese;
    // 重写构造方法
    private Weekday(int dayValue, String chinese) {
      this.dayValue = dayValue;
      this.chinese = chinese;
    }
    @Override
      public String toString() {
      return this.chinese;
    }
  }
  Weekday day = Weekday.TUE;
  // 装换为int
  if(2 == day.dayValue)
  {...}
  // 打印
  System.out.print(day.chinese)
  ```
- 记录类
  - 背景知识
    - 什么是不变类
      - 定义class时用final，无法继承
      - 定义字段时用final，无法修改
      ```java
      public final class Point {
          private final int x;
          private final int y;
          public Point(int x, int y) {
              this.x = x;
              this.y = y;
          }
          public int x() {
              return this.x;
          }
          public int y() {
              return this.y;
          }
      }
      ```
      *这些代码简单而繁琐*
  - 使用record改写
    - `public record Porint(int x, int y){}`
      表示这个类维护了两个不可变字段
      相当于
      ```java
      public final class Point extends Record {
          private final int x;
          private final int y;
          public Point(int x, int y) {
              this.x = x;
              this.y = y;
          }
          public int x() {
              return this.x;
          }
          public int y() {
              return this.y;
          }
          public String toString() {
              return String.format("Point[x=%s, y=%s]", x, y);
          }
          public boolean equals(Object o) {
              ...
          }
          public int hashCode() {
              ...
          }
      }
      ```
    - 重写构造方法添填加检查逻辑
      ```java
      public record Point(int x, int y) {
          public Point {
              if (x < 0 || y < 0) {
                  throw new IllegalArgumentException();
              }
          }
      }
      ```
      编译器生成的构造方法是
      ```java
      public final class Point extends Record {
          public Point(int x, int y) {
              // 这是我们编写的Compact Constructor:
              if (x < 0 || y < 0) {
                  throw new IllegalArgumentException();
              }
              // 这是编译器继续生成的赋值代码:
              this.x = x;
              this.y = y;
          }
          ...
      }
      ```
    - 可以继续添加静态方法，一般添加of，返回`point`对象
      ```java
      public record Point(int x, int y){
        public static Point of() {
          return new Point(0, 0);
        }
        public static Point of(int x, int y){
          return new Point(x, y)
        }
      }
      ```
      这样我们可以写出更简便的代码
      ```java
      Point p1 = Point.of();
      Point p2 = Point.of(1, 2);
      ```
- BigInteger
  - java.math.BigInteger
  - 用法
    - `BigInteger bi = new BigInteger("1234567890")`
    - `System.out.print(bi.pow(5))`
    - `BigInteger i1 = new BigInteger("1234567890")`
    - `BigInteger i1 = new BigInteger("12345678901234567890")`
    - `BigInteger sum = i1.add(i2)`
    - `long l = i1.longValue()`
    - `long l2 = i1.multiply(i1).longValueExact()  // 使用longValueExact()时，超出范围，会抛出ArithmeticException`
  - 不可变类，继承自Number，所以具备Number的如下方法
    - `byte：byteValue()`
    - `short：shortValue()`
    - `int：intValue()`
    - `long：longValue()`
    - `float：floatValue()`
    - `double：doubleValue()`
    - 上面几种方法都可以加`Exact`形式，超出范围就报错
  - BigInteger和long相比，范围不会受限制，但是速度比较慢
- BigDecimal
  - java.math.BigDecimal
  - 不可变类
  - 用法
    - `BigDecimal bd = new BigDecimal("12.345600")`
    - `bd.scale() // 6 scale()返回小数部分的位数`
    - `BigDecimal bd2 = bd.stripTrailingZeros() // 12.3456`
    - `bd.scale()  // 6`
    - `bd2.scale() // 4`
    - `BigDecimal bd3 = new BigDecimal("345600")`
    - `bd3.scale() // -2 表示整数，末尾2个0`
  - 四舍五入
    ```java
    import java.math.BigDecimal;
    import java.math.RoundingMode;
    ...
    BigDecimal d2 = d1.setScale(4, RoundingMode.HALF_UP); // 四舍五入，123.4568
    BigDecimal d3 = d1.setScale(4, RoundingMode.DOWN); // 直接截断，123.4567
    ```
  - 加减乘，精度不会丢失
  - 除法，必须指定精度和截断方式
    ```JAVA
    BigDecimal d1 = new BigDecimal("123.456");
    BigDecimal d2 = new BigDecimal("23.456789");
    BigDecimal d3 = d1.divide(d2, 10, RoundingMode.HALF_UP); // 保留10位小数并四舍五入
    BigDecimal d4 = d1.divide(d2); // 报错：ArithmeticException，因为除不尽
    ```
  - `divideAndRemainder()` 除法，求余 
    ```java
    BigDecimal n = new BigDecimal("12.345");
    BigDecimal m = new BigDecimal("0.12");
    BigDecimal[] dr = n.divideAndRemainder(m);
    System.out.println(dr[0]); // 102
    System.out.println(dr[1]); // 0.105
    ```
  - 比较
    - equals()
      - 要求值相等，还要scale(小数的位数)相等
    - compareTo() 实现了Comparable接口
      - 推荐用这个方法，只要求值相等
  - BigDecimal维护了一个BigInteger和一个scale，BigInteger一个大数，scale是小数的位数

- 常用工具类
  - Math
    - `Math.abs()`
    - `Math.max()`
    - `Math.min()`
    - `Math.min()`
    - `Math.pow(2, 10)`
    - `Math.sqrt(2)`
    - `Math.exp(2) // e^2`
    - `Math.log(4) // log e 4`
    - `Math.log10(100) // 2`
    - 生成一个随机数x, x的范围是0 <= x < 1
      - `Math.random()`
  - Random
    - `Random r = new Random() // 默认是以系统的当前时间戳为随机数种子生成，每一次都不一样`
    - `r.nextInt()`
    - `r.nextInt(10)`
    - `r.nextLong()`
    - `r.nextFloat()`
    - `r.nextDouble()`
    - `Random r = new Random(1234) // 要是指定种子，则每次随机数都会一样`
  - SecureRandom
    - java.security.SecureRandom
    - 真随机数，其实是假的，只是不可预测随机数 *这个种子是通过CPU的**热噪声**、**读写磁盘的字节**、**网络流量**等各种随机事件产生的“熵”*
    - 用法
      - `SecureRandom sr = new SecureRandom()`
      - `System.out.println(sr.nextInt(100))`
    - 高强度安全随机数生成器
      ```java
      import java.util.Arrays;
      import java.security.SecureRandom;
      import java.security.NoSuchAlgorithmException;
      ...
      SecureRandom sr = null;
      try {
          sr = SecureRandom.getInstanceStrong(); // 获取高强度安全随机数生成器
      } catch (NoSuchAlgorithmException e) {
          sr = new SecureRandom(); // 获取普通的安全随机数生成器
      }
      byte[] buffer = new byte[16];
      sr.nextBytes(buffer); // 用安全随机数填充buffer
      System.out.println(Arrays.toString(buffer));
      ```

## java的异常

```
                     ┌───────────┐
                     │  Object   │
                     └───────────┘
                           ▲
                           │
                     ┌───────────┐
                     │ Throwable │
                     └───────────┘
                           ▲
                 ┌─────────┴─────────┐
                 │                   │
           ┌───────────┐       ┌───────────┐
           │   Error   │       │ Exception │
           └───────────┘       └───────────┘
                 ▲                   ▲
         ┌───────┘              ┌────┴──────────┐
         │                      │               │
┌─────────────────┐    ┌─────────────────┐┌───────────┐
│OutOfMemoryError │... │RuntimeException ││IOException│...
└─────────────────┘    └─────────────────┘└───────────┘
                                ▲
                    ┌───────────┴─────────────┐
                    │                         │
         ┌─────────────────────┐ ┌─────────────────────────┐
         │NullPointerException │ │IllegalArgumentException │...
         └─────────────────────┘ └─────────────────────────┘

Exception
│
├─ RuntimeException
│  │
│  ├─ NullPointerException
│  │
│  ├─ IndexOutOfBoundsException
│  │
│  ├─ SecurityException
│  │
│  └─ IllegalArgumentException
│     │
│     └─ NumberFormatException
│
├─ IOException
│  │
│  ├─ UnsupportedCharsetException
│  │
│  ├─ FileNotFoundException
│  │
│  └─ SocketException
│
├─ ParseException
│
├─ GeneralSecurityException
│
├─ SQLException
│
└─ TimeoutException
```


- 异常规定
  - 必须捕获的异常，包括Exception及其子类，但不包括RuntimeException及其子类，这种类型的异常称为Checked Exception。
  - 不需要捕获的异常，包括Error及其子类，RuntimeException及其子类。
  - *注意：编译器对RuntimeException及其子类不做强制捕获要求，不是指应用程序本身不应该捕获并处理RuntimeException。是否需  注意：编译器对RuntimeException及其子类不做强制捕获要求，不是指应用程序本身不应该捕获并处理RuntimeException。是否需要捕获，具体问题具体分析。*
- checked Exception必须catch或向上抛出，否则编译器会报错
  ```java
  // try...catch
  import java.io.UnsupportedEncodingException;
  import java.util.Arrays;
  public class Main {
      public static void main(String[] args) {
          try {
              byte[] bs = toGBK("中文");
              System.out.println(Arrays.toString(bs));
          } catch (UnsupportedEncodingException e) {
              System.out.println(e);
          }
      }
      static byte[] toGBK(String s) throws UnsupportedEncodingException {
          // 用指定编码转换String为byte[]:
          return s.getBytes("GBK"); 
      }
  }
  public byte[] getBytes(String charsetName) throws UnsupportedEncodingException {
    ...
  }
  ```
- 打印异常栈：尽量不要消化异常，至少要做到打印
  ```java
  // 不好的代码
  static byte[] toGBK(String s) {
    try {
        return s.getBytes("GBK");
    } catch (UnsupportedEncodingException e) {
        // 什么也不干
    }
    return null;
  }
  // 好的代码
  static byte[] toGBK(String s) {
    try {
        return s.getBytes("GBK");
    } catch (UnsupportedEncodingException e) {
        // 先记下来再说:
        e.printStackTrace();
    }
    return null;
  }
  ```
- 多次捕获
  ```java
  // 下面代码是捕获多次的写法，但是第二次永远也捕获不到，因为IOException是UnsupportedEncodingException的父类
  public static void main(String[] args) {
      try {
          process1();
          process2();
          process3();
      } catch (IOException e) {
          System.out.println("IO error");
      } catch (UnsupportedEncodingException e) { // 永远捕获不到
          System.out.println("Bad encoding");
      }
  }
  ```
- 同时捕获
  ```java
  public static void main(String[] args) {
      try {
          process1();
          process2();
          process3();
      } catch (IOException | NumberFormatException e) { // IOException或NumberFormatException
          System.out.println("Bad input");
      } catch (Exception e) {
          System.out.println("Unknown error");
      }
  }
  ```
- 抛出异常/转换抛出异常
  ```java
  public class Main {
      public static void main(String[] args) {
          try {
              process1();                     // 第5行
          } catch (Exception e) {
              e.printStackTrace();
          }
      }

      static void process1() {
          try {
              process2();
          } catch (NullPointerException e) {
              throw new IllegalArgumentException();    // 第15行
          }
      }
      static void process2() {
          throw new NullPointerException();
      }
  }
  ```
  - 如果process2抛出异常，调用栈分析
    - 情况1：IllegalArgumentException() 不加参数e
      ```java
      java.lang.IllegalArgumentException
      at Main.process1(Main.java:15)
      at Main.main(Main.java:5)
      ```
    - 情况2：IllegalArgumentException() 加参数e
      ```java
      // 加上原始的Exception，新的Exception就可以持有原始的Exception信息
      java.lang.IllegalArgumentException: java.lang.NullPointerException
          at Main.process1(Main.java:15)
          at Main.main(Main.java:5)
      Caused by: java.lang.NullPointerException
          at Main.process2(Main.java:20)
          at Main.process1(Main.java:13)
      ``` 
- 自定义异常
  - 一般自定义异常的写法
    ```java
    public class BaseException extends RuntimeException {
      public BaseException(){
        super();
      }
      public BaseException(String message, Throwable cause){
        super(message, cause);
      }
      public BaseException(String message){
        super(message)
      }
      public BaseException(Throwable cause){
        super(Throwable)
      }
    }
    // 其他业务类型的异常就可以从BaseException派生：
    public class UserNotFoundException extends BaseException {

    }
    public class LoginFailedException extends BaseException{

    }
    ```

- 空指针异常 NullPointerException
  - 最为常见的异常
  - 原因：当调用一个null对象的方法或者访问其字段，就会抛出NullPointerException，是由JVM抛出
  - NullPointerException异常是代码逻辑错误，必须早暴露早解决，严禁使用catch去捕获屏蔽
  - 实例
    ```java
    public class Main {
      public static void main(String[] args) {
          Person p = new Person();
          System.out.println(p.address.city.toLowerCase());
      }
    }
    class Person {
        String[] name = new String[2];
        Address address = new Address();
    }
    class Address {
        String city;
        String street;
        String zipcode;
    }
    // 上述代码会报空指针的错误，因为city没有初始化，应该赋空值 String city = ""
    // 当开启了ShowCodeDetailsInExceptionMessages参数，会打印出具体是哪一级对象是null，这是java14的新特性，需要这样指定开启：java -XX:+ShowCodeDetailsInExceptionMessages Main.java
    ```
  - 定义字段时要赋初值

- 断言
  - 断言是不可恢复程序的Error，只用于开发和测试阶段，对于可恢复的程序错误，不能使用断言
  - 实例
    ```java
    // 当断言失败会打印后面的信息
    assert x >= 0 : "x must >= 0";
    ```
  - 要使用断言，必须给jvm加-enableassertions参数：`java -ea Main.java`,否则，会跳过所有的断言语句 
  - 有选择的启动断言
    - `java -ea:com.itranswarp.sample.Main Main.java` 表示只给com.itranswarp.sample.Main类使用断言
    - `java -ea:com.itranswarp.sample... Main.java` 表示只给com.itranswarp.sample 这个包使用断言
  - 实际开发中，***很少使用断言***。更好的方法是编写单元测试，后续我们会讲解JUnit的使用。

## 日志系统
- 日志就是Logging，它的目的是为了取代System.out.println()
- 输出日志，而不是用System.out.println()，有以下几个好处：
  1.  可以设置输出样式，避免自己每次都写"ERROR: " + var；
  2.  可以设置输出级别，禁止某些级别输出。例如，只输出错误日志；
  3.  可以被重定向到文件，这样可以在程序运行结束后查看日志；
  4.  可以按包名控制日志级别，只输出某些包打的日志；
  5.  可以……
- 标准库日志包：java.util.logging
  - 简单的例子：
    ```java
    import java.util.logging.Level;
    import java.util.logging.Logger;
    public class Hello {
        public static void main(String[] args) {
            Logger logger = Logger.getGlobal();
            logger.info("start process...");
            logger.warning("memory is running out...");
            logger.fine("ignored.");
            logger.severe("process will be terminated...");
        }
    }
    // 执行后会输出：
    Mar 02, 2019 6:32:13 PM Hello main
    INFO: start process...
    Mar 02, 2019 6:32:13 PM Hello main
    WARNING: memory is running out...
    Mar 02, 2019 6:32:13 PM Hello main
    SEVERE: process will be terminated..
    // 发现fine()没有打印
    ```
  - 日志级别
    - 等级划分
      - SEVERE
      - WARNING
      - INFO
      - CONFIG
      - FINE
      - FINER
      - FINEST
    - 默认的日志级别是INFO，INFO以下都不会打印
    - 设定级别需要写在配置文件中：-Djava.util.logging.config.file=<config-file-name>
  - 示例
    ```java
    import java.io.UnsupportedEncodingException;
    import java.util.logging.Logger;
    public class Main {
        public static void main(String[] args) {
            Logger logger = Logger.getLogger(Main.class.getName());
            logger.info("Start process...");
            try {
                "".getBytes("invalidCharsetName");
            } catch (UnsupportedEncodingException e) {
                // TODO: 使用logger.severe()打印异常
                logger.severe("UnsupportedEncodingException");
            }
            logger.info("Process end.");
        }
    }
    ```
- Commons Logging
  - 自动搜索并使用Log4j（Log4j是另一个流行的日志系统），如果没有找到Log4j，再使用JDK Logging
  - 示例
    ```java
    import org.apache.commons.logging.Log;
    import org.apache.commons.logging.LogFactory;

    public class Main{
      public class main(String[] args){
        Log log = LogFactory.getLog(Main.class);
        log.info("...");
        log.warn("end.");
      }
    }
    ```
    - 导入包并编译
      - 需要把`commons-logging-1.2.jar`jar包下载下来，放到一个/path路径中
      - 在编译时，使用命令`java -cp /path/commons-logging-1.2.jar Main.java`
      - cp代表class path，意为添加一个class path路径，这个jar包本身相当于一个路径
    - 运行程序
      - 执行`java -cp .;commons-logging-1.2.jar Main.class`
      - 上面命令-cp有两部分，当前目录加上jar包目录，要是没有当前目录./会导致java不会再当前路径搜索Main.class,导致报错
    - 拓展知识:
      - 有些jar包，java程序在编译和运行时都要用到，有些只有在运行时用到，例如mySql数据库驱动只在运行时需要，编译时只要有JDBC接口即可
  - 日志级别：
    - FATAL
    - ERROR
    - WARNING
    - INFO
    - DEBUG
    - TRACE
    - 默认是INFO
  - 重载方法
    - `log.info(String, Throwable)`
    - 示例
      ```java
      try {
          ...
      } catch (Exception e) {
          log.error("got exception!", e);
      }
      // 这样做的好处是方便打印异常，而不需要再写一行e.printStackTrace();
      ``` 
  - 注意
    ```java
        public class Person{
          // protected代表子类可以访问，外部不能随便访问，final代表一旦定义就不能修改值，相当于c++的const
          // 修饰符也可以加static，表示多个实例对象共用类中的公共字段, 节省内存
          // 这里传入的参数用getClass(),而不是Person.class是因为能兼容到继承这个类的子类Student，要是为Person.class，就写死了，不适用子类
          protected final Log log = LogFactory.getLog(getClass())
        }

        public class Student extends Person{
        }

    ```


- 使用Log4j
  - 需要准备下面的jar包并放入class path
    - commons logging包 这个包相当于一个API接口
      - commons-logging-1.2.jar
    - Log4j相关jar包  这个包相当于底层实现
      - log4j-api-2.x.jar
      - log4j-core-2.x.jar
      - log4j-jcl-2.x.jar
  - 配置方式：定义Log4j2.xml，放到classpath下就可以让Log4j读取配置文件并按照我们的配置来输出日志
  - 示例
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <Configuration>
        <Properties>
            <!-- 定义日志格式 -->
            <Property name="log.pattern">%d{MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36}%n%msg%n%n</Property>
            <!-- 定义文件名变量 -->
            <Property name="file.err.filename">log/err.log</Property>
            <Property name="file.err.pattern">log/err.%i.log.gz</Property>
        </Properties>
          <!-- 定义Appender，即目的地 -->
        <Appenders>
            <!-- 定义输出到屏幕 -->
            <Console name="console" target="SYSTEM_OUT">
                <!-- 日志格式引用上面定义的log.pattern -->
                <PatternLayout pattern="${log.pattern}" />
            </Console>
            <!-- 定义输出到文件,文件名引用上面定义的file.err.filename -->
            <RollingFile name="err" bufferedIO="true" fileName="${file.err.filename}" filePattern="${file.err.pattern}">
                <PatternLayout pattern="${log.pattern}" />
                <Policies>
                    <!-- 根据文件大小自动切割日志 -->
                    <SizeBasedTriggeringPolicy size="1 MB" />
                </Policies>
                <!-- 保留最近10份 -->
                <DefaultRolloverStrategy max="10" />
            </RollingFile>
        </Appenders>
        <Loggers>
            <Root level="info">
                <!-- 对info级别的日志，输出到console -->
                <AppenderRef ref="console" level="info" />
                <!-- 对error级别的日志，输出到err，即上面定义的RollingFile -->
                <AppenderRef ref="err" level="error" />
            </Root>
        </Loggers>
    </Configuration>
    ```
  - 用法: 参考上一节commons logging

- SLF4J和Logback
  - SLF4J相当于commons logging
  - Logback相当于Log4J
  - 比较
    | Commons Logging | SLF4J |
    | --- | --- |
    | org.apache.commons.logging.Log | org.slf4j.Logger |
    | org.apache.commons.logging.LogFactory | org.slf4j.Logger |
  - SLF4J
    ```JAVA
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    class Main {
        final Logger logger = LoggerFactory.getLogger(getClass());
    }
    ```
  - Logback
    ```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
         <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
      </appender>
      <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <encoder>
          <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
          <charset>utf-8</charset>
        </encoder>
        <file>log/output.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.FixedWindowRollingPolicy">
          <fileNamePattern>log/output.log.%i</fileNamePattern>
        </rollingPolicy>
        <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
          <MaxFileSize>1MB</MaxFileSize>
        </triggeringPolicy>
      </appender>
      <root level="INFO">
        <appender-ref ref="CONSOLE" />
        <appender-ref ref="FILE" />
      </root>
    </configuration>
    ```

## 反射机制
- Class是什么
  - class是由JVM在执行过程中动态加载的，JVM在第一次读取到一种class类型时，将其加载进内存，每加载一种class，JVM就为其创建一个`Class`类型的实例并与class关联起来
    ```java
    public final class Class {
      // 构造方法是private，我们不能创建Class实例，只能由JVM创建
      private Class() {}
    }
    Class cls = new Class(String)
    ```
  - 一个`Class`实例包含了该class的所有完整信息

    | Class instance |
    | --- |
    | name = java.lang.String |
    | package = java.lang |
    | super = java.lang.Object |
    | interface = CharSequence |
    | field = value[], hash, ... |
    | method = indexOf, ... |
- 通过Class实例来获取class信息的方法称为`反射`
- 如何获取Class
  - 方法一：Class cls = String.class
  - 方法二：Class cls = s.getClass()
  - 方法三：Class cls = Class.forName("java.lang.String")
- Class能干什么
  - 获取class信息
    ```java
    public class Main {
      public static void main(String[] args) {
          printClassInfo("".getClass());
          // Class name: java.lang.String 
          // Simple name: String 
          // Package name: java.lang
          // is interface: false
          // is enum: false
          // is array: false
          // is primitive: false
          printClassInfo(Runnable.class);
          // is interface: true
          printClassInfo(java.time.Month.class);
          // is enum: true
          printClassInfo(String[].class);
          // Class name: [Ljava.lang.String
          // Simple name: String[]
          // is array: true
          printClassInfo(int.class);      // jvm也为基本类型（非类类型）创建了Class实例
          // Class name: int
          // Simple name: int
          // is primitive: true
      }
      static void printClassInfo(Class cls) {
          System.out.println("Class name: " + cls.getName());   
          System.out.println("Simple name: " + cls.getSimpleName());  
          if (cls.getPackage() != null) {
              System.out.println("Package name: " + cls.getPackage().getName());
          }
          System.out.println("is interface: " + cls.isInterface());
          System.out.println("is enum: " + cls.isEnum());
          System.out.println("is array: " + cls.isArray());
          System.out.println("is primitive: " + cls.isPrimitive());
      }
    }
    ``` 
  - 创建class实例
    ```java
    Class cls = String.class;
    String s = cls.newInstance()  // 相当于new String()
    ``` 
    **newInstance()方法只支持无参构造**
- JAVA动态加载
  ```java
  // Main.java
  public class Main {
      public static void main(String[] args) {
          if (args.length > 0) {
              create(args[0]);
          }
      }
      static void create(String name) {
          Person p = new Person(name);
      }
  }
  ```
  当运行Main.class时，jvm会先加载Main.class，并不会加载Person.class，当运行到create()方法时，才会加载
