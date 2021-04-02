# 路径

**相对路径**：“."就表示**当前**的文件夹，而**”..“**则表示当前文件夹的**上一级**文件夹 ；

## 利用File类提供的方法获取文件路径

  getCanonicalPath( ): 得到相对路径；

  getAbsolutePath( ): 得到绝对路径；

  getPath( ): 得到的只是你在new File()时设定的路径 ；

```java
//例：当前的路径为 C:/test ： 
    （1）File directory = new File("abc"); 
            directory.getCanonicalPath(); 			//得到的是C:/test/abc 
            directory.getAbsolutePath();    		//得到的是C:/test/abc 

            direcotry.getPath();                    //得到的是abc 

    （2）File directory = new File(".");
        String a = directory.getCanonicalPath();
        String b = directory.getAbsolutePath();
        System.out.println(a);						//得到的是C:/test 
        System.out.println(b);						//得到的是C:/test/. 
        System.out.println(direcotry.getPath());    //得到的是. 

    （3）File directory = new File(".."); 
            directory.getCanonicalPath(); 			//得到的是C:/ 
            directory.getAbsolutePath();    		//得到的是C:/test/.. 
            direcotry.getPath();                    //得到的是.. 
```

## 利用System.getProperty(**key**)函数获取当前路径

**可以通过调用`System.getProperty("user.dir");` 来获得**当前路径。

#### 拓展

> **各种关键词**
> 	java.version          Java 运行时环境版本
> 	java.vendor         Java 运行时环境供应商
> 	java.vendor.url         Java 供应商的 URL
> 	java.vm.specification.version         Java 虚拟机规范版本
> 	java.vm.specification.vendor         Java 虚拟机规范供应商
> 	java.vm.specification.name         Java 虚拟机规范名称
> 	java.vm.version         Java 虚拟机实现版本
> 	java.vm.vendor         Java 虚拟机实现供应商
> 	java.vm.name         Java 虚拟机实现名称
> 	java.specification.version         Java 运行时环境规范版本
> 	java.specification.vendor         Java 运行时环境规范供应商
> 	java.specification.name         Java 运行时环境规范名称
> 	os.name         操作系统的名称
> 	os.arch         操作系统的架构
> 	os.version         操作系统的版本
> 	file.separator         文件分隔符（在 UNIX 系统中是“ / ”）
> 	path.separator         路径分隔符（在 UNIX 系统中是“ : ”）
> 	line.separator         行分隔符（在 UNIX 系统中是“ /n ”）
> 	 
>
> 	java.home         Java 安装目录
> 	java.class.version         Java 类格式版本号
> 	java.class.path         Java 类路径
> 	java.library.path          加载库时搜索的路径列表
> 	java.io.tmpdir         默认的临时文件路径
> 	java.compiler         要使用的 JIT 编译器的名称
> 	java.ext.dirs         一个或多个扩展目录的路径
> 	user.name         用户的账户名称
> 	user.home         用户的主目录

## 利用Thread

String path = Thread.currentThread().getContextClassLoader().getResource(**"com/classinfo2.properties"**).getPath(); 

这种方式获取文件绝对路径是通用的。填入类路径下的文件：     .   是当前，..   是上一层路径。

> Thread.currentThread() 当前线程对象
> getContextClassLoader() 是线程对象的方法，可以获取到当前线程的类加载器对象。
> getResource() 【获取资源】这是类加载器对象的方法，当前线程的类加载器默认从类的根路径下加载资源。

#### 类路径

​		方式在src下的都是类路径下。【记住它】 **src是类的根路径**。

```java
public static void main(String[] args) throws Exception{
    // 这种方式的路径缺点是：移植性差，在IDEA中默认的当前路径是project的根。
    // 这个代码假设离开了IDEA，换到了其它位置，可能当前路径就不是project的根了，这时这个路径就无效了。
    //FileReader reader = new FileReader("chapter25/classinfo2.properties");

    // 接下来说一种比较通用的一种路径。即使代码换位置了，这样编写仍然是通用的。
    // 注意：使用以下通用方式的前提是：这个文件必须在类路径下。
    // 什么类路径下？方式在src下的都是类路径下。【记住它】
    // src是类的根路径。
   
    String path = Thread.currentThread().getContextClassLoader()
        .getResource("classinfo2.properties").getPath(); // 这种方式获取文件绝对路径是通用的。

    // 采用以上的代码可以拿到一个文件的绝对路径。
    // /C:/Users/Administrator/IdeaProjects/javase/out/production/chapter25/classinfo2.properties
    System.out.println(path);

    // 获取db.properties文件的绝对路径（从类的根路径下作为起点开始）
    String path2 = Thread.currentThread().getContextClassLoader()
        .getResource("com/bjpowernode/java/bean/db.properties").getPath();
    System.out.println(path2);

}
```

```java
	String path = Thread.currentThread().getContextClassLoader()
					  .getResource("写相对路径，但是这个相对路径从src出发开始找").getPath();	
	String path = Thread.currentThread().getContextClassLoader()
					  .getResource("abc").getPath();	//必须保证src下有abc文件。

	String path = Thread.currentThread().getContextClassLoader()
					  .getResource("a/db").getPath();	//必须保证src下有a目录，a目录下有db文件。
	
	String path = Thread.currentThread().getContextClassLoader()
					  .getResource("com/bjpowernode/test.properties").getPath();	
					  //必须保证src下有com目录，com目录下有bjpowernode目录。
					  //bjpowernode目录下有test.properties文件。
```


## 资源绑定器

- 资源绑定器：
  java.util包下提供了一个资源绑定器，便于获取属性配置文件中的内容
  使用以下这种方式的时候，**属性配置文件xxx.properties**必须放到类路径下。
- 在写路径的时候，路径后面的**扩展名不能写**

> ​	ResourceBundle bundle = ResourceBundle.getBundle("com/bjpowernode/test");
> ​	String value = bundle.getString(key);

```java
public class ResourceBundleTest01 {
    public static void main(String[] args) {
        /*
        资源绑定器：只能绑定.properties文件，并且这文件必须放到类路径下。
        并且在写路径的时候，路径后面的扩展名不能写
         */
        java.util.ResourceBundle bundle = java.util.ResourceBundle.getBundle("reflection/Test");
        //通过key获取value
        System.out.println(bundle.getString("MyClass"));
    }
}
```





# 访问控制权限

### 1、访问控制权限都有哪些？

- private	私有
- public 公开
- protected	受保护
- 默认

### 2、访问控制权限控制的范围 

​	private 表示私有的，只能在本类中访问
​	public 表示公开的，在任何位置都可以访问
​	“默认”表示只能在本类，以及同包下访问。
​	protected表示只能在本类、同包、子类中访问。

| 访问控制修饰符 | 本类 | 同包 | 子类 | 任意位置 |
| -------------- | ---- | ---- | ---- | -------- |
| public         | 可以 | 可以 | 可以 | 可以     |
| protected      | 可以 | 可以 | 可以 | 不行     |
| 默认           | 可以 | 可以 | 不行 | 不行     |
| private        | 可以 | 不行 | 不行 | 不行     |

​	这个不要死记硬背，自己下去之后编写代码自己测试。

​	**范围从大到小排序：public > protected > 默认 > private**

### 3、访问控制权限修饰符可以修饰什么？

​	属性（4个都能用）
​	方法（4个都能用）
​	类（public和默认能用，其它不行。）
​	接口（public和默认能用，其它不行。）
​	.....







# 正则表达式

java.util.regex 包主要包括以下三个类：

- Pattern 类：

  pattern 对象是一个正则表达式的编译表示。Pattern 类没有公共构造方法。要创建一个 Pattern 对象，你必须首先调用其公共静态编译方法，它返回一个 Pattern 对象。该方法接受一个正则表达式作为它的第一个参数。

- Matcher 类：

  Matcher 对象是对输入字符串进行解释和匹配操作的引擎。与Pattern 类一样，Matcher 也没有公共构造方法。你需要调用 Pattern 对象的 matcher 方法来获得一个 Matcher 对象。

- PatternSyntaxException：

  PatternSyntaxException 是一个非强制异常类，它表示一个正则表达式模式中的语法错误。

  

### Pattern类

Pattern类用于创建一个正则表达式,也可以说创建一个匹配模式,它的构造方法是私有的,不可以直接创建,但可以通过Pattern.complie(String regex)简单工厂方法创建一个正则表达式, 

```java
Pattern p=Pattern.compile("\\w+"); 
p.pattern();//返回 \w+ 
```

pattern() 返回正则表达式的字符串形式,其实就是返回Pattern.complile(String regex)的regex参数

**1.Pattern.split(CharSequence input)**

​     Pattern有一个split(CharSequence input)方法,用于分隔字符串,并返回一个String[],我猜String.split(String regex)就是通过Pattern.split(CharSequence input)来实现的. 

```java
Pattern p=Pattern.compile("\\d+"); 
String[] str=p.split("我的QQ是:456456我的电话是:0532214我的邮箱是:aaa@aaa.com"); 
```

结果:str[0]="我的QQ是:" str[1]="我的电话是:" str[2]="我的邮箱是:aaa@aaa.com" 

**2.Pattern.matcher(String regex,CharSequence input)是一个静态方法,用于快速匹配字符串,该方法适合用于只匹配一次,且匹配全部字符串.**

```java
Pattern.matches("\\d+","2223");//返回true 
Pattern.matches("\\d+","2223aa");//返回false,需要匹配到所有字符串才能返回true,这里aa不能匹配到 
Pattern.matches("\\d+","22bb23");//返回false,需要匹配到所有字符串才能返回true,这里bb不能匹配到 
```

**3.Pattern.matcher(CharSequence input)**

​     说了这么多,终于轮到Matcher类登场了,Pattern.matcher(CharSequence input)返回一个Matcher对象.
​     Matcher类的构造方法也是私有的,不能随意创建,只能通过Pattern.matcher(CharSequence input)方法得到该类的实例. 
​     Pattern类只能做一些简单的匹配操作,要想得到更强更便捷的正则匹配操作,那就需要将Pattern与Matcher一起合作.Matcher类提供了对正则表达式的分组支持,以及对正则表达式的多次匹配支持. 

```java
Pattern p=Pattern.compile("\\d+"); 
Matcher m=p.matcher("22bb23"); 
m.pattern();//返回p 也就是返回该Matcher对象是由哪个Pattern对象的创建的 
```

### 捕获组

捕获组是把多个字符当一个单独单元进行处理的方法，它通过对括号内的字符分组来创建。

例如，正则表达式 (dog) 创建了单一分组，组里包含"d"，"o"，和"g"。

捕获组是通过从左至右计算其开括号来编号。例如，在表达式（（A）（B（C））），有四个这样的组：

- ((A)(B(C)))
- (A)
- (B(C))
- (C)

可以通过调用 matcher 对象的 groupCount 方法来查看表达式有多少个分组。groupCount 方法返回一个 int 值，表示matcher对象当前有多个捕获组。

还有一个特殊的组（group(0)），它总是代表整个表达式。该组不包括在 groupCount 的返回值中。





### Matcher 类的方法

#### 索引方法

索引方法提供了有用的索引值，精确表明输入字符串中在哪能找到匹配：

| **序号** | **方法及说明**                                               |
| :------- | :----------------------------------------------------------- |
| 1        | **public int start()** 返回以前匹配的初始索引。              |
| 2        | **public int start(int group)**  返回在以前的匹配操作期间，由给定组所捕获的子序列的初始索引 |
| 3        | **public int end()** 返回最后匹配字符之后的偏移量。          |
| 4        | **public int end(int group)** 返回在以前的匹配操作期间，由给定组所捕获子序列的最后字符之后的偏移量。 |

#### 查找方法

查找方法用来检查输入字符串并返回一个布尔值，表示是否找到该模式：

| **序号** | **方法及说明**                                               |
| :------- | :----------------------------------------------------------- |
| 1        | **public boolean lookingAt()**  尝试将从区域开头开始的输入序列与该模式匹配。 |
| 2        | **public boolean find()** 尝试查找与该模式匹配的输入序列的下一个子序列。 |
| 3        | **public boolean find(int start****）** 重置此匹配器，然后尝试查找匹配该模式、从指定索引开始的输入序列的下一个子序列。 |
| 4        | **public boolean matches()** 尝试将整个区域与模式匹配。      |

#### 替换方法

替换方法是替换输入字符串里文本的方法：

| **序号** | **方法及说明**                                               |
| :------- | :----------------------------------------------------------- |
| 1        | **public Matcher appendReplacement(StringBuffer sb, String replacement)** 实现非终端添加和替换步骤。 |
| 2        | **public StringBuffer appendTail(StringBuffer sb)** 实现终端添加和替换步骤。 |
| 3        | **public String replaceAll(String replacement)**  替换模式与给定替换字符串相匹配的输入序列的每个子序列。 |
| 4        | **public String replaceFirst(String replacement)**  替换模式与给定替换字符串匹配的输入序列的第一个子序列。 |
| 5        | **public static String quoteReplacement(String s)** 返回指定字符串的字面替换字符串。这个方法返回一个字符串，就像传递给Matcher类的appendReplacement 方法一个字面字符串一样工作。 |



#### **Matcher.matches()/ Matcher.lookingAt()/ Matcher.find()**

​    Matcher类提供三个匹配操作方法,三个方法均返回boolean类型,当匹配到时返回true,没匹配到则返回false 
​    matches()对整个字符串进行匹配,只有整个字符串都匹配了才返回true 

```java
Pattern p=Pattern.compile("\\d+"); 
Matcher m=p.matcher("22bb23"); 
m.matches();//返回false,因为bb不能被\d+匹配,导致整个字符串匹配未成功. 
Matcher m2=p.matcher("2223");
m2.matches();//返回true,因为\d+匹配到了整个字符串
```

​    我们现在回头看一下Pattern.matcher(String regex,CharSequence input),它与下面这段代码等价 
Pattern.compile(regex).matcher(input).matches() 
lookingAt()对前面的字符串进行匹配,只有匹配到的字符串在最前面才返回true 

```java
Pattern p=Pattern.compile("\\d+"); 
Matcher m=p.matcher("22bb23"); 
m.lookingAt();//返回true,因为\d+匹配到了前面的22 
Matcher m2=p.matcher("aa2223"); 
m2.lookingAt();//返回false,因为\d+不能匹配前面的aa 
```

find()对字符串进行匹配,匹配到的字符串可以在任何位置. 

```java
Pattern p=Pattern.compile("\\d+"); 
Matcher m=p.matcher("22bb23"); 
m.find();//返回true 
Matcher m2=p.matcher("aa2223"); 
m2.find();//返回true 
Matcher m3=p.matcher("aa2223bb"); 
m3.find();//返回true 
Matcher m4=p.matcher("aabb"); 
m4.find();//返回false 
```

#### Mathcer.start()/ Matcher.end()/ Matcher.group()**

  当使用matches(),lookingAt(),find()执行匹配操作后,就可以利用以上三个方法得到更详细的信息. 

- start()返回匹配到的子字符串在字符串中的索引位置. 
- end()返回匹配到的子字符串的最后一个字符在字符串中的索引位置. 
- group()返回匹配到的子字符串 

```java
Pattern p=Pattern.compile("\\d+"); 
Matcher m=p.matcher("aaa2223bb"); 
m.find();//匹配2223 
m.start();//返回3 
m.end();//返回7,返回的是2223后的索引号 
m.group();//返回2223 
Mathcer m2=m.matcher("2223bb"); 
m.lookingAt();   //匹配2223 
m.start();   //返回0,由于lookingAt()只能匹配前面的字符串,所以当使用lookingAt()匹配时,start()方法总是返回0 
m.end();   //返回4 
m.group();   //返回2223 
Matcher m3=m.matcher("2223bb"); 
m.matches();   //匹配整个字符串 
m.start();   //返回0,原因相信大家也清楚了 
m.end();   //返回6,原因相信大家也清楚了,因为matches()需要匹配所有字符串 
m.group();   //返回2223bb 
```

​    说了这么多,相信大家都明白了以上几个方法的使用,该说说正则表达式的分组在java中是怎么使用的. 
start(),end(),group()均有一个重载方法它们是start(int i),end(int i),group(int i)专用于分组操作,Mathcer类还有一个groupCount()用于返回有多少组. 

```java
Pattern p=Pattern.compile("([a-z]+)(\\d+)"); 
Matcher m=p.matcher("aaa2223bb"); 
m.find();   //匹配aaa2223 
m.groupCount();   //返回2,因为有2组 
m.start(1);   //返回0 返回第一组匹配到的子字符串在字符串中的索引号 
m.start(2);   //返回3 
m.end(1);   //返回3 返回第一组匹配到的子字符串的最后一个字符在字符串中的索引位置. 
m.end(2);   //返回7 
m.group(1);   //返回aaa,返回第一组匹配到的子字符串 
m.group(2);   //返回2223,返回第二组匹配到的子字符串
```







### 语法大全

| 字符          | 说明                                                         |
| :------------ | :----------------------------------------------------------- |
| \             | 将下一字符标记为特殊字符、文本、反向引用或八进制转义符。例如，"n"匹配字符"n"。"\n"匹配换行符。序列"\\"匹配"\"，"\("匹配"("。 |
| ^             | 匹配输入字符串开始的位置。如果设置了 **RegExp** 对象的 **Multiline** 属性，^ 还会与"\n"或"\r"之后的位置匹配。 |
| $             | 匹配输入字符串结尾的位置。如果设置了 **RegExp** 对象的 **Multiline** 属性，$ 还会与"\n"或"\r"之前的位置匹配。 |
| *             | 零次或多次匹配前面的字符或子表达式。例如，zo* 匹配"z"和"zoo"。* 等效于 {0,}。 |
| +             | 一次或多次匹配前面的字符或子表达式。例如，"zo+"与"zo"和"zoo"匹配，但与"z"不匹配。+ 等效于 {1,}。 |
| ?             | 零次或一次匹配前面的字符或子表达式。例如，"do(es)?"匹配"do"或"does"中的"do"。? 等效于 {0,1}。 |
| {*n*}         | *n* 是非负整数。正好匹配 *n* 次。例如，"o{2}"与"Bob"中的"o"不匹配，但与"food"中的两个"o"匹配。 |
| {*n*,}        | *n* 是非负整数。至少匹配 *n* 次。例如，"o{2,}"不匹配"Bob"中的"o"，而匹配"foooood"中的所有 o。"o{1,}"等效于"o+"。"o{0,}"等效于"o*"。 |
| {*n*,*m*}     | *M* 和 *n* 是非负整数，其中 *n* <= *m*。匹配至少 *n* 次，至多 *m* 次。例如，"o{1,3}"匹配"fooooood"中的头三个 o。'o{0,1}' 等效于 'o?'。注意：您不能将空格插入逗号和数字之间。 |
| ？            | 当此字符紧随任何其他限定符（*、+、?、{*n*}、{*n*,}、{*n*,*m*}）之后时，匹配模式是"非贪心的"。"非贪心的"模式匹配搜索到的、尽可能短的字符串，而默认的"贪心的"模式匹配搜索到的、尽可能长的字符串。例如，在字符串"oooo"中，"o+?"只匹配单个"o"，而"o+"匹配所有"o"。 |
| .             | 匹配除"\r\n"之外的任何单个字符。若要匹配包括"\r\n"在内的任意字符，请使用诸如"[\s\S]"之类的模式。 |
| (*pattern*)   | 匹配 *pattern* 并捕获该匹配的子表达式。可以使用 **$0…$9** 属性从结果"匹配"集合中检索捕获的匹配。若要匹配括号字符 ( )，请使用""或者""或者""。 |
| (?:*pattern*) | 匹配 *pattern* 但不捕获该匹配的子表达式，即它是一个非捕获匹配，不存储供以后使用的匹配。这对于用"or"字符 (\|) 组合模式部件的情况很有用。例如，'industr(?:y\|ies) 是比 'industry\|industries' 更经济的表达式。 |
| (?=*pattern*) | 执行正向预测先行搜索的子表达式，该表达式匹配处于匹配 *pattern* 的字符串的起始点的字符串。它是一个非捕获匹配，即不能捕获供以后使用的匹配。例如，'Windows (?=95\|98\|NT\|2000)' 匹配"Windows 2000"中的"Windows"，但不匹配"Windows 3.1"中的"Windows"。预测先行不占用字符，即发生匹配后，下一匹配的搜索紧随上一匹配之后，而不是在组成预测先行的字符后。 |
| (?!*pattern*) | 执行反向预测先行搜索的子表达式，该表达式匹配不处于匹配 *pattern* 的字符串的起始点的搜索字符串。它是一个非捕获匹配，即不能捕获供以后使用的匹配。例如，'Windows (?!95\|98\|NT\|2000)' 匹配"Windows 3.1"中的 "Windows"，但不匹配"Windows 2000"中的"Windows"。预测先行不占用字符，即发生匹配后，下一匹配的搜索紧随上一匹配之后，而不是在组成预测先行的字符后。 |
| *x*\|*y*      | 匹配 *x* 或 *y*。例如，'z\|food' 匹配"z"或"food"。'(z\|f)ood' 匹配"zood"或"food"。 |
| [*xyz*]       | 字符集。匹配包含的任一字符。例如，"[abc]"匹配"plain"中的"a"。 |
| [^*xyz*]      | 反向字符集。匹配未包含的任何字符。例如，"[^abc]"匹配"plain"中"p"，"l"，"i"，"n"。 |
| [*a-z*]       | 字符范围。匹配指定范围内的任何字符。例如，"[a-z]"匹配"a"到"z"范围内的任何小写字母。 |
| [^*a-z*]      | 反向范围字符。匹配不在指定的范围内的任何字符。例如，"[^a-z]"匹配任何不在"a"到"z"范围内的任何字符。 |
| \b            | 匹配一个字边界，即字与空格间的位置。例如，"er\b"匹配"never"中的"er"，但不匹配"verb"中的"er"。 |
| \B            | 非字边界匹配。"er\B"匹配"verb"中的"er"，但不匹配"never"中的"er"。 |
| \c*x*         | 匹配 *x* 指示的控制字符。例如，\cM 匹配 Control-M 或回车符。*x* 的值必须在 A-Z 或 a-z 之间。如果不是这样，则假定 c 就是"c"字符本身。 |
| \d            | 数字字符匹配。等效于 [0-9]。                                 |
| \D            | 非数字字符匹配。等效于 [^0-9]。                              |
| \f            | 换页符匹配。等效于 \x0c 和 \cL。                             |
| \n            | 换行符匹配。等效于 \x0a 和 \cJ。                             |
| \r            | 匹配一个回车符。等效于 \x0d 和 \cM。                         |
| \s            | 匹配任何空白字符，包括空格、制表符、换页符等。与 [ \f\n\r\t\v] 等效。 |
| \S            | 匹配任何非空白字符。与 [^ \f\n\r\t\v] 等效。                 |
| \t            | 制表符匹配。与 \x09 和 \cI 等效。                            |
| \v            | 垂直制表符匹配。与 \x0b 和 \cK 等效。                        |
| \w            | 匹配任何字类字符，包括下划线。与"[A-Za-z0-9_]"等效。         |
| \W            | 与任何非单词字符匹配。与"[^A-Za-z0-9_]"等效。                |
| \x*n*         | 匹配 *n*，此处的 *n* 是一个十六进制转义码。十六进制转义码必须正好是两位数长。例如，"\x41"匹配"A"。"\x041"与"\x04"&"1"等效。允许在正则表达式中使用 ASCII 代码。 |
| \*num*        | 匹配 *num*，此处的 *num* 是一个正整数。到捕获匹配的反向引用。例如，"(.)\1"匹配两个连续的相同字符。 |
| \*n*          | 标识一个八进制转义码或反向引用。如果 \*n* 前面至少有 *n* 个捕获子表达式，那么 *n* 是反向引用。否则，如果 *n* 是八进制数 (0-7)，那么 *n* 是八进制转义码。 |
| \*nm*         | 标识一个八进制转义码或反向引用。如果 \*nm* 前面至少有 *nm* 个捕获子表达式，那么 *nm* 是反向引用。如果 \*nm* 前面至少有 *n* 个捕获，则 *n* 是反向引用，后面跟有字符 *m*。如果两种前面的情况都不存在，则 \*nm* 匹配八进制值 *nm*，其中 *n* 和 *m* 是八进制数字 (0-7)。 |
| \nml          | 当 *n* 是八进制数 (0-3)，*m* 和 *l* 是八进制数 (0-7) 时，匹配八进制转义码 *nml*。 |
| \u*n*         | 匹配 *n*，其中 *n* 是以四位十六进制数表示的 Unicode 字符。例如，\u00A9 匹配版权符号 (©)。 |

### 一、校验数字的表达式

 1 数字：^[0-9]*$

 2 n位的数字：^\d{n}$

 3 至少n位的数字：^\d{n,}$

 4m-n位的数字：^\d{m,n}$

 5 零和非零开头的数字：^(0|[1-9][0-9]*)$

 6 非零开头的最多带两位小数的数字：^([1-9][0-9]*)+(.[0-9]{1,2})?$

 7 带1-2位小数的正数或负数：^(\-)?\d+(\.\d{1,2})?$

 8 正数、负数、和小数：^(\-|\+)?\d+(\.\d+)?$

 9 有两位小数的正实数：^[0-9]+(.[0-9]{2})?$

10 有1~3位小数的正实数：^[0-9]+(.[0-9]{1,3})?$

11 非零的正整数：^[1-9]\d*$或 ^([1-9][0-9]*){1,3}$或 ^\+?[1-9][0-9]*$

12 非零的负整数：^\-[1-9][]0-9"*$或 ^-[1-9]\d*$

13 非负整数：^\d+$或 ^[1-9]\d*|0$

14 非正整数：^-[1-9]\d*|0$或 ^((-\d+)|(0+))$

15 非负浮点数：^\d+(\.\d+)?$或 ^[1-9]\d*\.\d*|0\.\d*[1-9]\d*|0?\.0+|0$

16 非正浮点数：^((-\d+(\.\d+)?)|(0+(\.0+)?))$或 ^(-([1-9]\d*\.\d*|0\.\d*[1-9]\d*))|0?\.0+|0$

17 正浮点数：^[1-9]\d*\.\d*|0\.\d*[1-9]\d*$或 ^(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*))$

18 负浮点数：^-([1-9]\d*\.\d*|0\.\d*[1-9]\d*)$或 ^(-(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*)))$

19 浮点数：^(-?\d+)(\.\d+)?$或 ^-?([1-9]\d*\.\d*|0\.\d*[1-9]\d*|0?\.0+|0)$

 

### 二、校验字符的表达式

 1 汉字：^[\u4e00-\u9fa5]{0,}

 2 英文和数字：^[A-Za-z0-9]+$或 ^[A-Za-z0-9]{4,40}$

 3 长度为3-20的所有字符：^.{3,20}$

 4 由26个英文字母组成的字符串：^[A-Za-z]+$

 5 由26个大写英文字母组成的字符串：^[A-Z]+$

 6 由26个小写英文字母组成的字符串：^[a-z]+$

 7 由数字和26个英文字母组成的字符串：^[A-Za-z0-9]+$

 8 由数字、26个英文字母或者下划线组成的字符串：^\w+$或 ^\w{3,20}$

 9 中文、英文、数字包括下划线：^[\u4E00-\u9FA5A-Za-z0-9_]+$

10 中文、英文、数字但不包括下划线等符号：^[\u4E00-\u9FA5A-Za-z0-9]+$或^[\u4E00-\u9FA5A-Za-z0-9]{2,20}$

11 可以输入含有^%&',;=?$"等字符：[^%&',;=?$\x22]+

12 禁止输入含有~的字符：[^~\x22]+

### 三、特殊需求表达式

 1Email地址：^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$

 2 域名：[a-zA-Z0-9][-a-zA-Z0-9]{0,62}(/.[a-zA-Z0-9][-a-zA-Z0-9]{0,62})+/.?

 3InternetURL：[a-zA-z]+://[^\s]*或 ^http://([\w-]+\.)+[\w-]+(/[\w-./?%&=]*)?$

 4 手机号码：^(13[0-9]|14[5|7]|15[0|1|2|3|5|6|7|8|9]|18[0|1|2|3|5|6|7|8|9])\d{8}$

 5 电话号码("XXX-XXXXXXX"、"XXXX-XXXXXXXX"、"XXX-XXXXXXX"、"XXX-XXXXXXXX"、"XXXXXXX"和"XXXXXXXX)：^(\(\d{3,4}-)|\d{3.4}-)?\d{7,8}$

 6 国内电话号码(0511-4405222、021-87888822)：\d{3}-\d{8}|\d{4}-\d{7}

 7 身份证号(15位、18位数字)：^\d{15}|\d{18}$

 8 短身份证号码(数字、字母x结尾)：^([0-9]){7,18}(x|X)?$或 ^\d{8,18}|[0-9x]{8,18}|[0-9X]{8,18}?$

 9 帐号是否合法(字母开头，允许5-16字节，允许字母数字下划线)：^[a-zA-Z][a-zA-Z0-9_]{4,15}$

10 密码(以字母开头，长度在6~18之间，只能包含字母、数字和下划线)：^[a-zA-Z]\w{5,17}$

11 强密码(必须包含大小写字母和数字的组合，不能使用特殊字符，长度在8-10之间)：^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,10}$ 

12 日期格式：^\d{4}-\d{1,2}-\d{1,2}

13 一年的12个月(01～09和1～12)：^(0?[1-9]|1[0-2])$

14 一个月的31天(01～09和1～31)：^((0?[1-9])|((1|2)[0-9])|30|31)$

15 钱的输入格式：

  1.有四种钱的表示形式我们可以接受:"10000.00" 和 "10,000.00", 和没有 "分" 的"10000" 和 "10,000"：^[1-9][0-9]*$

  2.这表示任意一个不以0开头的数字,但是,这也意味着一个字符"0"不通过,所以我们采用下面的形式：^(0|[1-9][0-9]*)$

  3.一个0或者一个不以0开头的数字.我们还可以允许开头有一个负号：^(0|-?[1-9][0-9]*)$

  4.这表示一个0或者一个可能为负的开头不为0的数字.让用户以0开头好了.把负号的也去掉,因为钱总不能是负的吧.下面我们要加的是说明可能的小数部分：^[0-9]+(.[0-9]+)?$

  5.必须说明的是,小数点后面至少应该有1位数,所以"10."是不通过的,但是 "10" 和"10.2" 是通过的：^[0-9]+(.[0-9]{2})?$

  6.这样我们规定小数点后面必须有两位,如果你认为太苛刻了,可以这样：^[0-9]+(.[0-9]{1,2})?$

  7.这样就允许用户只写一位小数.下面我们该考虑数字中的逗号了,我们可以这样：^[0-9]{1,3}(,[0-9]{3})*(.[0-9]{1,2})?$

  8.1到3个数字,后面跟着任意个 逗号+3个数字,逗号成为可选,而不是必须：^([0-9]+|[0-9]{1,3}(,[0-9]{3})*)(.[0-9]{1,2})?$

  备注：这就是最终结果了,别忘了"+"可以用"*"替代如果你觉得空字符串也可以接受的话(奇怪,为什么?)最后,别忘了在用函数时去掉去掉那个反斜杠,一般的错误都在这里

16 xml文件：^([a-zA-Z]+-?)+[a-zA-Z0-9]+\\.[x|X][m|M][l|L]$

17 中文字符的正则表达式：[\u4e00-\u9fa5]

18 双字节字符：[^\x00-\xff]  (包括汉字在内，可以用来计算字符串的长度(一个双字节字符长度计2，ASCII字符计1))

19 空白行的正则表达式：\n\s*\r  (可以用来删除空白行)

20 HTML标记的正则表达式：<(\S*?)[^>]*>.*?</\1>|<.*?/>  (网上流传的版本太糟糕，上面这个也仅仅能部分，对于复杂的嵌套标记依旧无能为力)

21 首尾空白字符的正则表达式：^\s*|\s*$或(^\s*)|(\s*$)  (可以用来删除行首行尾的空白字符(包括空格、制表符、换页符等等)，非常有用的表达式)

22 腾讯QQ号：[1-9][0-9]{4,}  (腾讯QQ号从10000开始)

23 中国邮政编码：[1-9]\d{5}(?!\d)  (中国邮政编码为6位数字)

24 IP地址：\d+\.\d+\.\d+\.\d+  (提取IP地址时有用)

25 IP地址：((?:(?:25[0-5]|2[0-4]\\d|[01]?\\d?\\d)\\.){3}(?:25[0-5]|2[0-4]\\d|[01]?\\d?\\d))  

### 四、常用的正则表达式

（1）  "^\d+$"　　//非负整数（正整数 + 0）

（2）  "^[0-9]*[1-9][0-9]*$"　　//正整数

（3）  "^((-\d+)|(0+))$"　　//非正整数（负整数 + 0）

（4）  "^-[0-9]*[1-9][0-9]*$"　　//负整数

（5）  "^-?\d+$"　　　　//整数

（6）  "^\d+(\.\d+)?$"　　//非负浮点数（正浮点数 + 0）

（7）  "^(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*))$"　　//正浮点数

（8）  "^((-\d+(\.\d+)?)|(0+(\.0+)?))$"　　//非正浮点数（负浮点数 + 0）

（9）  "^(-(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*)))$"　　//负浮点数

（10） "^(-?\d+)(\.\d+)?$"　　//浮点数

（11） "^[A-Za-z]+$"　　//由26个英文字母组成的字符串

（12） "^[A-Z]+$"　　//由26个英文字母的大写组成的字符串

（13） "^[a-z]+$"　　//由26个英文字母的小写组成的字符串

（14） "^[A-Za-z0-9]+$"　　//由数字和26个英文字母组成的字符串

（15） "^\w+$"　　//由数字、26个英文字母或者下划线组成的字符串

（16） "^[\w-]+(\.[\w-]+)*@[\w-]+(\.[\w-]+)+$"　　　　//email地址

（17） "^[a-zA-z]+://(\w+(-\w+)*)(\.(\w+(-\w+)*))*(\?\S*)?$"　　//url

（18） /^(d{2}|d{4})-((0([1-9]{1}))|(1[1|2]))-(([0-2]([1-9]{1}))|(3[0|1]))$/  // 年-月-日

（19） /^((0([1-9]{1}))|(1[1|2]))/(([0-2]([1-9]{1}))|(3[0|1]))/(d{2}|d{4})$/  // 月/日/年

（20） "^([w-.]+)@(([[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}.)|(([w-]+.)+))([a-zA-Z]{2,4}|[0-9]{1,3})(]?)$"  //Emil

（21） /^((\+?[0-9]{2,4}\-[0-9]{3,4}\-)|([0-9]{3,4}\-))?([0-9]{7,8})(\-[0-9]+)?$/   //电话号码

（22） "^(d{1,2}|1dd|2[0-4]d|25[0-5]).(d{1,2}|1dd|2[0-4]d|25[0-5]).(d{1,2}|1dd|2[0-4]d|25[0-5]).(d{1,2}|1dd|2[0-4]d|25[0-5])$"  //IP地址

（23）  

（24） 匹配中文字符的正则表达式： [\u4e00-\u9fa5]

（25） 匹配双字节字符(包括汉字在内)：[^\x00-\xff]

（26） 匹配空行的正则表达式：\n[\s| ]*\r

（27） 匹配HTML标记的正则表达式：/<(.*)>.*<\/\1>|<(.*) \/>/

（28） 匹配首尾空格的正则表达式：(^\s*)|(\s*$)

（29） 匹配Email地址的正则表达式：\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*

（30） 匹配网址URL的正则表达式：^[a-zA-z]+://(\\w+(-\\w+)*)(\\.(\\w+(-\\w+)*))*(\\?\\S*)?$

（31） 匹配帐号是否合法(字母开头，允许5-16字节，允许字母数字下划线)：^[a-zA-Z][a-zA-Z0-9_]{4,15}$

（32） 匹配国内电话号码：(\d{3}-|\d{4}-)?(\d{8}|\d{7})?

（33） 匹配腾讯QQ号：^[1-9]*[1-9][0-9]*$

（34） 元字符及其在正则表达式上下文中的行为：

（35） \ 将下一个字符标记为一个特殊字符、或一个原义字符、或一个后向引用、或一个八进制转义符。

（36） ^ 匹配输入字符串的开始位置。如果设置了 RegExp 对象的Multiline 属性，^ 也匹配 ’\n’ 或 ’\r’ 之后的位置。

（37） $ 匹配输入字符串的结束位置。如果设置了 RegExp 对象的Multiline 属性，$ 也匹配 ’\n’ 或 ’\r’ 之前的位置。

（38） * 匹配前面的子表达式零次或多次。

（39） + 匹配前面的子表达式一次或多次。+ 等价于 {1,}。

（40） ? 匹配前面的子表达式零次或一次。? 等价于 {0,1}。

（41） {n} n 是一个非负整数，匹配确定的n 次。

（42） {n,} n 是一个非负整数，至少匹配n 次。

（43） {n,m} m 和 n 均为非负整数，其中n <= m。最少匹配 n 次且最多匹配 m 次。在逗号和两个数之间不能有空格。

（44） ? 当该字符紧跟在任何一个其他限制符 (*, +, ?, {n}, {n,}, {n,m}) 后面时，匹配模式是非贪婪的。非贪婪模式尽可能少的匹配所搜索的字符串，而默认的贪婪模式则尽可能多的匹配所搜索的字符串。

（45） . 匹配除 "\n" 之外的任何单个字符。要匹配包括 ’\n’ 在内的任何字符，请使用象 ’[.\n]’ 的模式。

（46） (pattern) 匹配pattern 并获取这一匹配。

（47） (?:pattern) 匹配pattern 但不获取匹配结果，也就是说这是一个非获取匹配，不进行存储供以后使用。

（48） (?=pattern) 正向预查，在任何匹配 pattern 的字符串开始处匹配查找字符串。这是一个非获取匹配，也就是说，该匹配不需要获取供以后使用。

（49） (?!pattern) 负向预查，与(?=pattern)作用相反

（50） x|y 匹配 x 或 y。

（51） [xyz] 字符集合。

（52） [^xyz] 负值字符集合。

（53） [a-z] 字符范围，匹配指定范围内的任意字符。

（54） [^a-z] 负值字符范围，匹配任何不在指定范围内的任意字符。

（55） \b 匹配一个单词边界，也就是指单词和空格间的位置。

（56） \B 匹配非单词边界。

（57） \cx 匹配由x指明的控制字符。

（58） \d 匹配一个数字字符。等价于 [0-9]。

（59） \D 匹配一个非数字字符。等价于 [^0-9]。

（60） \f 匹配一个换页符。等价于 \x0c 和 \cL。

（61） \n 匹配一个换行符。等价于 \x0a 和 \cJ。

（62） \r 匹配一个回车符。等价于 \x0d 和 \cM。

（63） \s 匹配任何空白字符，包括空格、制表符、换页符等等。等价于[ \f\n\r\t\v]。

（64） \S 匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。

（65） \t 匹配一个制表符。等价于 \x09 和 \cI。

（66） \v 匹配一个垂直制表符。等价于 \x0b 和 \cK。

（67） \w 匹配包括下划线的任何单词字符。等价于’[A-Za-z0-9_]’。

（68） \W 匹配任何非单词字符。等价于 ’[^A-Za-z0-9_]’。

（69） \xn 匹配 n，其中 n 为十六进制转义值。十六进制转义值必须为确定的两个数字长。

（70） \num 匹配 num，其中num是一个正整数。对所获取的匹配的引用。

（71） \n 标识一个八进制转义值或一个后向引用。如果 \n 之前至少 n 个获取的子表达式，则 n 为后向引用。否则，如果 n 为八进制数字 (0-7)，则 n 为一个八进制转义值。

（72） \nm 标识一个八进制转义值或一个后向引用。如果 \nm 之前至少有is preceded by at least nm 个获取得子表达式，则 nm 为后向引用。如果 \nm 之前至少有 n 个获取，则 n 为一个后跟文字 m 的后向引用。如果前面的条件都不满足，若 n 和 m 均为八进制数字 (0-7)，则 \nm 将匹配八进制转义值 nm。

（73） \nml 如果 n 为八进制数字 (0-3)，且 m 和 l 均为八进制数字 (0-7)，则匹配八进制转义值 nml。

（74） \un 匹配 n，其中 n 是一个用四个十六进制数字表示的Unicode字符。

（75） 匹配中文字符的正则表达式： [u4e00-u9fa5]

（76） 匹配双字节字符(包括汉字在内)：[^x00-xff]

（77） 匹配空行的正则表达式：n[s| ]*r

（78） 匹配HTML标记的正则表达式：/<(.*)>.*</1>|<(.*) />/

（79） 匹配首尾空格的正则表达式：(^s*)|(s*$)

（80） 匹配Email地址的正则表达式：w+([-+.]w+)*@w+([-.]w+)*.w+([-.]w+)*

（81） 匹配网址URL的正则表达式：http://([w-]+.)+[w-]+(/[w- ./?%&=]*)?

（82） 利用正则表达式限制网页表单里的文本框输入内容：

（83） 用正则表达式限制只能输入中文：οnkeyup="value=value.replace(/[^u4E00-u9FA5]/g,'')" onbeforepaste="clipboardData.setData('text',clipboardData.getData('text').replace(/[^u4E00-u9FA5]/g,''))"

（84） 用正则表达式限制只能输入全角字符： οnkeyup="value=value.replace(/[^uFF00-uFFFF]/g,'')" onbeforepaste="clipboardData.setData('text',clipboardData.getData('text').replace(/[^uFF00-uFFFF]/g,''))"

（85） 用正则表达式限制只能输入数字：οnkeyup="value=value.replace(/[^d]/g,'') "onbeforepaste="clipboardData.setData('text',clipboardData.getData('text').replace(/[^d]/g,''))"

（86） 用正则表达式限制只能输入数字和英文：οnkeyup="value=value.replace(/[W]/g,'') "onbeforepaste="clipboardData.setData('text',clipboardData.getData('text').replace(/[^d]/g,''))"

（87） 整理：

（88） 匹配中文字符的正则表达式： [\u4e00-\u9fa5]

（89） 匹配双字节字符(包括汉字在内)：[^\x00-\xff]

（90） 匹配空行的正则表达式：\n[\s| ]*\r

（91） 匹配HTML标记的正则表达式：/<(.*)>.*<\/\1>|<(.*) \/>/

（92） 匹配首尾空格的正则表达式：(^\s*)|(\s*$)

（93） 匹配IP地址的正则表达式：/(\d+)\.(\d+)\.(\d+)\.(\d+)/g //

（94） 匹配Email地址的正则表达式：\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*

（95） 匹配网址URL的正则表达式：http://(/[\w-]+\.)+[\w-]+(/[\w- ./?%&=]*)?

（96） sql语句：^(select|drop|delete|create|update|insert).*$

（97） 非负整数：^\d+$

（98） 正整数：^[0-9]*[1-9][0-9]*$

（99） 非正整数：^((-\d+)|(0+))$

（100） 负整数：^-[0-9]*[1-9][0-9]*$

（101） 整数：^-?\d+$

（102） 非负浮点数：^\d+(\.\d+)?$

（103） 正浮点数：^((0-9)+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*))$

（104） 非正浮点数：^((-\d+\.\d+)?)|(0+(\.0+)?))$

（105） 负浮点数：^(-((正浮点数正则式)))$

（106） 英文字符串：^[A-Za-z]+$

（107） 英文大写串：^[A-Z]+$

（108） 英文小写串：^[a-z]+$

（109） 英文字符数字串：^[A-Za-z0-9]+$

（110） 英数字加下划线串：^\w+$

（111） E-mail地址：^[\w-]+(\.[\w-]+)*@[\w-]+(\.[\w-]+)+$

（112） URL：^[a-zA-Z]+://(\w+(-\w+)*)(\.(\w+(-\w+)*))*(\?\s*)?$

或：^http:\/\/[A-Za-z0-9]+\.[A-Za-z0-9]+[\/=\?%\-&_~`@[\]\':+!]*([^<>\"\"])*$

（113） 邮政编码：^[1-9]\d{5}$

（114） 中文：^[\u0391-\uFFE5]+$

（115） 电话号码：^((\d2,3\d2,3)|(\d{3}\-))?(0\d2,30\d2,3|0\d{2,3}-)?[1-9]\d{6,7}(\-\d{1,4})?$

（116） 手机号码：^((\d2,3\d2,3)|(\d{3}\-))?13\d{9}$

（117） 双字节字符(包括汉字在内)：^\x00-\xff

（118） 匹配首尾空格：(^\s*)|(\s*$)（像vbscript那样的trim函数）

（119） 匹配HTML标记：<(.*)>.*<\/\1>|<(.*) \/>

（120） 匹配空行：\n[\s| ]*\r

（121） 提取信息中的网络链接：(h|H)(r|R)(e|E)(f|F) *= *('|")?(\w|\\|\/|\.)+('|"| *|>)?

（122） 提取信息中的邮件地址：\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*

（123） 提取信息中的图片链接：(s|S)(r|R)(c|C) *= *('|")?(\w|\\|\/|\.)+('|"| *|>)?

（124） 提取信息中的IP地址：(\d+)\.(\d+)\.(\d+)\.(\d+)

（125） 提取信息中的中国手机号码：(86)*0*13\d{9}

（126） 提取信息中的中国固定电话号码：(\d3,4\d3,4|\d{3,4}-|\s)?\d{8}

（127） 提取信息中的中国电话号码（包括移动和固定电话）：(\d3,4\d3,4|\d{3,4}-|\s)?\d{7,14}

（128） 提取信息中的中国邮政编码：[1-9]{1}(\d+){5}

（129） 提取信息中的浮点数（即小数）：(-?\d*)\.?\d+

（130） 提取信息中的任何数字 ：(-?\d*)(\.\d+)?

（131） IP：(\d+)\.(\d+)\.(\d+)\.(\d+)

（132） 电话区号：/^0\d{2,3}$/

（133） 腾讯QQ号：^[1-9]*[1-9][0-9]*$

（134） 帐号(字母开头，允许5-16字节，允许字母数字下划线)：^[a-zA-Z][a-zA-Z0-9_]{4,15}$

（135） 中文、英文、数字及下划线：^[\u4e00-\u9fa5_a-zA-Z0-9]+$ 





#  流(Stream)、文件(File)和IO

## 概述

![29029551d772d6952c1edcf333cf306b.png](高级知识点.assets/29029551d772d6952c1edcf333cf306b.png)

Java.io 包几乎包含了所有操作输入、输出需要的类。所有这些流类代表了输入源和输出目标。

Java.io 包中的流支持很多种格式，比如：基本类型、对象、本地化字符集等等。

一个流可以理解为一个数据的序列。输入流表示从一个源读取数据，输出流表示向一个目标写数据。

Java 为 I/O 提供了强大的而灵活的支持，使其更广泛地应用到文件传输和网络编程中。

### IO流的分类

![7476e49e15a304de8ec44134b68d0f5f.png](高级知识点.assets/7476e49e15a304de8ec44134b68d0f5f.png)

#### 	按照流的方向进行分类

​		以内存作为参照物，
​			往内存中去，叫做输入(Input)。或者叫做读(Read)。
​			从内存中出来，叫做输出(Output)。或者叫做写(Write)。

#### 	按照读取数据方式不同进行分类

​		**按照字节的方式读取数据**，一次读取1个字节byte，等同于一次读取8个二进制位。
​		这种流是万能的，什么类型的文件都可以读取。包括：文本文件，图片，声音文件，视频文件等....
​			假设文件file1.txt，采用字节流的话是这样读的：
​				a中国bc张三fe
​				第一次读：一个字节，正好读到'a'
​				第二次读：一个字节，正好读到'中'字符的一半。
​				第三次读：一个字节，正好读到'中'字符的另外一半。

​		**按照字符的方式读取数据**，一次读取一个字符，这种流是为了方便读取普通文本文件而存在的，这种流不能读取：图片、声音、视频等文件。只能读取纯文本文件，连word文件都无法读取。
​			假设文件file1.txt，采用字符流的话是这样读的：
​				a中国bc张三fe
​				第一次读：'a'字符（'a'字符在windows系统中占用1个字节。）
​				第二次读：'中'字符（'中'字符在windows系统中占用2个字节。）

> 综上所述：流的分类
> 							输入流、输出流
> 							字节流、字符流

### java.io包下需要掌握的流有16个

#### 文件专属：

​	java.io.FileInputStream（掌握）
​	java.io.FileOutputStream（掌握）
​	java.io.FileReader
​	java.io.FileWriter

#### 转换流：（将字节流转换成字符流）

​	java.io.InputStreamReader
​	java.io.OutputStreamWriter

#### 缓冲流专属：

​	java.io.BufferedReader
​	java.io.BufferedWriter
​	java.io.BufferedInputStream
​	java.io.BufferedOutputStream

#### 数据流专属：

​	java.io.DataInputStream
​	java.io.DataOutputStream

#### 标准输出流：

​	java.io.PrintWriter
​	java.io.PrintStream（掌握）

#### 对象专属流：

​	java.io.ObjectInputStream（掌握）
​	java.io.ObjectOutputStream（掌握）



##  IO流的四大家族

> ​	四大家族的**首领**：
> ​	java.io.InputStream  		字节输入流
> ​	java.io.OutputStream 	  字节输出流
>
> ​	java.io.Reader				  字符输入流
> ​	java.io.Writer				   字符输出流
>
> ​	四大家族的首领都是**抽象类。(abstract class)**

​	![9ac0bcce60d751888ba69b3013510858.png](高级知识点.assets/9ac0bcce60d751888ba69b3013510858.png)

所有的流都实现了：
		java.io.Closeable接口，都是可关闭的，都有**close()方法**。流毕竟是一个管道，这个是内存和硬盘之间的通道，用完之后**一定要关闭**，不然会耗费(占用)很多资源。养成好习惯，用完流一定要关闭。

​	所有的输出流都实现了：
​		java.io.Flushable接口，都是可刷新的，都有**flush()方法**。
​		养成一个好习惯，输出流在最终输出之后，**一定**要**记得flush()**
​		刷新一下。这个刷新表示将通道/管道当中剩余未输出的数据，强行输出完（清空管道！）刷新的作用就是清空管道。
​		注意：**如果没有flush()可能会导致丢失数据。**

> 注意：在java中只要“类名”以Stream结尾的都是**字节**流。
>
> ​											  以“Reader/Writer”结尾的都是**字符**流。



### FileInputStream

  FileInputStream流被称为文件字节输入流，意思指对文件数据以字节的形式进行读取操作如读取图片视频等

#### 构造方法

 **1）通过打开与File类对象代表的实际文件的链接来创建FileInputStream流对象**

```java
public FileInputStream(File file) throws FileNotFoundException{}
```

 若File类对象的所代表的文件不存在;不是文件是目录;或者其他原因不能打开的话，则会抛出`FileNotFoundException`

```java
    /* * 运行会产生异常并被扑捉--因为不存在xxxxxxxx这样的文件
     */
public static void main(String[] args)
    {
        File file=new File("xxxxxxxx"); //根据路径创建File类对象--这里路径即使错误也不会报错，因为只是产生File对象，还并未与计算机文件读写有关联
        
        try
        {
            FileInputStream fileInputStream=new FileInputStream(file);//与根据File类对象的所代表的实际文件建立链接创建fileInputStream对象
        }
        catch (FileNotFoundException e)
        {
           System.out.println("文件不存在或者文件不可读或者文件是目录");
        } 
    }	
```

***2）通过指定的字符串参数来创建File类对象，而后再与File对象所代表的实际路径建立链接创建FileInputStream流对象***

```java
public FileInputStream(String name) throws FileNotFoundException
```

​      通过源码发现该构造方法等于是在第一个构造方法的基础上进行延伸的，因此规则也和第一个构造方法一致

```java
    public FileInputStream(String name) throws FileNotFoundException {
        this(name != null ? new File(name) : null);
    }
```

***3）该构造方法没有理解---查看api是指使用的fdObj文件描述符来作为参数，文件描述符是指与计算机系统中的文件的连接，前面两个方法的源码中最后都是利用文件描述符来建立连接的***

```java
public FileInputStream(FileDescriptor fdObj)
```

#### FileInputStream常用API

***1）从输入流中读取一个字节返回int型变量，若到达文件末尾，则返回-1***

```java
public int read() throws IOException
```

  **理解读取的字节为什么返回int型变量**

   1、方法解释中的-1相当于是数据字典告诉调用者文件已到底，可以结束读取了，这里的-1是Int型

   2、那么当文件未到底时，我们读取的是字节，若返回byte类型，那么势必造成同一方法返回类型不同的情况这是不允许的

   3、我们读取的字节实际是由8位二进制组成，二进制文件不利于直观查看，可以转成常用的十进制进行展示，因此需要把读取的字节从二进制转成十进制整数，故返回int型

  4、 因此结合以上3点，保证返回类型一致以及直观查看的情况，因此该方法虽然读取的是字节但返回int型

```java
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
 
public class FileStream{
    public static void main(String[] args)
    {
        //建立文件对象
        File file=new File("C:\\Users\\Administrator\\Desktop\\1.txt"); 
        try
        {
            //建立链接
            FileInputStream fileInputStream=new FileInputStream(file);
            int  n=0;  
            StringBuffer sBuffer=new StringBuffer();
 
            while (n!=-1)  //当n不等于-1,则代表未到末尾
            {           
               n=fileInputStream.read();//读取文件的一个字节(8个二进制位),并将其由二进制转成十进制的整数返回       
               char by=(char) n; //转成字符       
               sBuffer.append(by);
            }
           System.out.println(sBuffer.toString()); 
        }
        catch (FileNotFoundException e)
        {       
           System.out.println("文件不存在或者文件不可读或者文件是目录");
        }
        catch (IOException e)
        {
           System.out.println("读取过程存在异常");
        } 
    }
}
```

***2）从输入流中读取b.length个字节到字节数组中，返回读入缓冲区的总字节数，若到达文件末尾，则返回-1***

```java
public int read(byte b[]) throws IOException {
        return readBytes(b, 0, b.length);
    }
```

  **1.** 我们先设定一个缓冲区即字节数组用于存储从流中读取的字节数据，该数组的长度为N

2. 那么就是从流中读取N个字节到字节数组中。但是注意返回的是读入的总字节数而并不是N，说明有的时候实际读入的总字节数不一定等于数组的长度

3. 文件的内容是12345.那么流中一共有5个字节，但是我们设定的字节数组长度为2.那么会读取几次？每次情况是怎么样的？

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
/*
最终版，需要掌握。
 */
public class FileInputStreamTest04 {
    public static void main(String[] args) {
        FileInputStream fis = null;
        try {
            fis = new FileInputStream("chapter23/src/tempfile3");
            // 准备一个byte数组
            byte[] bytes = new byte[4];
            /*while(true){
                int readCount = fis.read(bytes);
                if(readCount == -1){
                    break;
                }
                // 把byte数组转换成字符串，读到多少个转换多少个。
                System.out.print(new String(bytes, 0, readCount));
            }*/
            int readCount = 0;
            while((readCount = fis.read(bytes)) != -1) {
                System.out.print(new String(bytes, 0, readCount));
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fis != null) {
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

> 使用read(byte b[])方法时的数组**长度至关重要**，若长度小于流的字节长度，那么最后得出的内容会出现丢失。若大于流的字节长度，那么最后数组的内存就浪费了，那么就需要根据文件的字节长度来设置数组的长度

```java
byte[] b=new byte[(int) file.length()];
```





------

### FileOutputStream

***FileOutputStream ：字节输出流的操作步骤、write方法\****

####  字节输出流操作步骤

 A: 创建字节输出流对象
 B: 调用write( )方法
 C: 释放资源

>  \* public void write(int b) :写一个字节
>  \* public void write(byte[] b) :写一个字节数组
>  \* public void write(byte[] b,int off,int len) :写一个字节数组的一部分



```java
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
public class FileOutputStreamTest01 {
    public static void main(String[] args) {
        FileOutputStream fos = null;
        try {
            // myfile文件不存在的时候会自动新建！
            // 这种方式谨慎使用，这种方式会先将原文件清空，然后重新写入。
            fos1= new FileOutputStream("myfile");
            // 以追加的方式在文件末尾写入。不会清空原文件内容。
            fos = new FileOutputStream("chapter23/src/tempfile3", true);
            // 开始写。
            byte[] bytes = {97, 98, 99, 100};
            // 将byte数组全部写出！
            fos.write(bytes); // abcd
            // 将byte数组的一部分写出！
            fos.write(bytes, 0, 2); // 再写出ab

            // 字符串
            String s = "我是一个中国人，我骄傲！！！";
            // 将字符串转换成byte数组。
            byte[] bs = s.getBytes();
            // 写
            fos.write(bs);

            // 写完之后，最后一定要刷新
            fos.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fos != null) {
                try {
                    //关闭
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

### FileReader

​    文件字符输入流，只能读取普通文本。读取文本内容时，比较方便，快捷。

常用构造方法：

- FileReader(File file)
- FileReader(String fileName)

常用的4个

- read(char[] chars)
- read(char[] cbuf,int off,int len)
- skip(long n)
- close()

```java
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
public class FileReaderTest {
    public static void main(String[] args) {
        FileReader reader = null;
        try {
            // 创建文件字符输入流
            reader = new FileReader("tempfile");
            //准备一个char数组
            char[] chars = new char[4];
            // 往char数组中读
            reader.read(chars); // 按照字符的方式读取：第一次e，第二次f，第三次 风....
            for(char c : chars) {
                System.out.println(c);
            }

            /*// 开始读
            char[] chars = new char[4]; // 一次读取4个字符
            int readCount = 0;
            while((readCount = reader.read(chars)) != -1) {
                System.out.print(new String(chars,0,readCount));
            }*/
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (reader != null) {
                try {
                    reader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

### FileWriter

#### 常用构造方法

> - FileWriter(File file)
> - FileWriter(File file, boolean append)
> - FileWriter(String fileName)
> - FileWriter(String fileName, boolean append)

#### 常用的6个方法

> - write(char[] cbuf)
> - write(String str)
> - write(String str,int off,int len)
> - write(char[] cbuf,int off,int len)
> - close()
> - flush()

```java
import java.io.FileWriter;
import java.io.IOException;
public class FileWriterTest {
    public static void main(String[] args) {
        FileWriter out = null;
        try {
            // 创建文件字符输出流对象
            //out = new FileWriter("file");
            out = new FileWriter("file", true);

            // 开始写。
            char[] chars = {'我','是','中','国','人'};
            out.write(chars);
            out.write(chars, 2, 3);

            out.write("我是一名java软件工程师！");
            // 写出一个换行符。
            out.write("\n");
            out.write("hello world!");

            // 刷新
            out.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (out != null) {
                try {
                    out.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```



## 缓冲流

### BufferedReader

​    带有缓冲区的字符输入流。
​    使用这个流的时候不需要自定义char数组，或者说不需要自定义byte数组。自带缓冲。

```java
import java.io.BufferedReader;
import java.io.FileReader;
public class BufferedReaderTest01 {
    public static void main(String[] args) throws Exception{

        FileReader reader = new FileReader("Copy02.java");
        // 当一个流的构造方法中需要一个流的时候，这个被传进来的流叫做：节点流。
        // 外部负责包装的这个流，叫做：包装流，还有一个名字叫做：处理流。
        // 像当前这个程序来说：FileReader就是一个节点流。BufferedReader就是包装流/处理流。
        BufferedReader br = new BufferedReader(reader);
        // 读一行
        /*String firstLine = br.readLine();
        System.out.println(firstLine);
        String secondLine = br.readLine();
        System.out.println(secondLine);
        String line3 = br.readLine();
        System.out.println(line3);*/

        // br.readLine()方法读取一个文本行，但不带换行符。
        String s = null;
        while((s = br.readLine()) != null){
            System.out.print(s);
        }

        // 关闭流
        // 对于包装流来说，只需要关闭最外层流就行，里面的节点流会自动关闭。（可以看源代码。）
        br.close();
    }
}
```

#### 读取控制台输入

Java 的控制台输入由 System.in 完成。

为了获得一个绑定到控制台的字符流，你可以把 System.in 包装在一个 BufferedReader 对象中来创建一个字符流。

下面是创建 BufferedReader 的基本语法：

```java
BufferedReader br = new BufferedReader(new
                       InputStreamReader(System.in));
```

BufferedReader 对象创建后，我们便可以使用 read() 方法从控制台读取一个字符，或者用 readLine() 方法读取一个字符串。

------

#### 从控制台读取多字符输入

从 BufferedReader 对象读取一个字符要使用 read() 方法，它的语法如下：

```Java
int read( ) throws IOException
```

每次调用 read() 方法，它从输入流读取一个字符并把该字符作为整数值返回。 当流结束的时候返回 -1。该方法抛出 IOException。

下面的程序示范了用 read() 方法从控制台不断读取字符直到用户输入 "q"。

```java
//使用 BufferedReader 在控制台读取字符 
import java.io.* 
public class BRRead {
    public static void main(String args[]) throws IOException {
        char c;
        // 使用 System.in 创建 BufferedReader
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        System.out.println("输入字符, 按下 'q' 键退出。");
        // 读取字符
        do {
            c = (char) br.read();
            System.out.println(c);
        } while (c != 'q');
    }
}
```

#### 从控制台读取字符串

从标准输入读取一个字符串需要使用 BufferedReader 的 readLine() 方法。

它的一般格式是：

```Java
String readLine( ) throws IOException
```

下面的程序读取和显示字符行直到你输入了单词"end"。

```java
//使用 BufferedReader 在控制台读取字符
import java.io.*;
 
public class BRReadLines {
    public static void main(String args[]) throws IOException {
        // 使用 System.in 创建 BufferedReader
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str;
        System.out.println("Enter lines of text.");
        System.out.println("Enter 'end' to quit.");
        do {
            str = br.readLine();
            System.out.println(str);
        } while (!str.equals("end"));
    }
}
```

### BufferedWriter

带有缓冲的字符输出流

```java
import java.io.BufferedWriter;
import java.io.FileOutputStream;
import java.io.FileWriter;
import java.io.OutputStreamWriter;
public class BufferedWriterTest {
    public static void main(String[] args) throws Exception{
        // 带有缓冲区的字符输出流
        //BufferedWriter out = new BufferedWriter(new FileWriter("copy"));

        BufferedWriter out = new BufferedWriter(new OutputStreamWriter(new FileOutputStream("copy", true)));
        // 开始写。
        out.write("hello world!");
        out.write("\n");
        out.write("hello kitty!");
        // 刷新
        out.flush();
        // 关闭最外层
        out.close();
    }
}
```




​	第二：IO流+Properties集合的联合使用。

1、拷贝目录。

2、关于对象流
	ObjectInputStream
	ObjectOutputStream

重点：
	参与序列化的类型必须实现java.io.Serializable接口。
	并且建议将序列化版本号手动的写出来。
		private static final long serialVersionUID = 1L;

3、IO + Properties联合使用。
	IO流：文件的读和写。
	Properties:是一个Map集合，key和value都是String类型。







## 转化流

### InputStreamReader

```java
import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.InputStream;
import java.io.InputStreamReader;

public class BufferedReaderTest02 {
    public static void main(String[] args) throws Exception{

        /*// 字节流
        FileInputStream in = new FileInputStream("Copy02.java");

        // 通过转换流转换（InputStreamReader将字节流转换成字符流。）
        // in是节点流。reader是包装流。
        InputStreamReader reader = new InputStreamReader(in);

        // 这个构造方法只能传一个字符流。不能传字节流。
        // reader是节点流。br是包装流。
        BufferedReader br = new BufferedReader(reader);*/

        // 合并
        BufferedReader br = new BufferedReader(new InputStreamReader(new FileInputStream("Copy02.java")));

        String line = null;
        while((line = br.readLine()) != null){
            System.out.println(line);
        }
        // 关闭最外层
        br.close();
    }
}
```

### OutputStreamWriter





## IO+Properties的联合应用

```java
import java.io.FileReader;
import java.util.Properties;
/*
非常好的一个设计理念：
    以后经常改变的数据，可以单独写到一个文件中，使用程序动态读取。将来只需要修改这个文件的内容，java代码不需要改动，不需要重新编译，服务器也不需要重启。就可以拿到动态的信息。

    类似于以上机制的这种文件被称为配置文件。
    并且当配置文件中的内容格式是：
        key1=value
        key2=value
    的时候，我们把这种配置文件叫做属性配置文件。

    java规范中有要求：属性配置文件建议以.properties结尾，但这不是必须的。这种以.properties结尾的文件在java中被称为：属性配置文件。
    其中Properties是专门存放属性配置文件内容的一个类。
 */
public class IoPropertiesTest01 {
    public static void main(String[] args) throws Exception{
        /*
        Properties是一个Map集合，key和value都是String类型。
        想将userinfo文件中的数据加载到Properties对象当中。
         */
        // 新建一个输入流对象
        FileReader reader = new FileReader("chapter23/userinfo.properties");

        // 新建一个Map集合
        Properties pro = new Properties();

        // 调用Properties对象的load方法将文件中的数据加载到Map集合中。
        pro.load(reader); // 文件中的数据顺着管道加载到Map集合中，其中等号=左边做key，右边做value

        // 通过key来获取value呢？
        String username = pro.getProperty("username");
        System.out.println(username);

        String password = pro.getProperty("password");
        System.out.println(password);

        String data = pro.getProperty("data");
        System.out.println(data);

        String usernamex = pro.getProperty("usernamex");
        System.out.println(usernamex);
    }
}
```



## PrintStream

标准的字节输出流。默认输出到控制台。标准输出流**不需要手动close()关闭**。**可以改变标准输出流的输出方向**

```java
import java.io.FileOutputStream;
import java.io.PrintStream;
public class PrintStreamTest {
    public static void main(String[] args) throws Exception{
        // 联合起来写
        System.out.println("hello world!");

        // 分开写
        PrintStream ps = System.out;
        ps.println("hello zhangsan");
        ps.println("hello lisi");
        ps.println("hello wangwu");

        /*
        // 这些是之前System类使用过的方法和属性。
        System.gc();
        System.currentTimeMillis();
        PrintStream ps2 = System.out;
        System.exit(0);
        System.arraycopy(....);
         */

        // 标准输出流不再指向控制台，指向“log”文件。
        PrintStream printStream = new PrintStream(new FileOutputStream("log"));
        // 修改输出方向，将输出方向修改到"log"文件。
        System.setOut(printStream);
        // 再输出
        System.out.println("hello world");
        System.out.println("hello kitty");
        System.out.println("hello zhangsan");

    }
}
```









## 目录

#### 创建目录：

File类中有两个方法可以用来创建文件夹：

- **mkdir( )**方法创建一个文件夹，成功则返回true，失败则返回false。失败表明File对象指定的路径已经存在，或者由于整个路径还不存在，该文件夹不能被创建。
- **mkdirs()**方法创建一个文件夹和它的所有父文件夹。

创建 "/tmp/user/java/bin"文件夹：

```java
import java.io.File;
 
public class CreateDir {
    public static void main(String args[]) {
        String dirname = "/tmp/user/java/bin";
        File d = new File(dirname);
        // 现在创建目录
        d.mkdirs();
    }
}
```



**注意：** Java 在 UNIX 和 Windows 自动按约定分辨文件路径分隔符。如果你在 Windows 版本的 Java 中使用分隔符 (/) ，路径依然能够被正确解析。

------

#### 读取目录

一个目录其实就是一个 File 对象，它包含其他文件和文件夹。

如果创建一个 File 对象并且它是一个目录，那么调用 isDirectory() 方法会返回 true。

可以通过调用该对象上的 list() 方法，来提取它包含的文件和文件夹的列表。

下面展示的例子说明如何使用 list() 方法来检查一个文件夹中包含的内容：

```java
import java.io.File;
 
public class DirList {
    public static void main(String args[]) {
        String dirname = "/tmp";
        File f1 = new File(dirname);
        if (f1.isDirectory()) {
            System.out.println("目录 " + dirname);
            String s[] = f1.list();
            for (int i = 0; i < s.length; i++) {
                File f = new File(dirname + "/" + s[i]);
                if (f.isDirectory()) {
                    System.out.println(s[i] + " 是一个目录");
                } else {
                    System.out.println(s[i] + " 是一个文件");
                }
            }
        } else {
            System.out.println(dirname + " 不是一个目录");
        }
    }
}
```



#### 删除目录或文件

删除文件可以使用 **java.io.File.delete()** 方法。

以下代码会删除目录 **/tmp/java/**，需要注意的是当删除某一目录时，必须保证该目录下没有其他文件才能正确删除，否则将删除失败。

测试目录结构：

```
/tmp/java/
|-- 1.log
|-- test
```

```java
import java.io.File;
 
public class DeleteFileDemo {
    public static void main(String args[]) {
        // 这里修改为自己的测试目录
        File folder = new File("/tmp/java/");
        deleteFolder(folder);
    }
    // 删除文件及目录
    public static void deleteFolder(File folder) {
        File[] files = folder.listFiles();
        if (files != null) {
            for (File f : files) {
                if (f.isDirectory()) {
                    deleteFolder(f);
                } else {
                    f.delete();
                }
            }
        }
        folder.delete();
    }
}
```



## Scanner 类

```java
Scanner s = new Scanner(System.in);
```

通过 Scanner 类的 **next()** 与 nextLine() 方法获取输入的字符串，在读取前我们一般需要 使用 hasNext 与 hasNextLine 判断是否还有输入的数据：

#### 使用 next 方法：

```java
import java.util.Scanner; 
 
public class ScannerDemo {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        // 从键盘接收数据
 
        // next方式接收字符串
        System.out.println("next方式接收：");
        // 判断是否还有输入
        if (scan.hasNext()) {
            String str1 = scan.next();
            System.out.println("输入的数据为：" + str1);
        }
        scan.close();
    }
}
```

#### 使用 nextLine 方法：

```java
import java.util.Scanner;
 
public class ScannerDemo {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        // 从键盘接收数据
 
        // nextLine方式接收字符串
        System.out.println("nextLine方式接收：");
        // 判断是否还有输入
        if (scan.hasNextLine()) {
            String str2 = scan.nextLine();
            System.out.println("输入的数据为：" + str2);
        }
        scan.close();
    }
}
```

### next() 与 nextLine() 区别

next():

- 1、一定要读取到有效字符后才可以结束输入。
- 2、对输入有效字符之前遇到的空白，next() 方法会自动将其去掉。
- 3、只有输入有效字符后才将其后面输入的空白作为分隔符或者结束符。
- next() 不能得到带有空格的字符串。

nextLine()：

- 1、以Enter为结束符,也就是说 nextLine()方法返回的是输入回车之前的所有字符。
- 2、可以获得空白。





# 异常处理

要理解Java异常处理是如何工作的，你需要掌握以下三种类型的异常：

- **检查性异常：**最具代表的检查性异常是用户错误或问题引起的异常，这是程序员无法预见的。例如要打开一个不存在文件时，一个异常就发生了，这些异常在编译时不能被简单地忽略。
- **运行时异常：** 运行时异常是可能被程序员避免的异常。与检查性异常相反，运行时异常可以在编译时被忽略。
- **错误：** 错误不是异常，而是脱离程序员控制的问题。错误在代码中通常被忽略。例如，当栈溢出时，一个错误就发生了，它们在编译也检查不到的。

## Exception 类的层次

所有的异常类是从 java.lang.Exception 类继承的子类。

Exception 类是 Throwable 类的子类。除了Exception类外，Throwable还有一个子类Error 。

Java 程序通常不捕获错误。错误一般发生在严重故障时，它们在Java程序处理的范畴之外。

Error 用来指示运行时环境发生的错误。例如，JVM 内存溢出。一般地，程序不会从错误中恢复。

![image-20210320223649609](高级知识点.assets/image-20210320223649609.png)

在 Java 内置类中(接下来会说明)，有大部分常用检查性和非检查性异常。



## 处理方法

### 处理异常的第一种方式：

​    在方法声明的位置上使用throws关键字抛出，谁调用我这个方法，我就抛给谁。抛给调用者来处理。
​    这种处理异常的态度：上报。

### 处理异常的第二种方式：

​    使用try..catch语句对异常进行捕捉。这个异常不会上报，自己把这个事儿处理了。
​    异常抛到此处为止，不再上抛了。

#### 注意：

​    只要异常没有捕捉，采用上报的方式，此方法的后续代码不会执行。另外需要注意，try语句块中的某一行出现异常，该行后面的代码不会执行。
​    **try..catch捕捉异常之后，后续代码可以执行。**

在以后的开发中，处理编译时异常，应该上报还是捕捉呢，怎么选？
    如果希望**调用者**来处理，选择**throws**上报。其它情况使用捕捉的方式。

一般**不建议在main方法上使用throws**，因为这个异常如果真正的发生了，一定会抛给JVM。JVM只有终止。 异常处理机制的作用就是增强程序的健壮性。怎么能做到，异常发生了也不影响程序的执行。所以一般main方法中的异常建议使用try..catch进行捕捉。main就不要继续上抛了。

## Java 内置异常类

Java 语言定义了一些异常类在 java.lang 标准包中。

标准运行时异常类的子类是最常见的异常类。由于 java.lang 包是默认加载到所有的 Java 程序的，所以大部分从运行时异常类继承而来的异常都可以直接使用。

Java 根据各个类库也定义了一些其他的异常，下面的表中列出了 Java 的非检查性异常。

| **异常**                        | **描述**                                                     |
| :------------------------------ | :----------------------------------------------------------- |
| ArithmeticException             | 当出现异常的运算条件时，抛出此异常。例如，一个整数"除以零"时，抛出此类的一个实例。 |
| ArrayIndexOutOfBoundsException  | 用非法索引访问数组时抛出的异常。如果索引为负或大于等于数组大小，则该索引为非法索引。 |
| ArrayStoreException             | 试图将错误类型的对象存储到一个对象数组时抛出的异常。         |
| ClassCastException              | 当试图将对象强制转换为不是实例的子类时，抛出该异常。         |
| IllegalArgumentException        | 抛出的异常表明向方法传递了一个不合法或不正确的参数。         |
| IllegalMonitorStateException    | 抛出的异常表明某一线程已经试图等待对象的监视器，或者试图通知其他正在等待对象的监视器而本身没有指定监视器的线程。 |
| IllegalStateException           | 在非法或不适当的时间调用方法时产生的信号。换句话说，即 Java 环境或 Java 应用程序没有处于请求操作所要求的适当状态下。 |
| IllegalThreadStateException     | 线程没有处于请求操作所要求的适当状态时抛出的异常。           |
| IndexOutOfBoundsException       | 指示某排序索引（例如对数组、字符串或向量的排序）超出范围时抛出。 |
| NegativeArraySizeException      | 如果应用程序试图创建大小为负的数组，则抛出该异常。           |
| NullPointerException            | 当应用程序试图在需要对象的地方使用 `null` 时，抛出该异常     |
| NumberFormatException           | 当应用程序试图将字符串转换成一种数值类型，但该字符串不能转换为适当格式时，抛出该异常。 |
| SecurityException               | 由安全管理器抛出的异常，指示存在安全侵犯。                   |
| StringIndexOutOfBoundsException | 此异常由 `String` 方法抛出，指示索引或者为负，或者超出字符串的大小。 |
| UnsupportedOperationException   | 当不支持请求的操作时，抛出该异常。                           |

下面的表中列出了 Java 定义在 java.lang 包中的检查性异常类。

| **异常**                   | **描述**                                                     |
| :------------------------- | :----------------------------------------------------------- |
| ClassNotFoundException     | 应用程序试图加载类时，找不到相应的类，抛出该异常。           |
| CloneNotSupportedException | 当调用 `Object` 类中的 `clone` 方法克隆对象，但该对象的类无法实现 `Cloneable` 接口时，抛出该异常。 |
| IllegalAccessException     | 拒绝访问一个类的时候，抛出该异常。                           |
| InstantiationException     | 当试图使用 `Class` 类中的 `newInstance` 方法创建一个类的实例，而指定的类对象因为是一个接口或是一个抽象类而无法实例化时，抛出该异常。 |
| InterruptedException       | 一个线程被另一个线程中断，抛出该异常。                       |
| NoSuchFieldException       | 请求的变量不存在                                             |
| NoSuchMethodException      | 请求的方法不存在                                             |

------

```java
/*
java语言中异常是以什么形式存在的呢？
    1、异常在java中以类的形式存在，每一个异常类都可以创建异常对象。
    2、异常对应的现实生活中是怎样的？
        火灾(异常类)：
            2008年8月8日,小明家着火了（异常对象）
            2008年8月9日,小刚家着火了（异常对象）
            2008年9月8日,小红家着火了（异常对象）

        类是：模板。
        对象是：实际存在的个体。

        钱包丢了（异常类）：
            2008年1月8日，小明的钱包丢了（异常对象）
            2008年1月9日，小芳的钱包丢了（异常对象）
            ....
 */
public class ExceptionTest02 {
    public static void main(String[] args) {

        // 通过“异常类”实例化“异常对象”
        NumberFormatException nfe = new NumberFormatException("数字格式化异常！");

        // java.lang.NumberFormatException: 数字格式化异常！
        System.out.println(nfe);

        // 通过“异常类”创建“异常对象”
        NullPointerException npe = new NullPointerException("空指针异常发生了！");

        //java.lang.NullPointerException: 空指针异常发生了！
        System.out.println(npe);
    }
}

```



## 异常常用方法

下面的列表是 Throwable 类的主要方法:

| **序号** | **方法及说明**                                               |
| :------- | :----------------------------------------------------------- |
| 1        | **public String getMessage()** 返回关于发生的异常的详细信息。这个消息在Throwable 类的构造函数中初始化了。 |
| 2        | **public Throwable getCause()** 返回一个Throwable 对象代表异常原因。 |
| 3        | **public String toString()** 使用getMessage()的结果返回类的串级名字。 |
| 4        | **public void printStackTrace()** 打印toString()结果和栈层次到System.err，即错误输出流。 |
| 5        | **public StackTraceElement [] getStackTrace()** 返回一个包含堆栈层次的数组。下标为0的元素代表栈顶，最后一个元素代表方法调用堆栈的栈底。 |
| 6        | **public Throwable fillInStackTrace()** 用当前的调用栈层次填充Throwable 对象栈层次，添加到栈层次任何先前信息中。 |

异常对象有**两个非常重要的方法**：

```java
获取异常简单的描述信息：
    String msg = exception.getMessage();
打印异常追踪的堆栈信息：
    exception.printStackTrace();
```



## 异常捕获

### 捕获异常

使用 try 和 catch 关键字可以捕获异常。try/catch 代码块放在异常可能发生的地方。

try/catch代码块中的代码称为保护代码，使用 try/catch 的语法如下：

```java
try
{
   // 程序代码
}catch(ExceptionName e1)
{
   //Catch 块
}
```

### 多重捕获块

一个 try 代码块后面跟随多个 catch 代码块的情况就叫多重捕获。

多重捕获块的语法如下所示：

```java
try{
   // 程序代码
}catch(异常类型1 异常的变量名1){
  // 程序代码
}catch(异常类型2 异常的变量名2){
  // 程序代码
}catch(异常类型3 异常的变量名3){
  // 程序代码
}
```

上面的代码段包含了 3 个 catch块。可以在 try 语句后面添加任意数量的 catch 块。

如果保护代码中发生异常，异常被抛给第一个 catch 块。如果抛出异常的数据类型与 ExceptionType1 匹配，它在这里就会被捕获。

如果不匹配，它会被传递给第二个 catch 块。如此，直到异常被捕获或者通过所有的 catch 块。

### 深入try..catch

​    1、catch后面的小括号中的类型可以是具体的异常类型，也可以是该异常类型的父类型。
​    2、catch可以写多个。建议catch的时候，精确的一个一个处理。这样有利于程序的调试。
​    3、catch写多个的时候，**从上到下，必须遵守从小到大。**

## throws/throw 关键字：

如果一个方法没有捕获到一个检查性异常，那么该方法必须使用 throws 关键字来声明。throws 关键字放在方法签名的尾部。也可以使用 throw 关键字抛出一个异常，无论它是新实例化的还是刚捕获到的。

### throw

  throw是语句抛出一个异常。

  语法：throw（异常对象）；

```java
public class demo {
 	public static void main(String[] args) {
	    if(true) { 
	      throw new NumberFormatException(); 
	    } else { 
	      System.out.println("sss"); 
	    } 
	}
}
```



### 实例

```java
/*
以下代码报错的原因是什么？
    因为doSome()方法声明位置上使用了：throws ClassNotFoundException
    而ClassNotFoundException是编译时异常。必须编写代码时处理，没有处理
    编译器报错。
 */
public class ExceptionTest04 {
    public static void main(String[] args) {
        // main方法中调用doSome()方法
        // 因为doSome()方法声明位置上有：throws ClassNotFoundException
        // 我们在调用doSome()方法的时候必须对这种异常进行预先的处理。
        // 如果不处理，编译器就报错。
        //编译器报错信息： Unhandled exception: java.lang.ClassNotFoundException
        doSome();
    }

    /**
     * doSome方法在方法声明的位置上使用了：throws ClassNotFoundException
     * 这个代码表示doSome()方法在执行过程中，有可能会出现ClassNotFoundException异常。
     * 叫做类没找到异常。这个异常直接父类是：Exception，所以ClassNotFoundException属于编译时异常。
     * @throws ClassNotFoundException
     */
    public static void doSome() throws ClassNotFoundException{
        System.out.println("doSome!!!!");
    }

}

```

### 解决方法

```java
public class ExceptionTest05 {
    // 第一种处理方式：在方法声明的位置上继续使用：throws，来完成异常的继续上抛。抛给调用者。
    // 上抛类似于推卸责任。（继续把异常传递给调用者。）
    /*
    public static void main(String[] args) throws ClassNotFoundException {
        doSome();
    }
     */

    // 第二种处理方式：try..catch进行捕捉。
    // 捕捉等于把异常拦下了，异常真正的解决了。（调用者是不知道的。）
    public static void main(String[] args) {
        try {
            doSome();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    public static void doSome() throws ClassNotFoundException{
        System.out.println("doSome!!!!");
    }

}
```

### 方法覆盖

重写之后的方法不能比重写之前的方法抛出更多（更宽泛）的异常，可以更少。

```java
class Animal {
    public void doSome() {
    }
    public void doOther() throws Exception {
    }
}

class Cat extends Animal {
    // 编译正常。
    @Override
    public void doSome() throws RuntimeException {
    }

    // 编译报错。
    public void doSome() throws Exception{
    }
    // 编译正常。
    /*public void doOther() {

    }*/

    // 编译正常。
    /*public void doOther() throws Exception{

    }*/

    // 编译正常。
    public void doOther() throws NullPointerException {
    }
}
```



## finally关键字

finally 关键字用来创建在 try 代码块后面执行的代码块。

无论是否发生异常，finally 代码块中的代码总会被执行。

在 finally 代码块中，可以运行清理类型等收尾善后性质的语句。

finally 代码块出现在 catch 代码块最后

```java
try{
  // 程序代码
}catch(异常类型1 异常的变量名1){
  // 程序代码
}catch(异常类型2 异常的变量名2){
  // 程序代码
}finally{
  // 程序代码
}
```

### 特殊情况

```java

/*
finally
 */
public class ExceptionTest12 {
    public static void main(String[] args) {
        try {
            System.out.println("try...");
            // 退出JVM
            System.exit(0); // 退出JVM之后，finally语句中的代码就不执行了！
        } finally {
            System.out.println("finally...");
        }
    }
}
```

### 注意下面事项：

- catch 不能独立于 try 存在。
- 在 try/catch 后面添加 finally 块并非强制性要求的。
- try 代码后不能既没 catch 块也没 finally 块。
- try, catch, finally 块之间不能添加任何代码。



## 自定义异常

1、SUN提供的JDK内置的异常肯定是不够的用的。在实际的开发中，有很多业务，这些业务出现异常之后，JDK中都是没有的。和业务挂钩的。

2、Java中怎么自定义异常呢？
    两步：
        第一步：编写一个类继承Exception或者RuntimeException.
        第二步：提供两个构造方法，一个无参数的，一个带有String参数的。

```java
public class MyException extends Exception{ // 编译时异常
    public MyException(){

}
public MyException(String s){
    super(s);
}

}

/*
public class MyException extends RuntimeException{ // 运行时异常

}
 */
```

```java
public class CheckScore { 
  // 检查分数合法性的方法check() 如果定义的是运行时异常就不用抛异常了 
  public void check(int score) throws MyException {// 抛出自己的异常类 
    if (score > 120 || score < 0) { 
      // 分数不合法时抛出异常 
      throw new MyException("分数不合法，分数应该是0--120之间");// new一个自己的异常类 
    } else { 
      System.out.println("分数合法，你的分数是" + score); 
    } 
  } 
} 

```





# 集合

## 定义

​	 1、数组其实就是一个集合。集合实际上就是一个容器。可以来容纳其它类型的数据。

​	 2、集合不能直接存储基本数据类型，另外集合也不能直接存储java对象，集合当中存储的都是java对象的内存地址。（或者说集合中存储的是引用。）
​	3、在java中每一个不同的集合，底层会对应不同的数据结构。往不同的集合中存储元素，等于将数据放到了不同的数据结构当中。不同的数据结构，数据存储方式不同。

​	new ArrayList(); 创建一个集合，底层是数组。
​	new LinkedList(); 创建一个集合对象，底层是链表。
​	new TreeSet(); 创建一个集合对象，底层是二叉树。

### 概述

Java 集合主要有 3 种重要的类型：
 List：是一个有序集合，可以放重复的数据
 Set：是一个无序集合，不允许放重复的数据
 Map：是一个无序集合，集合中包含一个键对象，一个值对象，键对象不允许重复，值对象可以重复（身份证号—姓名）

![image-20210321091016043](高级知识点.assets/image-20210321091016043.png)

### 数组和collection和区别

- 1、数组长度固定，集合长度可变。   数组是静态的，一个数组实例具有固定的大小，一旦创建了就无法改变容量了，而且生命周期也是不能改变的，还有数组也会做边界检查，如果发现有越界现象，会报RuntimeException异常错误，当然检查边界会以效率为代价。而集合的长度是可变的，可以动态扩展容量，可以根据需要动态改变大小。 
- 2、数组中只能是同一类型的元素且可以存储基本数据类型和对象。集合不能存放基本数据类型，只能存对象，类型可以不一致。 
- 3、集合以类的形式存在，具有封装、继承、多态等类的特性，通过简单的方法和属性即可实现各种复杂操作，大大提高了软件的开发效率

### 数组转换为集合：Arrays.asList(数组)

#### 易错点

1、不能将基本类型数组作为asList的参数
2、不能将数组作为asList参数后，修改数组或List
3、数组转换为集合后，不能进行增删元素



### 集合转换为数组：集合.toArray()

toArray()方法会返回List中所有元素构成的数组，并且数组类型是Object[]。还要注意一点就是，返回的数组是新生成的一个数组，也就是说，多次运行toArray()方法会获得不同的数组对象，但是这些数组对象中内容一样的。也就是说，toArray()返回的数组是安全的，你可以对它进行任意的修改，其原因就是List不会维持一个对该返回的数组的引用。

```java
public static void main(String[] args) {
    List<Integer> list = new ArrayList<>();
    list.add(1);
    list.add(2);
    Object[] objects1 = list.toArray();
    Object[] objects2 = list.toArray();
    System.out.println("objects1 == objects2 : "+(objects1 == objects2));
    objects1[1]=4;
    System.out.println("show objects1: "+ Arrays.toString(objects1));
    System.out.println("show objects2: "+ Arrays.toString(objects2));
    System.out.println("show list: "+list);
}
```



### 	注意

​		集合在java中本身是一个容器，是一个对象。
​		集合中任何时候存储的**都是“引用”**。



## collection

### Collection中能存放什么元素？

​    **没有**使用**“泛型”之前**，Collection中可以**存储Object的所有子类型**。
​    使用了“泛型”之后，Collection中只能存储某个**具体的类型**。

### Collection中的常用方法

- public boolean add(E e)：把给定的对象添加到当前集合中 。
- public void clear()：清空集合中所有的元素。
- public boolean remove(E e)：把给定的对象在当前集合中删除。
- public boolean contains(E e)：判断当前集合中是否包含给定的对象。
- public boolean isEmpty()：判断当前集合是否为空。
- public int size()：返回集合中元素的个数。
- public Object[] toArray()：把集合中的元素，存储到数组中。【作为了解，使用不多。】

## 集合遍历/迭代

```java
public class Dog {
    private String name;
    private int age;
    public Dog(String name, int age) {
        super();
        this.name = name;
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    //重写toString方法
    @Override
    public String toString() {
        return "Dog [name=" + name + ", age=" + age + "]";
    }
}
```

### collection通用方法（**List泛型集合迭代**）

```java
	List<Dog> list=new ArrayList<Dog>();// 创建集合对象
    list.add(new Dog("小胡",8));
    list.add(new Dog("小花",2));
    list.add(new Dog("小花",2));
	// 第一步：获取集合对象的迭代器对象Iterator
    Iterator<Dog> iterList=list.iterator();
 	/* 第二步：通过以上获取的迭代器对象开始迭代/遍历集合。
       以下两个方法是迭代器对象Iterator中的方法：
                boolean hasNext()如果仍有元素可以迭代，则返回 true。
                Object next() 返回迭代的下一个元素。
     */
    while(iterList.hasNext()){
        Dog doglist=iterList.next();
        System.out.println("名字"+doglist.getName()+"年龄"+doglist.getAge());
    }
```

>  在Map集合中**不能用**。在所有的Collection以及子类中使用，是所有Collection通用的一种方式。

### collection通用方法（**List的增强型for循环遍历**）

```java 
	for(Dog dog:list){
		System.out.println("名字"+dog.getName()+"年龄"+dog.getAge());	
	}
```

**或**

```java 
	for(int i=0;i<list.size();i++){
		System.out.println("名字"+list.get(i).getName()+"年龄"+list.get(i).getAge());	
	}
```

### **map的泛型迭代**Map.Entry<Integer,Dog>

```java
		Map<Integer,Dog> map=new HashMap<Integer,Dog>();
		map.put(1, new Dog("小胡",8));
		map.put(2, new Dog("小花",2));
		map.put(2, new Dog("小小",8));
		map.put(2, new Dog("小花",2));
```

#### 迭代器：方式一(迭代器里的泛型也可以省略)


```java
	Iterator<Map.Entry<Integer,Dog> > itr=map.entrySet().iterator();
	while(itr.hasNext()){
		Map.Entry<Integer,Dog> entry=itr.next();
		System.out.println("键"+  entry.getKey()+"<---->值"+ entry.getValue());
	}
```

> **或**

```java
for (Map.Entry<Integer, Dog> entry : map.entrySet()) {
    System.out.println("键" + entry.getKey() + "<---->值" + entry.getValue());
}
```

#### map的泛型迭代->通过获取key的Set集合来获取值

```java
	Iterator<Integer> itr3=map.keySet().iterator();
	while(itr3.hasNext()){
		Integer key=itr3.next();
		System.out.println("键"+ key +"<---->值"+ map.get(key));
	}
```

> **或**

```Java
for (Integer key : map.keySet()) {
    System.out.println("键" + key + "<---->值" + map.get(key));
}
```

#### 通过获取值存入集合中

```java
	Iterator<Dog> itr4=map.values().iterator();
	while(itr4.hasNext()){
		Dog values=itr4.next();
		System.out.println("值"+values);
	}
```

> **或**

```Java
for (Dog values : map.values()) {
    System.out.println("值" + values);
}
```



## List

1. 元素存取有序
2. 带有索引的集合：与数组的索引是一个道理
3. 元素重复：通过元素的equals方法，来比较是否为重复的元素。

### List接口中的常用方法

​	List是Collection接口的子接口。所以List接口中有一些特有的方法。

- **public void add(int index, E element)**: 将指定的元素，添加到该集合中的指定位置上。
- **public E get(int index)**:返回集合中指定位置的元素。
- **public E remove(int index)**:移除列表中指定位置的元素, 返回的是被移除的元素。
- **public E set(int index, E element)**:用指定元素替换集合中指定位置的元素,返回值的更新前的元素。

#### 注意

迭代器迭代元素的过程中不能使用集合对象的remove方法删除元素，要使用迭代器Iterator的remove方法来删除元素，防止出现异常：ConcurrentModificationException

### ArrayList

java.util.ArrayList集合数据存储的结构是**数组结构—元素增删慢，查找快**。日常开发中使用最多的功能为查询数据、遍历数据，所以ArrayList是最常用的集合。**线程不安全，效率高**

> - 容量不固定，随着容量的增加而动态扩容（阈值基本不会达到）
> - 有序集合（插入的顺序==输出的顺序）
> - 插入的元素可以为null
> - 增删改查效率更高（相对于LinkedList来说）
> - 线程不安全
>
> ArrayList集合初始化容量10。扩容为原容量1.5倍。底层是数组。

- 1，ArrayList实现`List`，得到了List集合框架基础功能；
- 2，ArrayList实现`RandomAccess`，获得了快速随机访问存储元素的功能，`RandomAccess`是一个标记接口，没有任何方法；
- 3，ArrayList实现`Cloneable`，得到了`clone()`方法，可以实现克隆功能；
- 4，ArrayList实现`Serializable`，表示可以被序列化，通过序列化去传输，典型的应用就是`hessian协议`。

### 属性

```java
//序列化id
private static final long serialVersionUID = 8683452581122892189L;
//容器默认初始化大小
private static final int DEFAULT_CAPACITY = 10;
//一个空对象
private static final Object[] EMPTY_ELEMENTDATA = {};
//一个空对象，如果使用默认构造函数创建ArrayList，则默认对象内容是该值
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
//ArrayList存放对象的容器，后面的添加、删除等操作都是基于该属性来进行操作
transient Object[] elementData;
//当前列表已使用的长度
private int size;
//数组最大长度（2147483639），这里为什么是Integer.MAX_VALUE - 8是因为有些虚拟机在数组中保留了一些头部信息，防止内存溢出
private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
//这个是从AbstractList继承过来的，代表ArrayList集合修改的次数
protected transient int modCount = 0;
```

### 方法

#### 添加元素

```java
//官方解释：将指定的元素追加到列表（elementData）的末尾
add(E e)
//按照元素的位置，指定新元素位置插入
add(int index, E element)

addAll(Collection<? extends E> c)

addAll(int index, Collection<? extends E> c)

set(int index, E element)
```

#### 删除元素

```java
remove(int index) //移除指定位置上的元素

remove(Object o)//移除指定元素

removeAll(Collection<?> c)

clear()//清空ArrayList内的所有元素，不减小数组容量
```

#### 查找元素

ArrayList提供了get(int index)用读取ArrayList中的元素。由于ArrayList是动态数组，所以我们完全可以根据下标来获取ArrayList中的元素，而且速度还比较快。

```java
    public E get(int index) {
        //判断删除位置是否正确，如果大于列表长度会抛出异常
        rangeCheck(index);
        //直接返回列表中下标等于index的元素
        return elementData(index);
    }
```

#### 判断元素是否存在列表中

```java
public boolean contains(Object o) {
        //调用indexOf方法判断需要查找的元素在列表中的下标是否大于等于0，小于0则不存在
        return indexOf(o) >= 0;
    }
```

> **注意：contains方法会遍历ArrayList。**



#### 截取ArrayList部分内容

```java
    public List<E> subList(int fromIndex, int toIndex) {
        //检查需要截取的下标位置是否正确
        subListRangeCheck(fromIndex, toIndex, size);
        return new SubList(this, 0, fromIndex, toIndex);
    }
```

> **如果修改了subList返回的内容的话，原来的内容也会被修改。**

#### 其他方法

```java
    //判断ArrayList是否为空
    public boolean isEmpty() {
        return size == 0;
    }

    //反向查找元素位置，与上述的indexOf相反
    public int lastIndexOf(Object o) {
        if (o == null) {
            for (int i = size-1; i >= 0; i--)
                if (elementData[i]==null)
                    return i;
        } else {
            for (int i = size-1; i >= 0; i--)
                if (o.equals(elementData[i]))
                    return i;
        }
        return -1;
    }

    //将元素全部拷贝到v中
    public Object clone() {
        try {
            ArrayList<?> v = (ArrayList<?>) super.clone();
            v.elementData = Arrays.copyOf(elementData, size);
            v.modCount = 0;
            return v;
        } catch (CloneNotSupportedException e) {
            // this shouldn't happen, since we are Cloneable
            throw new InternalError(e);
        }
    }

    //返回ArrayList拷贝后的Object数组
    public Object[] toArray() {
        return Arrays.copyOf(elementData, size);
    }

    //返回ArrayList的模板数组。所谓模板数组，即可将T设置为任意数据类型
    public <T> T[] toArray(T[] a) {
        //若a的长度小于ArrayList中的元素个数，返回拷贝了ArrayList中全部元素的新数组
        if (a.length < size)
            // Make a new array of a's runtime type, but my contents:
            return (T[]) Arrays.copyOf(elementData, size, a.getClass());
        //若a的长度大于等于ArrayList中的元素个数，则将ArrayList中的元素全部拷贝到a中
        System.arraycopy(elementData, 0, a, 0, size);
        if (a.length > size)
            a[size] = null;
        return a;
    }

    //将ArrayList中的元素写入到输入流中，先写容量，在写元素
    private void writeObject(java.io.ObjectOutputStream s)
        throws java.io.IOException{
        // Write out element count, and any hidden stuff
        int expectedModCount = modCount;
        s.defaultWriteObject();

        // Write out size as capacity for behavioural compatibility with clone()
        s.writeInt(size);

        // Write out all elements in the proper order.
        for (int i=0; i<size; i++) {
            s.writeObject(elementData[i]);
        }

        if (modCount != expectedModCount) {
            throw new ConcurrentModificationException();
        }
    }

    //从输入流中读取数据到elementData中，一样是先读容量，再读数据
    private void readObject(java.io.ObjectInputStream s)
        throws java.io.IOException, ClassNotFoundException {
        elementData = EMPTY_ELEMENTDATA;

        // Read in size, and any hidden stuff
        s.defaultReadObject();

        // Read in capacity
        s.readInt(); // ignored

        if (size > 0) {
            // be like clone(), allocate array based upon size not capacity
            int capacity = calculateCapacity(elementData, size);
            SharedSecrets.getJavaOISAccess().checkArray(s, Object[].class, capacity);
            ensureCapacityInternal(size);

            Object[] a = elementData;
            // Read in all elements in the proper order.
            for (int i=0; i<size; i++) {
                a[i] = s.readObject();
            }
        }
    }
```

### 小结

- ArrayList自己实现了序列化和反序列化，因为它实现了writeObject和readObject方法。
- ArrayList基于数组实现，会自动扩容。
- 添加元素时会自己判断是否需要扩容，最好指定一个大概的大小，防止后面多次扩容带来的内存消耗；删除元素时不会减少容量，删除元素时，将删除掉的位置元素置为null，下次gc就会自动回收这些元素所占的空间。
- ArrayList是线程不安全的。
- 使用iterator遍历可能会引发多线程异常。

> **怎么得到一个线程安全的List：Collections.synchronizedList(list);**

### LinkedList

java.util.LinkedList集合数据存储的结构是双向链表结构-----**元素增删快**，查找慢的集合（从头遍历）。

插入元素只需新建一个node，再把前后指针指向对应的前后元素即可：

![在这里插入图片描述](高级知识点.assets/20200104151542691.PNG)

删除元素只要把删除节点的链剪掉，再把前后节点连起来就搞定：

![在这里插入图片描述](高级知识点.assets/20200104151859701.PNG)

## Set接口

![image-20210321085630384](高级知识点.assets/image-20210321085630384.png)

![image-20210321090547098](高级知识点.assets/image-20210321090547098.png)

### HashSet

HashSet是根据对象的哈希值来确定元素在集合中的存储位置，因此具有良好的存取和查找性能。**保证元素唯一性的方式依赖于：hashCode与equals方法。**

哈希表存储采用**数组+链表+红黑树**实现，当链表长度超过阈值（8）时，将链表**转换为红黑树**，这样大大减少了查找时间。

![haxibiao](高级知识点.assets/20200104140817561.png)

![在这里插入图片描述](高级知识点.assets/20200104140927989.png)

**因此保证HashSet集合元素的唯一，其实就是根据对象的hashCode和equals方法来决定的**，如果我们往集合中存放自定义的对象，那么保证其唯一，就必须复写hashCode和equals方法建立属于当前对象的比较方式。HashSet没有提供get()方法，是同HashMap一样，Set内部是无序的，只能通过迭代的方式获得。

#### 小结

> **1、**向Map集合中存，以及从Map集合中取，都是先调用key的hashCode方法，然后再调用equals方法！
> equals方法有可能调用，也有可能不调用。
>  拿put(k,v)举例，什么时候equals不会调用？
>      k.hashCode()方法返回哈希值，哈希值经过哈希算法转换成数组下标。数组下标位置上如果是null，equals不需要执行。
>  拿get(k)举例，什么时候equals不会调用？
>      k.hashCode()方法返回哈希值，哈希值经过哈希算法转换成数组下标。数组下标位置上如果是null，equals不需要执行。
>
> **2、**注意：如果一个类的equals方法重写了，那么hashCode()方法必须重写。并且equals方法返回如果是true，hashCode()方法返回的值必须一样。
>  equals方法返回true表示两个对象相同，在同一个单向链表上比较。
>  那么对于同一个单向链表上的节点来说，他们的哈希值都是相同的。所以hashCode()方法的返回值也应该相同。
>
> **3、**hashCode()方法和equals()方法不用研究了，直接使用IDEA工具生成，但是这两个方法需要同时生成。
>
> **4、**终极结论：
>  放在HashMap集合key部分的，以及放在HashSet集合中的元素，需要同时重写hashCode方法和equals方法。
>
> **5、**对于哈希表数据结构来说：
>  如果o1和o2的hash值相同，一定是放到同一个单向链表上。
>  当然如果o1和o2的hash值不同，但由于哈希算法执行结束之后转换的数组下标可能相同，此时会发生“哈希碰撞”。

```java
import java.util.HashSet;
import java.util.Set;

public class HashMapTest02 {
    public static void main(String[] args) {

        Student s1 = new Student("zhangsan");
        Student s2 = new Student("zhangsan");

        // 重写equals方法之前是false
        //System.out.println(s1.equals(s2)); // false

        // 重写equals方法之后是true
        System.out.println(s1.equals(s2)); //true （s1和s2表示相等）

        System.out.println("s1的hashCode=" + s1.hashCode()); //284720968 (重写hashCode之后-1432604525)
        System.out.println("s2的hashCode=" + s2.hashCode()); //122883338 (重写hashCode之后-1432604525)

        // s1.equals(s2)结果已经是true了，表示s1和s2是一样的，相同的，那么往HashSet集合中放的话，
        // 按说只能放进去1个。（HashSet集合特点：无序不可重复）
        Set<Student> students = new HashSet<>();
        students.add(s1);
        students.add(s2);
        System.out.println(students.size()); // 这个结果按说应该是1. 但是结果是2.显然不符合HashSet集合存储特点。怎么办？
    }
}


class Student {
    private String name;

    public Student() {
    }

    public Student(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
    // hashCode
    // equals（如果学生名字一样，表示同一个学生。）
    /*public boolean equals(Object obj){
        if(obj == null || !(obj instanceof Student)) return false;
        if(obj == this) return true;
        Student s = (Student)obj;
        return this.name.equals(s.name);
    }*/
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student student = (Student) o;
        return Objects.equals(name, student.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name);
    }
}
```





###  TreeSet

TreeSet**底层是红黑树**，TreeSet 真正的比较是依赖于元素的 **compareTo()方法**，而这个方法是定义在 **Comparable 接口**里面的。
所以，你要**想重写该方法**，就必须**先实现 Comparable 接口**。这个接口表示的就是自然排序。

![在这里插入图片描述](高级知识点.assets/20200104160718250.PNG)



```java
import java.util.TreeSet;
/*
先按照年龄升序，如果年龄一样的再按照姓名升序。
 */
public class TreeSetTest05 {
    public static void main(String[] args) {
        TreeSet<Vip> vips = new TreeSet<>();
        vips.add(new Vip("zhangsi", 20));
        vips.add(new Vip("zhangsan", 20));
        vips.add(new Vip("king", 18));
        vips.add(new Vip("soft", 17));
        for(Vip vip : vips){
            System.out.println(vip);
        }
    }
class Vip implements Comparable<Vip>{
    String name;
    int age;
    public Vip(String name, int age) {
        this.name = name;
        this.age = age;
    }
    @Override
    public String toString() {
        return "Vip{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
    /*
    compareTo方法的返回值很重要：
        返回0表示相同，value会覆盖。
        返回>0，会继续在右子树上找。【10 - 9 = 1 ，1 > 0的说明左边这个数字比较大。所以在右子树上找。】
        返回<0，会继续在左子树上找。
     */
    @Override
    public int compareTo(Vip v) {
        // 写排序规则，按照什么进行比较。
        if(this.age == v.age){
            // 年龄相同时按照名字排序。
            // 姓名是String类型，可以直接比。调用compareTo来完成比较。
            return this.name.compareTo(v.name);
        } else {
            // 年龄不一样
            return this.age - v.age;
        }
    }
}
```



## Map

### 概述

将键映射到值的对象
一个映射不能包含重复的键
每个键最多只能映射到一个值
Map接口和Collection接口的不同
Map是双列的,Collection是单列的
Map的键唯一,Collection的子体系Set是唯一的
Map集合的数据结构针对键有效，跟值无关;Collection集合的数据结构是针对元素有效

### 常用方法

Map中的常用方法：

- `void clear()`:删除该Map对象中所有键值对；
- `boolean containsKey(Object key)`:查询Map中是否包含指定的key值；
- `boolean containsValue(Object value)`:查询Map中是否包含一个或多个value;
- `Set entrySet()`:返回map中包含的键值对所组成的Set集合，每个集合都是Map.Entry对象。
- `Object get()`：返回指定key对应的value，如果不包含key则返回null；
- `boolean isEmpty()`:查询该Map是否为空；
- `Set keySet()`:返回Map中所有key组成的集合；
- `Collection values()`:返回该Map里所有value组成的Collection。
- `Object put(Object key,Object value)`:添加一个键值对，如果集合中的key重复，则覆盖原来的键值对
- `void putAll(Map m)`:将Map中的键值对复制到本Map中；
- `Object remove(Object key)`:删除指定的key对应的键值对，并返回被删除键值对的value，如果不存在，则返回null；
- `boolean remove(Object key,Object value)`:删除指定键值对，删除成功返回true；
- `int size()`:返回该Map里的键值对个数；

**内部类Entry**

Map中包括一个内部类Entry,该类封装一个键值对，常用方法：

- `Object getKey()`:返回该Entry里包含的key值；
- `Object getvalue()`:返回该Entry里包含的value值；
- `Object setValue(V value)`:设置该Entry里包含的value值，并设置新的value值。

### Map集合的功能概述

#### a:添加功能

V put(K key,V value):添加元素。这个其实还有另一个功能?替换
如果键是第一次存储，就直接存储元素，返回null
如果键不是第一次存在，就用值把以前的值替换掉，返回以前的值

#### b:删除功能

void clear():移除所有的键值对元素
V remove(Object key)：根据键删除键值对元素，并把值返回

#### c:判断功能

boolean containsKey(Object key)：判断集合是否包含指定的键
boolean containsValue(Object value):判断集合是否包含指定的值
boolean isEmpty()：判断集合是否为空

#### d:获取功能

Set<Map.Entry<K,V>> entrySet(): 返回一个键值对的Set集合
V get(Object key):根据键获取值
Set keySet():获取集合中所有键的集合
Collection values():获取集合中所有值的集合

#### e:长度功能

int size()：返回集合中的键值对的对数

### Map集合的遍历之键找值

获取所有键的集合
遍历键的集合，获取到每一个键
根据键找值

### LinkedHashMap的概述和使用

LinkedHashMap的概述: Map 接口的哈希表和链接列表实现，具有可预知的迭代顺序LinkedHashMap的特点： 底层的数据结构是链表和哈希表 元素有序 并且唯一元素的有序性由链表数据结构保证 唯一性由 哈希表数据结构保证Map集合的数据结构只和键有关





### TreeMap集合

TreeMap 键**不允许插入null**
TreeMap: 键的**数据结构是红黑树**,可保证键的排序和唯一性
排序分为自然排序和比较器排序
**线程是不安全的效率比较高**
TreeMap集合排序：
**实现Comparable接口，重写CompareTo方法**
使用比较器

#### TreeMap集合的遍历

```java
public class Test4 {
    public static void main(String[] args) {
        TreeMap<Phone,String> map = new TreeMap<>();
        map.put(new Phone("Apple",7000),"美国");
        map.put(new Phone("Sony",5000),"日本");
        map.put(new Phone("Huawei",6000),"中国");
        //众多的一种遍历方法
        Set<Phone> phones = map.keySet();
        Iterator<Phone> iterator = phones.iterator();
        while(iterator.hasNext()){
            Phone next = iterator.next();
            System.out.println(next.getBrand()+"==="+next.getPrice()+"==="+map.get(next));
        }
    }
}
class Phone implements Comparable<Phone>{
    private String Brand;
    private int Price;

    public Phone(String brand, int price) {
        Brand = brand;
        Price = price;
    }

    public String getBrand() {
        return Brand;
    }

    public void setBrand(String brand) {
        Brand = brand;
    }

    public int getPrice() {
        return Price;
    }

    public void setPrice(int price) {
        Price = price;
    }

    @Override
    public int compareTo(Phone o) {
        return this.getPrice() - o.getPrice();
    }
}
```



### Properties读写属性文件

Properties类是Hashtable类的子类，该对象在处理属性文件时特别方便。Properties类可以把Map对象和属性文件关联起来，从而把Map对象的键值对写入属性文件中，也可以把属性文件中的“属性名=属性值”加载到Map对象中。

> Properties相当于一个key、value都是String类型的Map

- `String getProperty(String key)`:获取Properties中指定的属性名对应的属性值。
- `String getProperty(String key，String defaultValue)`：和前一个方法相同，只不过如果key不存在，则该方法指定默认值。
- `Object setProperty（String key,String value）`:设置属性值，类似于Hashtable的put方法；
- `void load(InputStream inStream)`:从属性文件中加载键值对，把加载出来的键值对追加到Properties里。
- `void store(OutputStream out,String comments)`:将Properties中的键值对输出到指定的属性文件中。

实例：

```java
    public static void main(String[] args) throws Exception {
        Properties props = new Properties();
        //向Properties中添加属性
        props.setProperty("username","yeeku");
        props.setProperty("password","123456");
        //保存到文件中
        props.store(new FileOutputStream("a.ini"),"comment line");
        //新建一个Properties对象
        Properties props2 = new Properties();
        props2.setProperty("gender","male");
        //将文件中的键值对追加到对象中
        props2.load(new FileInputStream("a.ini"));
        System.out.println(props2);//{password=123456, gender=male, username=yeeku}
    }
```



# hashCode和hash算法

## 概述

> hashCode的设计初衷是提高哈希容器的性能

抛开hashCode，现在让你对比两个对象是否相等，你会怎么做？

> thisObj == thatObj
> thisObj.equals(thatObj)

我想不出第三种了，而且这两种其实没啥大的区别，object的equals()方法底层也是==，jdk1.8 Object类的第148行；

```cpp
    public boolean equals(Object obj) {
        return (this == obj);
    }
```

**为什么有了equals还要有hashCode**，hashCode的设计初衷是提高哈希容器的性能，equals的效率是没有hashCode高的。

### hashCode和equals的关系

> 如果两个对象的hashCode()相等，那么他们的equals()不一定相等。
> 如果两个对象的equals()相等，那么他们的hashCode()必定相等。

**★★★★★★★**重写equals()方法时候**一定要重写**hashCode()方法，防止hash碰撞

## hash算法

HashMap的hash算法来看

```cpp
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

hashMap我们知道默认初始容量是16，也就是有16个桶，那hashmap是通过什么来计算出put对象的时候该放到哪个桶呢？下面是**hashmap的getNode方法**

```Java
final Node<K,V> getNode(int hash, Object key) {
    Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (first = tab[(n - 1) & hash]) != null) {
        if (first.hash == hash && // always check first node
            ((k = first.key) == key || (key != null && key.equals(k))))
            return first;
        if ((e = first.next) != null) {
            if (first instanceof TreeNode)
                return ((TreeNode<K,V>)first).getTreeNode(hash, key);
            do {
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    return e;
            } while ((e = e.next) != null);
        }
    }
    return null;
}
```

### 小结论

数组长度-1、^运算、>>>16，这三个操作都是为了让key在hashmap的桶中尽可能分散用&而不用%是为了提高计算性能。

#### 1、为什么数组长度要 - 1，直接数组长度&key.hashCode不行吗?

**我们先不考虑数组下标越界的问题**，hashMap默认长度是16，看看16的二进制码是多少

```cpp
log.info("16的二进制码：{}",Integer.toBinaryString(16));  
//16的二进制码：10000，
12
```

再看看key.hashCode()的二进制码是多少，以郭德纲为例

```cpp
log.info("key的二进制码：{}",Integer.toBinaryString("郭德纲".hashCode()));
//key的二进制码：10001011000001111110001000
12
length & key.hashCode()  => 10000 & 10001011000001111110001000
位数不够，高位补0，即

0000 0000 0000 0000 0000 0001 0000 
                & 
0010 0010 1100 0001 1111 1000 1000

&运算规则是第一个操作数的的第n位于第二个操作数的第n位都为1才为1，否则为0
所以结果为0000 0000 0000 0000 0000 0000 0000，即 0
123456789
```

问题就出在16的二进制码上，它码是10000，只有遇到hash值二进制码倒数第五位为1的key他们&运算的结果才不等于0，再来看16-1的二进制码，它码是1111，同样用郭德纲这个key来举例

```cpp
(length-1) & key.hashCode()  => 1111 & 10001011000001111110001000
位数不够，高位补0，即

0000 0000 0000 0000 0000 0000 1111 
                & 
0010 0010 1100 0001 1111 1000 1000

&运算规则是第一个操作数的的第n位于第二个操作数的第n位都为1才为1，否则为0
所以结果为0000 0000 0000 0000 0000 0000 1000，即 8
123456789
```

如果还看不出这其中的玄机，你就多搞几个key来试试，总之记住，限制它们&运算的结果就会有很多种可能性了，不再受到hash值二进制码倒数第五位为1才能为1的限制

#### 2、为什么要length-1&key.hashCode计算下标，而不是用key.hashCode%length?

1、其实(length-1)&key.hashCode计算出来的值和key.hashCode%length是一样的

```cpp
log.info("(length-1)&key.hashCode：{}",15&"郭德纲".hashCode());
log.info("key.hashCode%length：{}","郭德纲".hashCode()%16);

//  (length-1)&key.hashCode：8
//  key.hashCode%length：8
12345
```

那你可能更蒙逼了，都一样的为啥不用%，这就要说到第二个知识点

2、只有当length为2的n次方时，(length-1)&key.hashCode才等于key.hashCode%length，比如当length为15时

```cpp
log.info("(length-1)&key的hash值：{}",14&"郭德纲".hashCode());
log.info("key的hash值%length：{}","郭德纲".hashCode()%15);

//  (length-1)&key.hashCode：8
//  key.hashCode%length：3
12345
```

可能又有小朋友会思考，我不管那我就想用%运算，要用魔法打败魔法，请看第三点

3、用&而不用%是为了提高计算性能，对于处理器来讲，&运算的效率是高于%运算的，就这么简单，除此之外，除法的效率也没&高

#### 3、为什么要进行^运算，|运算、&运算不行吗?

这是异或运算符，第一个操作数的的第n位于第二个操作数的第n位相反才为1，否则为0。我们多算几个key的值出来对比

```cpp
//不进行异或运算返回的数组下标
log.info("郭德纲：{}", Integer.toBinaryString("郭德纲".hashCode()));            
log.info("彭于晏：{}", Integer.toBinaryString("彭于晏".hashCode()));            
log.info("李小龙：{}", Integer.toBinaryString("李小龙".hashCode()));            
log.info("蔡徐鸡：{}", Integer.toBinaryString("蔡徐鸡".hashCode()));            
log.info("唱跳rap篮球鸡叫：{}", Integer.toBinaryString("唱跳rap篮球鸡叫".hashCode()));

00001000101100000111111000 1000
00000101110000001000011010 1110
00000110001111100100010011 1000
00000111111111111100010111 0010
10111010111100100011001111 1110

进行&运算，看下它们返回的数组下标，length为16的话，只看后四位即可
8
14
8
2
14

1234567891011121314151617181920
//进行异或运算返回的数组下标
log.info("郭德纲：{}", Integer.toBinaryString("郭德纲".hashCode()^("郭德纲".hashCode()>>>16)));                  
log.info("彭于晏：{}", Integer.toBinaryString("彭于晏".hashCode()^("彭于晏".hashCode()>>>16)));                  
log.info("李小龙：{}", Integer.toBinaryString("李小龙".hashCode()^("李小龙".hashCode()>>>16)));                  
log.info("蔡徐鸡：{}", Integer.toBinaryString("蔡徐鸡".hashCode()^("蔡徐鸡".hashCode()>>>16)));                  
log.info("唱跳rap篮球鸡叫：{}", Integer.toBinaryString("唱跳rap篮球鸡叫".hashCode()^("唱跳rap篮球鸡叫".hashCode()>>>16)));

0000001000101100000111011010 0100
0000000101110000001000001101 1110
0000000110001111100100001011 0111
0000000111111111111100001000 1101
0010111010111100101000100100 0010

进行&运算，看下它们返回的数组下标，length为16的话，只看后四位即可
4
14
7
13
2

1234567891011121314151617181920
```

很明显，做了^运算的数组下标更分散

如果还不死心，再来看几个例子

看下 ^、|、&这三个位运算的结果就知道了

```clike
log.info("^ 运算：{}", 15 & ("郭德纲".hashCode() ^ ("郭德纲".hashCode() >>> 16)));  
log.info("^ 运算：{}", 15 & ("彭于晏".hashCode() ^ ("彭于晏".hashCode() >>> 16)));  
log.info("^ 运算：{}", 15 & ("李小龙".hashCode() ^ ("李小龙".hashCode() >>> 16)));  
log.info("^ 运算：{}", 15 & ("蔡徐鸡".hashCode() ^ ("蔡徐鸡".hashCode() >>> 16)));  
//^ 运算：4      
//^ 运算：14     
//^ 运算：7      
//^ 运算：13      
                                                                               
log.info("| 运算：{}", 15 & ("郭德纲".hashCode() | ("郭德纲".hashCode() >>> 16)));  
log.info("| 运算：{}", 15 & ("彭于晏".hashCode() | ("彭于晏".hashCode() >>> 16)));  
log.info("| 运算：{}", 15 & ("李小龙".hashCode() | ("李小龙".hashCode() >>> 16)));  
log.info("| 运算：{}", 15 & ("蔡徐鸡".hashCode() | ("蔡徐鸡".hashCode() >>> 16)));  
//| 运算：12     
//| 运算：14     
//| 运算：15     
//| 运算：15  
                                                                                           
log.info("& 运算：{}", 15 & ("郭德纲".hashCode() & ("郭德纲".hashCode() >>> 16)));  
log.info("& 运算：{}", 15 & ("彭于晏".hashCode() & ("彭于晏".hashCode() >>> 16)));  
log.info("& 运算：{}", 15 & ("李小龙".hashCode() & ("李小龙".hashCode() >>> 16)));  
log.info("& 运算：{}", 15 & ("蔡徐鸡".hashCode() & ("蔡徐鸡".hashCode() >>> 16))); 
//& 运算：8      
//& 运算：0      
//& 运算：8      
//& 运算：2   
1234567891011121314151617181920212223242526
```

现在看出来了吧，^ 运算的下标分散，具体原理在下文会说

#### 4、为什么要>>>16，>>>15不行吗？

这是无符号右移16位，位数不够，高位补0

现在来说进行 ^ 运算中的玄学，其实>>>16和 ^ 运算是**相辅相成**的关系，这一套操作是为了保留hash值高16位和低16位的特征，因为数组长度(按默认的16来算)减1后的二进制码低16位永远是1111，我们肯定要尽可能的让1111和hash值**产生联系**，但是很显然，如果只是1111&hash值的话，1111只会与hash值的低四位产生联系，也就是说这种算法出来的值**只保留了hash值低四位的特征**，前面还有28位的特征全部丢失了；

因为&运算是都为1才为1，1111我们肯定是改变不了的，只有从hash值入手，所以hashMap作者采用了 **key.hashCode() ^ (key.hashCode() >>> 16)** 这个巧妙的扰动算法，key的hash值经过无符号右移16位，再与key原来的hash值进行 ^ 运算，就能很好的保留hash值的所有**特征**，这种离散效果才是我们最想要的。

上面这两段话就是理解>>>16和 ^ 运算的精髓所在，如果没看懂，建议你休息一会儿再回来看，总结记住，目的都是为了让数组下标更分散

再补充一点点，其实并不是非得右移16位，如下面得测试，右移8位右移12位都能起到很好的扰动效果，但是hash值的二进制码是32位，所以最理想的肯定是折半咯，雨露均沾

```clike
log.info(">>>16运算：{}", 15 & ("郭德纲".hashCode() ^ ("郭德纲".hashCode() >>> 16)));
log.info(">>>16运算：{}", 15 & ("彭于晏".hashCode() ^ ("彭于晏".hashCode() >>> 16)));
log.info(">>>16运算：{}", 15 & ("李小龙".hashCode() ^ ("李小龙".hashCode() >>> 16)));
log.info(">>>16运算：{}", 15 & ("蔡徐鸡".hashCode() ^ ("蔡徐鸡".hashCode() >>> 16)));
//>>>16运算：4  
//>>>16运算：14 
//>>>16运算：7  
//>>>16运算：13
   
log.info(">>>16运算：{}", 15 & ("郭德纲".hashCode() ^ ("郭德纲".hashCode() >>> 8))); 
log.info(">>>16运算：{}", 15 & ("彭于晏".hashCode() ^ ("彭于晏".hashCode() >>> 8))); 
log.info(">>>16运算：{}", 15 & ("李小龙".hashCode() ^ ("李小龙".hashCode() >>> 8))); 
log.info(">>>16运算：{}", 15 & ("蔡徐鸡".hashCode() ^ ("蔡徐鸡".hashCode() >>> 8))); 
//>>>8运算：7
//>>>8运算：1
//>>>8运算：9
//>>>8运算：3 

log.info(">>>16运算：{}", 15 & ("郭德纲".hashCode() ^ ("郭德纲".hashCode() >>> 12)));
log.info(">>>16运算：{}", 15 & ("彭于晏".hashCode() ^ ("彭于晏".hashCode() >>> 12)));
log.info(">>>16运算：{}", 15 & ("李小龙".hashCode() ^ ("李小龙".hashCode() >>> 12)));
log.info(">>>16运算：{}", 15 & ("蔡徐鸡".hashCode() ^ ("蔡徐鸡".hashCode() >>> 12)));
//>>>12运算：9 
//>>>12运算：12
//>>>12运算：1 
//>>>12运算：13
1234567891011121314151617181920212223242526
```









# 泛型

## 概念

![java-generics](高级知识点.assets/l)

**Java泛型设计原则：只要在编译时期没有出现警告，那么运行时期就不会出现ClassCastException异常**.

泛型：**把类型明确的工作推迟到创建对象或调用方法的时候才去明确的特殊的类型**

参数化类型:

- **把类型当作是参数一样传递**
- **`<数据类型>` 只能是引用类型**

相关术语：

- `ArrayList<E>`中的**E称为类型参数变量**
- `ArrayList<Integer>`中的**Integer称为实际类型参数**
- **整个称为`ArrayList<E>`泛型类型**
- **整个`ArrayList<Integer>`称为参数化的类型ParameterizedType**

> JDK8新特性：**钻石表达式**
> 	List<String> list = new ArrayList<>();
> 	类型自动推断！

## 泛型的作用

- 代码更加简洁【不用强制转换】
- 程序更加健壮【只要编译时期没有警告，那么运行时期就不会出现ClassCastException异常】
- 可读性和稳定性【在编写集合的时候，就限定了类型】

> **早期Java是使用Object来代表任意类型的，但是向下转型有强转的问题，这样程序就不太安全**

## 泛型后的遍历

在创建集合的时候，**我们明确了集合的类型了**，所以我们可以使用增强for来遍历集合！

```java
    //创建集合对象
    ArrayList<String> list = new ArrayList<>();

    list.add("hello");
    list.add("world");
    list.add("java");

    //遍历,由于明确了类型.我们可以增强for
    for (String s : list) {
        System.out.println(s);
    }
```

## 泛型类

```java
/*
    1:把泛型定义在类上
    2:类型变量定义在类上,方法中也可以使用
 */
public class ObjectTool<T> {
    private T obj;

    public T getObj() {
        return obj;
    }

    public void setObj(T obj) {
        this.obj = obj;
    }
}
```

> **用户想要使用哪种类型，就在创建的时候指定类型。使用的时候，该类就会自动转换成用户想要使用的类型了。**

```java
    public static void main(String[] args) {
        //创建对象并指定元素类型
        ObjectTool<String> tool = new ObjectTool<>();

        tool.setObj(new String("罗海江"));
        String s = tool.getObj();
        System.out.println(s);


        //创建对象并指定元素类型
        ObjectTool<Integer> objectTool = new ObjectTool<>();
        /**
         * 如果我在这个对象里传入的是String类型的,它在编译时期就通过不了了.
         */
        objectTool.setObj(10);
        int i = objectTool.getObj();
        System.out.println(i);
    }
```

## 泛型方法

- 定义泛型方法....**泛型是先定义后使用的**

```java
    //定义泛型方法..
    public <T> void show(T t) {
        System.out.println(t);
    }


    public static void main(String[] args) {
        //创建对象
        ObjectTool tool = new ObjectTool();

        //调用方法,传入的参数是什么类型,返回值就是什么类型
        tool.show("hello");
        tool.show(12);
        tool.show(12.5);

    }
```

## 泛型类派生出的子类

**泛型类是拥有泛型这个特性的类，它本质上还是一个Java类，那么它就可以被继承**

那它是怎么被继承的呢？？这里分两种情况

### 子类**明确**泛型类的类型参数变量

```java
/*
    把泛型定义在接口上
 */
interface Inter<T> {
    public abstract void show(T t);

}


/**
 * 子类明确泛型类的类型参数变量:
 */

class InterImpl implements Inter<String> {
    @Override
    public void show(String s) {
        System.out.println(s);
    }
}
```



### 子类**不明确**泛型类的类型参数变量

- 当子类不明确泛型类的类型参数变量时，**外界使用子类的时候，也需要传递类型参数变量进来，在实现类上需要定义出类型参数变量**

```java
/*
    把泛型定义在接口上
 */
interface Inter<T> {
    public abstract void show(T t);
}

/**
 * 子类不明确泛型类的类型参数变量:
 *      实现类也要定义出<T>类型的
 *
 */
class InterImpl<T> implements Inter<T> {
    @Override
    public void show(T t) {
        System.out.println(t);
    }
}
```

### 测试代码

```java
    public static void main(String[] args) {
     	//第一种情况测试
        Inter<String> i = new InterImpl();
        i.show("hello");
        //第二种情况测试
        Inter<String> ii = new InterImp2<>();
        ii.show("100");
    }
```

> - **实现类的要是重写父类的方法，返回值的类型是要和父类一样的！**
> - **类上声明的泛形只对非静态成员有效**

## 类型通配符

题目：接收一个集合参数，遍历集合并把集合元素打印出来，怎么办？

```java
//出现警告，说没有确定集合元素的类型....这样是不优雅的...
    public void test(List list) {
        for (Object o : list) {
            System.out.println(o);
        }
    }
//下面都可以
    public void test1(List<Object> list){
        for (Object o : list) {
            System.out.println(o);
        }
    }

    public void test2(List<?> list){
        for (Object o : list) {
            System.out.println(o);
        }
    }
```





# 序列化

Java 提供了一种**对象序列化**的机制，该机制中，**一个对象**可以被表示为**一个字节序列**，该字节序列包括该对象的数据、有关对象的类型的信息和存储在对象中数据的类型。

将**序列化对象写入文件之后**，可以从**文件中读取出来，并且对它进行反序列化**，也就是说，**对象的类型信息、对象的数据**，还有对象中的数据类型可以用来在内存中新建对象。

整个过程都是 Java 虚拟机（JVM）独立的，也就是说，在一个平台上序列化的对象可以在**另一个完全不同的平台上反序列化该对象**。

## 一、序列化与反序列化

序列化：指堆内存中的java对象数据，通过某种方式把对存储到磁盘文件中，或者传递给其他网络节点（网络传输）。这个过程称为序列化，通常是指将数据结构或对象转化成二进制的过程。

> 即将对象转化为二进制，用于保存，或者网络传输。

反序列化：把磁盘文件中的对象数据或者把网络节点上的对象数据，恢复成Java对象模型的过程。也就是将在序列化过程中所生成的二进制串转换成数据结构或者对象的过程

> 与序列化相反，将二进制转化成对象。

## 二、序列化的作用

① 想把内存中的对象保存到一个文件中或者数据库中时候；
② 想用套接字在网络上传送对象的时候；
③ 想通过RMI传输对象的时候

> 一些应用场景，涉及到将对象转化成二进制，序列化保证了能够成功读取到保存的对象。

## 三、java的序列化实现

要实现对象的序列化，最直接的操作就是实现Serializable接口

使用IO流中的对象流可以实现序列化操作，将对象保存到文件，再读取出来。

**类 ObjectInputStream 和 ObjectOutputStream 是高层次的数据流，它们包含反序列化和序列化对象的方法**

### ObjectOutputStream

ObjectOutputStream是序列化流，可以将Java程序中的对象写到文件中。

**ObjectOutputStream 构造方法：**ObjectOutputStream(OutputStream out)：参数要传递字节输出流。

> -  创建序列化流，用来写。
> -  调用 writeObject 方法，写对象。
> -  释放资源。
> -  **tips:** 要使用序列化流向文件中写的对象，必须实现 Serializable 接口。

ObjectOutputStream 类包含很多写方法来写各种数据类型，但是一个**特别的方法**例外，此方法序列化一个对象，并将它发送到输出流。

```java
public final void writeObject(Object x) throws IOException
```

### ObjectInputStream

ObjectInputStream 是反序列化流， 可以将文件中的对象读取到 Java 程序中。它的返回值为Object，因此，你需要将它转换成合适的数据类型。

**ObjectInputStream 的构造方法：**ObjectInputStream(InputStream in)：参数要传递字节输入流。

**ObjectInputStream 读取对象的方法（特有的方法）：**Object readObject()： 从文件中读取对象，并将该对象返回

```java
public final Object readObject() throws IOException,ClassNotFoundException
```



> -  创建 ObjectInputStream 反序列化流。
> -  调用 readObject 方法，读取对象。
> -  释放资源。
>
> **特殊情况:**
>
> 被 static 修饰的成员变量**无法序列化**，无法写到文件。
>
> 如果不希望某个成员变量写到文件，同时又不希望**使用 static 关键字**， 那么可以**使用 transient**。transient 关键字**表示瞬态**，被 transient 修饰的成员变量无法被序列化。



### 案例

#### 首先创建一个对象，并实现Serializable接口：

```java
import java.io.Serializable;

public class User implements Serializable{
    private static final long serialVersionUID = 1L;

    private String name;

    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User [name=" + name + ", age=" + age + "]";
    }
}
```

#### 用对象流写一个保存对象与读取对象的工具类：

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;

public class SerializeUtil {
    // 保存对象，序列化
    public static void saveObject(Object object) throws Exception {
        ObjectOutputStream out = null;
        FileOutputStream fout = null;
        try {
            fout = new FileOutputStream("D:/1.txt");
            out = new ObjectOutputStream(fout);
            out.writeObject(object);
        } finally {
            fout.close();
            out.close();
        }
    }

    // 读取对象，反序列化
    public static Object readObject() throws Exception {
        ObjectInputStream in = null;
        FileInputStream fin = null;
        try {
            fin = new FileInputStream("D:/1.txt");
            in = new ObjectInputStream(fin);
            Object object = in.readObject();
            return object;
        } finally {
            fin.close();
            in.close();
        }
    }
}
```

#### 测试：

```java
public class Main {
    public static void main(String[] args) {
        User user = new User();
        user.setName("旭旭宝宝");
        user.setAge(33);

        // 保存
        try {
            SerializeUtil.saveObject(user);
        } catch (Exception e) {
            System.out.println("保存时异常：" + e.getMessage());
        }
        // 读取
        User userObject;
        try {
            userObject = (User) SerializeUtil.readObject();
            System.out.println(userObject);
        } catch (Exception e) {
            System.out.println("读取时异常：" + e.getMessage());
        }
    }
}
```

## 四、序列化ID的作用

可以看到，我们在进行序列化时，加了一个serialVersionUID字段，这便是序列化ID

```java
 private static final long serialVersionUID = 1L;
```

这个序列化ID起着关键的作用，它决定着是否能够成功反序列化！java的序列化机制是通过判断运行时类的serialVersionUID来验证版本一致性的，在进行反序列化时，JVM会把传进来的字节流中的serialVersionUID与本地实体类中的serialVersionUID进行比较，如果相同则认为是一致的，便可以进行反序列化，否则就会报序列化版本不一致的异常。

> 即序列化ID是为了保证成功进行反序列化

## 五、默认的序列化ID

当我们一个实体类中没有显式的定义一个名为“serialVersionUID”、类型为long的变量时，Java序列化机制会根据编译时的class自动生成一个serialVersionUID作为序列化版本比较，这种情况下，只有同一次编译生成的class才会生成相同的serialVersionUID。譬如，当我们编写一个类时，随着时间的推移，我们因为需求改动，需要在本地类中添加其他的字段，这个时候再反序列化时便会出现serialVersionUID不一致，导致反序列化失败。那么如何解决呢？便是在本地类中添加一个“serialVersionUID”变量，值保持不变，便可以进行序列化和反序列化。

>  如果没有显示指定serialVersionUID，会自动生成一个。 
>
>  只有同一次编译生成的class才会生成相同的serialVersionUID 
>
>  但是如果出现需求变动，Bean类发生改变，则会导致反序列化失败。为了不出现这类的问题，所以我们最好还是显式的指定一个serialVersionUID。 

## 六、序列化的其他问题

1. 静态变量不会被序列化（ static,transient）
2. 当一个父类实现序列化，子类自动实现序列化，不需要显式实现Serializable接口。
3. 当一个对象的实例变量引用其他对象，序列化该对象时也把引用对象进行序列化。

> ```
> 子类序列化时：
> 如果父类没有实现Serializable接口，没有提供默认构造函数，那么子类的序列化会出错；
> 如果父类没有实现Serializable接口，提供了默认的构造函数，那么子类可以序列化，父类的成员变量不会被序列化。如果父类实现了Serializable接口，则父类和子类都可以序列化。
> 123
> ```

## 七、效率更高的序列化框架—Protostuff

其实java的原生序列化方式（通过实现Serialiable接口），效率并不是最高的。

![这里写图片描述](高级知识点.assets/20180813153008133)





# 多线程

## 概述

### 进程和线程

**一个程序**就是**一个进程**，而一个程序中的**多个任务**则被称为**线程**。进程是表示资源分配的**基本单位**，**线程**是进程中执行运算的**最小单位**，亦是**调度运行**的**基本单位**。

### 并发性（concurrency）和并行性（parallel）

并行是指在同一时刻，有多条指令在多个处理器上同时运行；

并发指的是在同一时刻只能有一条指令执行，即每个指令以时间片为单位来执行

![这里写图片描述](高级知识点.assets/20180719091355307)

## 单线程

简单的说，单线程就是进程中只有一个线程。单线程在程序执行时，所走的程序路径按照连续顺序排下来，前面的必须处理好，后面的才会执行。

```java
public class SingleThread {
    public static void main(String[] args) {
        for (int i = 0; i < 10000; i++) {
            System.out.print(i + " ");
        }
    }
}
```

*上述Java代码中，只有一个主线程执行`main`方法。*





##  多线程

由一个以上线程组成的程序称为多线程程序。常见的多线程程序如：**GUI应用程序、I/O操作、网络容器**等。
Java中，一定是从**主线程**开始执行（**main**方法），然后在主线程的**某个位置启动新的线程**。java中线程之间**共享堆内存和方法区**，**栈内存独立**，每个栈之间互不干扰，**多线程并发**。

### 线程的生命周期

![image-20210324141456731](高级知识点.assets/image-20210324141456731.png)

> - 新建状态:
>
>   使用 **new** 关键字和 **Thread** 类或其子类建立一个线程对象后，该线程对象就处于新建状态。它保持这个状态直到程序 **start()** 这个线程。
>
> - 就绪状态:
>
>   当线程对象调用了start()方法之后，该线程就进入就绪状态。就绪状态的线程处于就绪队列中，要等待JVM里线程调度器的调度。
>
> - 运行状态:
>
>   如果就绪状态的线程获取 CPU 资源，就可以执行 **run()**，此时线程便处于运行状态。处于运行状态的线程最为复杂，它可以变为阻塞状态、就绪状态和死亡状态。
>
> - 阻塞状态:
>
>   如果一个线程执行了sleep（睡眠）、suspend（挂起）等方法，失去所占用资源之后，该线程就从运行状态进入阻塞状态。在睡眠时间已到或获得设备资源后可以重新进入就绪状态。可以分为三种：
>
>   - 等待阻塞：运行状态中的线程执行 wait() 方法，使线程进入到等待阻塞状态。
>   - 同步阻塞：线程在获取 synchronized 同步锁失败(因为同步锁被其他线程占用)。
>   - 其他阻塞：通过调用线程的 sleep() 或 join() 发出了 I/O 请求时，线程就会进入到阻塞状态。当sleep() 状态超时，join() 等待线程终止或超时，或者 I/O 处理完毕，线程重新转入就绪状态。
>
> - 死亡状态:
>
>   一个运行状态的线程完成任务或者其他终止条件发生时，该线程就切换到终止状态。



## 线程的基本操作

### 创建

Java中创建多线程类两种方法：

#### **1、继承java.lang.Thread**

```java
public class ThreadTest02 {
    public static void main(String[] args) {
        // 这里是main方法，这里的代码属于主线程，在主栈中运行。
        // 新建一个分支线程对象
        MyThread t = new MyThread();
        // 启动线程
        //t.run(); // 不会启动线程，不会分配新的分支栈。（这种方式就是单线程。）
        // 这段代码的任务只是为了开启一个新的栈空间，只要新的栈空间开出来，start()方法就结束了。线程就启动成功了。
        // 启动成功的线程会自动调用run方法，并且run方法在分支栈的栈底部（压栈）。
        // run方法在分支栈的栈底部，main方法在主栈的栈底部。run和main是平级的。
        //start()方法的作用是：启动一个分支线程，在JVM中开辟一个新的栈空间，这段代码任务完成之后，瞬间就结束
        t.start();
        // 这里的代码还是运行在主线程中。
        for(int i = 0; i < 1000; i++){
            System.out.println("主线程--->" + i);
        }
    }
}

class MyThread extends Thread {
    @Override
    public void run() {
        // 编写程序，这段程序运行在分支线程中（分支栈）。
        for(int i = 0; i < 1000; i++){
            System.out.println("分支线程--->" + i);
        }
    }
}
```

上述代码中，`MyThread`类继承了类`java.lang.Thread`，并覆写了`run`方法。

主线程从`main`方法开始执行，当主线程执行至`t.start()`时，启动新线程（注意此处是调用`start`方法，不是`run`方法），新线程会**并发执行自身**的`run`方法。

**start()方法的作用**是：启动一个分支线程，在JVM中开辟一个新的栈空间，这段代码任务完成之后，**瞬间就结束**





#### **2、实现java.lang.Runnable接口**

##### 建议方法

```java
public class ThreadTest03 {
    public static void main(String[] args) {
        // 创建一个可运行的对象
        //MyRunnable r = new MyRunnable();
        // 将可运行的对象封装成一个线程对象
        //Thread t = new Thread(r);
        Thread t = new Thread(new MyRunnable()); // 合并代码
        // 启动线程
        t.start();

        for(int i = 0; i < 100; i++){
            System.out.println("主线程--->" + i);
        }
    }
}

// 这并不是一个线程类，是一个可运行的类。它还不是一个线程。
class MyRunnable implements Runnable {

    @Override
    public void run() {
        for(int i = 0; i < 100; i++){
            System.out.println("分支线程--->" + i);
        }
    }
}
```

> 注意：主线程执行完成后，如果还有子线程正在执行，程序也不会结束。只有当所有线程都结束时（不含Daemon Thread），程序才会结束。
>
> 亘古不变：程序从上而下运行，上面不结束，不会到下面；



#### 3、实现Callable接口通过FutureTask包装器来创建Thread线程

Callable接口提供了一个call()方法可以作为线程执行体，这个方法具有返回值，还可以声明抛出异常

```java
import java.util.concurrent.Callable;
import java.util.concurrent.FutureTask;

public class ThirdThread {
    public static void main(String[] args) {
        //创建Callable对象
        ThirdThread rt = new ThirdThread();
        //先使用Lambda表达式创建Callable<Integer>对象
        //使用FutureTask来包装Callable对象
        FutureTask<Integer> task = new FutureTask<>((Callable<Integer>)()->{
            int i=0;
            for(;i<100;i++) {
                System.out.println(Thread.currentThread().getName()+"循环变量i的值："+i);
            }
            return i;
        });
        for(int i=0;i<100;i++) {
            System.out.println(Thread.currentThread().getName()+" 的循环变量i的值："+i);
            
            if(i==20) {
                //实质还是以Callable对象来创建并启动的
                new Thread(task,"有返回值的线程").start();

                /*Thread t = new Thread(task);
                t.start();*/
            }
        }
        try {
            System.out.println("子线程的返回值："+task.get());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

}
```

### Runnable，Callable接口与Thread类的对比

#### 采用实现Runnable，Callable接口优缺点

1，接口可以多继承，继承了Runnable接口还能继承其他接口
2，适合多个相同线程来处理同一份资源的情况，
3，缺点是，编程稍微复杂，访问当前线程必须使用Thread.currentThread()

#### 采用继承Thread类优缺点

1，编写简单，访问当前线程可直接用this
2，缺点是，不能再继承其他类
综上，建议采用**实现Runnable**接口的方法来创建和启动线程

### 当前线程的获取

```java
package com.bjpowernode.java.thread;
/*
1、怎么获取当前线程对象？
    Thread t = Thread.currentThread();
    返回值t就是当前线程。

2、获取线程对象的名字
    String name = 线程对象.getName();

3、修改线程对象的名字
    线程对象.setName("线程名字");

4、当线程没有设置名字的时候，默认的名字有什么规律？（了解一下）
    Thread-0
    Thread-1
    Thread-2
    Thread-3
    .....
 */
public class ThreadTest05 {
    public void doSome(){
        // 这样就不行了
        //this.getName();
        //super.getName();
        // 但是这样可以
        String name = Thread.currentThread().getName();
        System.out.println("------->" + name);
    }

    public static void main(String[] args) {
        ThreadTest05 tt = new ThreadTest05();
        tt.doSome();

        //currentThread就是当前线程对象
        // 这个代码出现在main方法当中，所以当前线程就是主线程。
        Thread currentThread = Thread.currentThread();
        System.out.println(currentThread.getName()); //main

        // 创建线程对象
        MyThread2 t = new MyThread2();
        // 设置线程的名字
        t.setName("t1");
        // 获取线程的名字
        String tName = t.getName();
        System.out.println(tName); //Thread-0

        MyThread2 t2 = new MyThread2();
        t2.setName("t2");
        System.out.println(t2.getName()); //Thread-1\
        t2.start();

        // 启动线程
        t.start();
    }
}

class MyThread2 extends Thread {
    public void run(){
        for(int i = 0; i < 100; i++){
            // currentThread就是当前线程对象。当前线程是谁呢？
            // 当t1线程执行run方法，那么这个当前线程就是t1
            // 当t2线程执行run方法，那么这个当前线程就是t2
            Thread currentThread = Thread.currentThread();
            System.out.println(currentThread.getName() + "-->" + i);

            //System.out.println(super.getName() + "-->" + i);
            //System.out.println(this.getName() + "-->" + i);
        }
    }
}

```



###  暂停Thread.sleep

Java中线程的暂停是调用`java.lang.Thread`类的`sleep`方法（注意是**类方法**）。该方法会使**当前正在执行的线程**暂停指定的时间，如果线程持有锁，`sleep`方法结束前并不会释放该锁。

```java
public class Main {
    public static void main(String[] args) {
        for (int i = 0; i < 10; i++) {
            System.out.print(i + " ");
            try {
                Thread.sleep(1000);    //当前main线程暂停1000ms
            } catch (InterruptedException e) {
            }
        }
    }
}
```

*当main线程调用`Thread.sleep(1000)`后，线程会被暂停，如果被`interrupt`，则会抛出`InterruptedException`异常。*

#### 面试题

```java
public class ThreadTest07 {
    public static void main(String[] args) {
        // 创建线程对象
        Thread t = new MyThread3();
        t.setName("t");
        t.start();

        // 调用sleep方法
        try {
            // 问题：这行代码会让线程t进入休眠状态吗？
            t.sleep(1000 * 5); // 在执行的时候还是会转换成：Thread.sleep(1000 * 5);
                              // 这行代码的作用是：让当前线程进入休眠，也就是说main线程进入休眠。
                              // 这样代码出现在main方法中，main线程睡眠。
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // 5秒之后这里才会执行。
        System.out.println("hello World!");
    }
}

class MyThread3 extends Thread {
    public void run(){
        for(int i = 0; i < 10000; i++){
            System.out.println(Thread.currentThread().getName() + "--->" + i);
        }
    }
}

```

#### 中断睡眠interrupt

```java
/*
sleep睡眠太久了，如果希望半道上醒来，你应该怎么办？也就是说怎么叫醒一个正在睡眠的线程？？
    注意：这个不是终断线程的执行，是终止线程的睡眠。
 */
public class ThreadTest08 {
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable2());
        t.setName("t");
        t.start();

        // 希望5秒之后，t线程醒来（5秒之后主线程手里的活儿干完了。）
        try {
            Thread.sleep(1000 * 5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 终断t线程的睡眠（这种终断睡眠的方式依靠了java的异常处理机制。）
        t.interrupt(); // 干扰，一盆冷水过去！
    }
}
class MyRunnable2 implements Runnable {
    // 重点：run()当中的异常不能throws，只能try catch
    // 因为run()方法在父类中没有抛出任何异常，子类不能比父类抛出更多的异常。
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "---> begin");
        try {
            // 睡眠1年
            Thread.sleep(1000 * 60 * 60 * 24 * 365);
        } catch (InterruptedException e) {
            // 打印异常信息
            //e.printStackTrace();
        }
        //1年之后才会执行这里
        System.out.println(Thread.currentThread().getName() + "---> end");

        // 调用doOther
        //doOther();
    }
    // 其它方法可以throws
    /*public void doOther() throws Exception{

    }*/
}
```

#### 合理中断

```java
public class ThreadTest10 {
    public static void main(String[] args) {
        MyRunable4 r = new MyRunable4();
        Thread t = new Thread(r);
        t.setName("t");
        t.start();

        // 模拟5秒
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 终止线程
        // 你想要什么时候终止t的执行，那么你把标记修改为false，就结束了。
        r.run = false;
    }
}

class MyRunable4 implements Runnable {

    // 打一个布尔标记
    boolean run = true;

    @Override
    public void run() {
        for (int i = 0; i < 10; i++){
            if(run){
                System.out.println(Thread.currentThread().getName() + "--->" + i);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }else{
                // return就结束了，你在结束之前还有什么没保存的。
                // 在这里可以保存呀。
                //save....

                //终止当前线程
                return;
            }
        }
    }
}
```

### 让位

> 让位，当前线程暂停，回到就绪状态，让给其它线程。
> 静态方法：Thread.yield();

```java
public class ThreadTest12 {
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable6());
        t.setName("t");
        t.start();

        for(int i = 1; i <= 10000; i++) {
            System.out.println(Thread.currentThread().getName() + "--->" + i);
        }
    }
}

class MyRunnable6 implements Runnable {
    @Override
    public void run() {
        for(int i = 1; i <= 10000; i++) {
            //每100个让位一次。
            if(i % 100 == 0){
                Thread.yield(); // 当前线程暂停一下，让给主线程。
            }
            System.out.println(Thread.currentThread().getName() + "--->" + i);
        }
    }
}
```

### 线程合并

```java
public class ThreadTest13 {
    public static void main(String[] args) {
        System.out.println("main begin");

        Thread t = new Thread(new MyRunnable7());
        t.setName("t");
        t.start();

        //合并线程
        try {
            t.join(); // t合并到当前线程中，当前线程受阻塞，t线程执行直到结束。
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("main over");
    }
}

class MyRunnable7 implements Runnable {
    @Override
    public void run() {
        for(int i = 0; i < 10000; i++){
            System.out.println(Thread.currentThread().getName() + "--->" + i);
        }
    }
}
```

### 守护线程

```java
public class ThreadTest14 {
    public static void main(String[] args) {
        Thread t = new BakDataThread();
        t.setName("备份数据的线程");

        // 启动线程之前，将线程设置为守护线程
        t.setDaemon(true);

        t.start();

        // 主线程：主线程是用户线程
        for(int i = 0; i < 10; i++){
            System.out.println(Thread.currentThread().getName() + "--->" + i);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class BakDataThread extends Thread {
    public void run(){
        int i = 0;
        // 即使是死循环，但由于该线程是守护者，当用户线程结束，守护线程自动终止。
        while(true){
            System.out.println(Thread.currentThread().getName() + "--->" + (++i));
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

## 消费生产者问题

```java
import java.util.ArrayList;
import java.util.List;

/*
1、使用wait方法和notify方法实现“生产者和消费者模式”

2、什么是“生产者和消费者模式”？
    生产线程负责生产，消费线程负责消费。
    生产线程和消费线程要达到均衡。
    这是一种特殊的业务需求，在这种特殊的情况下需要使用wait方法和notify方法。

3、wait和notify方法不是线程对象的方法，是普通java对象都有的方法。

4、wait方法和notify方法建立在线程同步的基础之上。因为多线程要同时操作一个仓库。有线程安全问题。

5、wait方法作用：o.wait()让正在o对象上活动的线程t进入等待状态，并且释放掉t线程之前占有的o对象的锁。

6、notify方法作用：o.notify()让正在o对象上等待的线程唤醒，只是通知，不会释放o对象上之前占有的锁。

7、模拟这样一个需求：
    仓库我们采用List集合。
    List集合中假设只能存储1个元素。
    1个元素就表示仓库满了。
    如果List集合中元素个数是0，就表示仓库空了。
    保证List集合中永远都是最多存储1个元素。

    必须做到这种效果：生产1个消费1个。
 */
public class ThreadTest16 {
    public static void main(String[] args) {
        // 创建1个仓库对象，共享的。
        List list = new ArrayList();
        // 创建两个线程对象
        // 生产者线程
        Thread t1 = new Thread(new Producer(list));
        // 消费者线程
        Thread t2 = new Thread(new Consumer(list));

        t1.setName("生产者线程");
        t2.setName("消费者线程");

        t1.start();
        t2.start();
    }
}

// 生产线程
class Producer implements Runnable {
    // 仓库
    private List list;

    public Producer(List list) {
        this.list = list;
    }
    @Override
    public void run() {
        // 一直生产（使用死循环来模拟一直生产）
        while(true){
            // 给仓库对象list加锁。
            synchronized (list){
                if(list.size() > 0){ // 大于0，说明仓库中已经有1个元素了。
                    try {
                        // 当前线程进入等待状态，并且释放Producer之前占有的list集合的锁。
                        list.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                // 程序能够执行到这里说明仓库是空的，可以生产
                Object obj = new Object();
                list.add(obj);
                System.out.println(Thread.currentThread().getName() + "--->" + obj);
                // 唤醒消费者进行消费
                list.notifyAll();
            }
        }
    }
}

// 消费线程
class Consumer implements Runnable {
    // 仓库
    private List list;

    public Consumer(List list) {
        this.list = list;
    }
    @Override
    public void run() {
        // 一直消费
        while(true){
            synchronized (list) {
                if(list.size() == 0){
                    try {
                        // 仓库已经空了。
                        // 消费者线程等待，释放掉list集合的锁
                        list.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                // 程序能够执行到此处说明仓库中有数据，进行消费。
                Object obj = list.remove(0);
                System.out.println(Thread.currentThread().getName() + "--->" + obj);
                // 唤醒生产者生产。
                list.notifyAll();
            }
        }
    }
}
```

## 定时任务

```java
public class TimerTest {
    public static void main(String[] args) throws Exception {

        // 创建定时器对象
        Timer timer = new Timer();
        //Timer timer = new Timer(true); //守护线程的方式

        // 指定定时任务
        //timer.schedule(定时任务, 第一次执行时间, 间隔多久执行一次);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        Date firstTime = sdf.parse("2020-03-14 09:34:30");
        //timer.schedule(new LogTimerTask() , firstTime, 1000 * 10);
        // 每年执行一次。
        //timer.schedule(new LogTimerTask() , firstTime, 1000 * 60 * 60 * 24 * 365);

        //匿名内部类方式
        timer.schedule(new TimerTask(){
            @Override
            public void run() {
                // code....
            }
        } , firstTime, 1000 * 10);

    }
}

// 编写一个定时任务类
// 假设这是一个记录日志的定时任务
class LogTimerTask extends TimerTask {

    @Override
    public void run() {
        // 编写你需要执行的任务就行了。
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String strTime = sdf.format(new Date());
        System.out.println(strTime + ":成功完成了一次数据备份！");
    }
}
```



## 线程的优先级

```java 
public class ThreadTest11 {
    public static void main(String[] args) {
        // 设置主线程的优先级为1
        Thread.currentThread().setPriority(1);

        /*System.out.println("最高优先级" + Thread.MAX_PRIORITY);
        System.out.println("最低优先级" + Thread.MIN_PRIORITY);
        System.out.println("默认优先级" + Thread.NORM_PRIORITY);*/

        // 获取当前线程对象，获取当前线程的优先级
        Thread currentThread = Thread.currentThread();
        // main线程的默认优先级是：5
        //System.out.println(currentThread.getName() + "线程的默认优先级是：" + currentThread.getPriority());

        Thread t = new Thread(new MyRunnable5());
        t.setPriority(10);
        t.setName("t");
        t.start();

        // 优先级较高的，只是抢到的CPU时间片相对多一些。
        // 大概率方向更偏向于优先级比较高的。
        for(int i = 0; i < 10000; i++){
            System.out.println(Thread.currentThread().getName() + "-->" + i);
        }
    }
}

class MyRunnable5 implements Runnable {
    @Override
    public void run() {
        // 获取线程优先级
        //System.out.println(Thread.currentThread().getName() + "线程的默认优先级：" + Thread.currentThread().getPriority());
        for(int i = 0; i < 10000; i++){
            System.out.println(Thread.currentThread().getName() + "-->" + i);
        }
    }
}

```



## 互斥

Java中线程的共享互斥操作，会使用synchronized关键字。线程共享互斥的架构称为监视（monitor），而获取锁有时也称为“持有(own)监视”。

每个锁在同一时刻，只能由一个线程持有。
*注意：`synchronized`方法或声明执行期间，如程序遇到任何异常或return，线程都会释放锁。*

**1、synchronized方法**

*Java示例1：*

```
//synchronized实例方法
public synchronized void deposit(int m) {
    System.out.print("This is synchronized method.");
}
```

*注：synchronized实例方法采用this锁（即当前对象）去做线程的共享互斥。*

*Java示例2：*

```
//synchronized类方法
public static synchronized void deposit(int m) {
    System.out.print("This is synchronized static method.");
}
```

*注：synchronized类方法采用类对象锁（即当前类的类对象）去做线程的共享互斥。如上述示例中，采用类.class（继承自java.lang.Class）作为锁。*

**2、synchronized声明**
*Java示例：*

```java
public void deposit(int m) {
    synchronized (this) {
        System.out.print("This is synchronized statement with this lock.");
    }
    synchronized (Something.class) {
        System.out.print("This is synchronized statement with class lock.");
    }
}
```

*注：synchronized声明可以采用任意锁，上述示例中，分别采用了对象锁（this）和类锁（something.class）*





### 中断

java.lang.Thread类有一个`interrupt`方法，该方法直接对线程调用。当被interrupt的线程正在sleep或wait时，会抛出`InterruptedException`异常。
事实上，`interrupt`方法只是改变目标线程的中断状态（interrupt status），而那些会抛出`InterruptedException`异常的方法，如wait、sleep、join等，都是在方法内部不断地检查中断状态的值。

- **interrupt方法**

Thread实例方法：必须由其它线程获取被调用线程的实例后，进行调用。实际上，只是改变了被调用线程的内部中断状态；

- **Thread.interrupted方法**

Thread类方法：必须在当前执行线程内调用，该方法返回当前线程的内部中断状态，然后清除中断状态（置为false） ；

- **isInterrupted方法**

Thread实例方法：用来检查指定线程的中断状态。当线程为中断状态时，会返回true；否则返回false。

![img](高级知识点.assets/1460000015558541)

### 2.5 协调

**1、wait set / wait方法**
wait set是一个虚拟的概念，每个Java类的实例都有一个wait set，当对象执行wait方法时，当前线程就会暂停，并进入该对象的wait set。
当发生以下事件时，线程才会退出wait set：
①有其它线程以notify方法唤醒该线程
②有其它线程以notifyAll方法唤醒该线程
③有其它线程以interrupt方法唤醒该线程
④wait方法已到期
*注：当前线程若要执行`obj.wait()`，则必须先获取该对象锁。当线程进入wait set后，就已经释放了该对象锁。*

下图中线程A先获得对象锁，然后调用wait()方法（此时线程B无法获取锁，只能等待）。当线程A调用完wait()方法进入wait set后会自动释放锁，线程B获得锁。

![img](高级知识点.assets/1460000015558542)

**2、notify方法**
notify方法相当于从wait set中从挑出一个线程并唤醒。
下图中线程A在当前实例对象的wait set中等待，此时线程B必须拿到同一实例的对象锁，才能调用notify方法唤醒wait set中的任意一个线程。
*注：线程B调用notify方法后，并不会立即释放锁，会有一段时间差。*

![img](高级知识点.assets/1460000015558543)

**3、notifyAll方法**
notifyAll方法相当于将wait set中的所有线程都唤醒。

**4、总结**
wait、notify、notifyAll这三个方法都是java.lang.Object类的方法（注意，不是Thread类的方法）。
若线程没有拿到当前对象锁就直接调用对象的这些方法，都会抛出java.lang.IllegalMonitorStateException异常。

- `obj.wait()`是把当前线程放到obj的wait set；
- `obj.notify()`是从obj的wait set里唤醒1个线程；
- `obj.notifyAll()`是唤醒所有在obj的wait set里的线程。

## 三、线程的状态转移



- 当创建一个Thread子类或实现Runnable接口类的实例时，线程进入【初始】状态；
- 调用实例的start方法后，线程进入【可执行】状态；
- 系统会在某一时刻自动调度处于【可执行】状态的线程，被调度的线程会调用run方法，进入【执行中】状态；
- 线程执行完run方法后，进入【结束】状态；
- 处于【结束】状态的线程，在某一时刻，会被JVM垃圾回收；
- 处于【执行中】状态的线程，若调用了Thread.yield方法，会回到【可执行】状态，等待再次被调度；
- 处于【执行中】状态的线程，若调用了wait方法，会进入wait set并一直等待，直到被其它线程通过notify、notifyAll、interrupt方法唤醒；
- 处于【执行中】状态的线程，若调用了Thread.sleep方法，会进入【Sleep】状态，无法继续向下执行。当sleep时间结束或被interrupt时，会回到【可执行状态】；
- 处于【执行中】状态的线程，若遇到阻塞I/O操作，也会停止等待I/O完成，然后回到【可执行状态】；





# 反射

![img](高级知识点.assets/20170513133210763)

## 反射机制有什么用？

反射机制允许程序在执行时获取某个类自身的定义信息，例如熟悉和方法等也可以实现动态创建 类的对象、变更属性的内容或执行特定的方法的功能。从而使Java具有动态语言的特性，增强了程序的灵活性和可移植性。

> ​	通过java语言中的反射机制可以操作字节码文件。
> ​	优点类似于黑客。（可以读和修改字节码文件。）
> ​	通过反射机制可以操作代码片段。（class文件。）

- （1）在运行时判断任意一个对象所属的类型。
- （2）在运行时构造任意一个类的对象。
- （3）在运行时判断任意一个类所具有的成员变量和方法。
- （4）在运行时调用任意一个对象的方法，甚至可以调用private方法。

注意：上述功能都是在**运行时环境**中，而不是在编译时环境中。

## 反射机制相关的重要的类

​	java.lang.Class：代表整个字节码，代表一个类型，代表整个类。

​	java.lang.reflect.Method：代表字节码中的方法字节码。代表类中的方法。

​	java.lang.reflect.Constructor：代表字节码中的构造方法字节码。代表类中的构造方法

​	java.lang.reflect.Field：代表字节码中的属性字节码。代表类中的成员变量（静态变量+实例变量）

> （1）Class类：代表一个类。
>
> （2）Filed类：代表类的成员变量。
>
> （3）Method类：代表类的方法。
>
> （4）Constructor类：代表类的构造方法。
>
> （5）Array类：提供了动态创建数组及访问数组元素的静态方法。该类中的所有方法都是静态的。

反射机制的相关类在	**java.lang.reflect.***;

## 三种方式获取类的字节码

###         第一种：Class c = Class.forName("完整类名带包名");

> Class类中的静态方法public static Class forName(String classname)——常用方法。
>
> ​        1、静态方法
> ​        2、方法的参数是一个字符串。
> ​        3、字符串需要的是一个完整类名。
> ​        4、完整类名必须带有包名。java.lang包也不能省略。

```java
public static void main(String[] args) throws Exception{

    // 这种方式代码就写死了。只能创建一个User类型的对象
    //User user = new User();

    // 以下代码是灵活的，代码不需要改动，可以修改配置文件，配置文件修改之后，可以创建出不同的实例对象。
    // 通过IO流读取classinfo.properties文件
    FileReader reader = new FileReader("chapter25/classinfo2.properties");
    // 创建属性类对象Map
    Properties pro = new Properties(); // key value都是String
    // 加载
    pro.load(reader);
    // 关闭流
    reader.close();

    // 通过key获取value
    String className = pro.getProperty("className");
    //System.out.println(className);

    // 通过反射机制实例化对象
    Class c = Class.forName(className);
    Object obj = c.newInstance();
    System.out.println(obj);
}
```

###         第二种：Class c = 对象.getClass();

```java
// java中任何一个对象都有一个方法：getClass()
        String s = "abc";
        Class x = s.getClass(); // x代表String.class字节码文件，x代表String类型。
        System.out.println(c1 == x); // true（==判断的是对象的内存地址。）

        Date time = new Date();
        Class y = time.getClass();
        System.out.println(c2 == y); // true (c2和y两个变量中保存的内存地址都是一样的，都指向方法区中的字节码文件。)
```



###         第三种：Class c = 任何类型.class;

```java
//java语言中任何一种类型，包括基本数据类型，它都有.class属性。
        Class z = String.class; // z代表String类型
        Class k = Date.class; // k代表Date类型
        Class f = int.class; // f代表int类型
        Class e = double.class; // e代表double类型
        System.out.println(x == z); // true
```



## 反射Method



```java
    public static void main(String[] args) throws Exception{
        // 获取类了
        Class userServiceClass = Class.forName("com.bjpowernode.java.service.UserService");

        // 获取所有的Method（包括私有的！）
        Method[] methods = userServiceClass.getDeclaredMethods();
        //System.out.println(methods.length); // 2

        // 遍历Method
        for(Method method : methods){
            // 获取修饰符列表
            System.out.println(Modifier.toString(method.getModifiers()));
            // 获取方法的返回值类型
            System.out.println(method.getReturnType().getSimpleName());
            // 获取方法名
            System.out.println(method.getName());
            // 方法的修饰符列表（一个方法的参数可能会有多个。）
            Class[] parameterTypes = method.getParameterTypes();
            for(Class parameterType : parameterTypes){
                System.out.println(parameterType.getSimpleName());
            }
        }
    }
```

## ★反射机制修改属性

```java
 public static void main(String[] args) throws Exception{
        // 我们不使用反射机制，怎么去访问一个对象的属性呢？
        Student s = new Student();
        // 给属性赋值
        s.no = 1111; //三要素：给s对象的no属性赋值1111
                    //要素1：对象s
                    //要素2：no属性
                    //要素3：1111
        // 读属性值
        // 两个要素：获取s对象的no属性的值。
        System.out.println(s.no);

        // 使用反射机制，怎么去访问一个对象的属性。（set get）
        Class studentClass = Class.forName("com.bjpowernode.java.bean.Student");
        Object obj = studentClass.newInstance(); // obj就是Student对象。（底层调用无参数构造方法）

        // 获取no属性（根据属性的名称来获取Field）
        Field noFiled = studentClass.getDeclaredField("no");

        // 给obj对象(Student对象)的no属性赋值
        /*
        虽然使用了反射机制，但是三要素还是缺一不可：
            要素1：obj对象
            要素2：no属性
            要素3：2222值
        注意：反射机制让代码复杂了，但是为了一个“灵活”，这也是值得的。
         */
        noFiled.set(obj, 22222); // 给obj对象的no属性赋值2222

        // 读取属性的值
        // 两个要素：获取obj对象的no属性的值。
        System.out.println(noFiled.get(obj));

        // 可以访问私有的属性吗？
        Field nameField = studentClass.getDeclaredField("name");

        // 打破封装（反射机制的缺点：打破封装，可能会给不法分子留下机会！！！）
        // 这样设置完之后，在外部也是可以访问private的。
        nameField.setAccessible(true);

        // 给name属性赋值
        nameField.set(obj, "jackson");
        // 获取name属性的值
        System.out.println(nameField.get(obj));
    }
```

## ★反射机制调用对象方法

> 1. 获取成员方法：
>    public Method getMethod(String name ，Class<?>… parameterTypes):获取"公有方法"；（包含了父类的方法也包含Object类）
>    public Method getDeclaredMethods(String name ，Class<?>… parameterTypes) :获取成员方法，包括私有的(不包括继承的)
>    参数解释：
>     name : 方法名；
>     Class … : 形参的Class类型对象
> 2. 调用方法
>    Method --> public Object invoke(Object obj,Object… args):
>    参数说明：
>     obj : 要调用方法的对象；
>     args:调用方式时所传递的实参；

```java
    public static void main(String[] args) throws Exception{
        // 不使用反射机制，怎么调用方法
        // 创建对象
        UserService userService = new UserService();
        // 调用方法
        /*
        要素分析：
            要素1：对象userService
            要素2：login方法名
            要素3：实参列表
            要素4：返回值
         */
        boolean loginSuccess = userService.login("admin","123");
        //System.out.println(loginSuccess);
        System.out.println(loginSuccess ? "登录成功" : "登录失败");

        // 使用反射机制来调用一个对象的方法该怎么做？
        Class userServiceClass = Class.forName("com.bjpowernode.java.service.UserService");
        // 创建对象
        Object obj = userServiceClass.newInstance();
        // 获取Method
        Method loginMethod = userServiceClass.getDeclaredMethod("login", String.class, String.class);
        //Method loginMethod = userServiceClass.getDeclaredMethod("login", int.class);
        // 调用方法
        // 调用方法有几个要素？ 也需要4要素。
        // 反射机制中最最最最最重要的一个方法，必须记住。
        /*
        四要素：
        loginMethod方法
        obj对象
        "admin","123" 实参
        retValue 返回值
         */
        Object retValue = loginMethod.invoke(obj, "admin","123123");
        System.out.println(retValue);
    }

class UserService {
    /**
     * 登录方法
     * @param name 用户名
     * @param password 密码
     * @return true表示登录成功，false表示登录失败！
     */
    public boolean login(String name,String password){
        if("admin".equals(name) && "123".equals(password)){
            return true;
        }
        return false;
    }

    // 可能还有一个同名login方法
    // java中怎么区分一个方法，依靠方法名和参数列表。
    public void login(int i){
    }
    /**
     * 退出系统的方法
     */
    public void logout(){
        System.out.println("系统已经安全退出！");
    }
}
```





# 注解

注解，或者叫做注释类型，英文单词是：Annotation。n）又称 Java 标注，是 JDK5.0 引入的一种注释机制。注解Annotation是一种引用数据类型。编译之后也是生成xxx.class文件。

## 自定义注解

```java
[修饰符列表] @interface 注解类型名{
 }
```

```java
public @interface MyAnnotation {
    /**
     * 我们通常在注解当中可以定义属性，以下这个是MyAnnotation的name属性。
     * 看着像1个方法，但实际上我们称之为属性name。
     * 如果一个注解的属性的名字是value，并且只有一个属性的话，在使用的时候，该属性名可以省略。
     */
    String name();
    /*
    颜色属性
     */
    String color();
    /*
    年龄属性
     */
    int age() default 25; //属性指定默认值
    /*
    邮箱地址属性，支持多个
     */
    String[] email();

    /**
     * 季节数组，Season是枚举类型
     * @return
     */
    Season[] seasonArray();
}
enum Season {
    SPRING,SUMMER,AUTUMN,WINTER
}
```

```java
public class MyAnnotationTest {
    // 报错的原因：如果一个注解当中有属性，那么必须给属性赋值。（除非该属性使用default指定了默认值。）
    /*@MyAnnotation
    public void doSome(){
    }*/
    //@MyAnnotation(属性名=属性值,属性名=属性值,属性名=属性值)
    //指定name属性的值就好了。
    // 如果数组中只有1个元素：大括号可以省略。
    @MyAnnotation(name = "zhangsan", color = "红色",age = 25, email = {"zhangsan@123.com", "zhangsan@sohu.com"}, seasonArray = Season.WINTER)
    public void doSome(){
    }
}
```



### 注解怎么使用，用在什么地方？

​	第一：注解使用时的语法格式是：
​		@注解类型名
​	
​	第二：注解可以出现在类上、属性上、方法上、变量上等....
​	注解还可以出现在注解类型上。

## 需求

​		假设有这样一个注解，叫做：@Id
​		这个注解只能出现在类上面，当这个类上有这个注解的时候，要求这个类中必须有一个int类型的id属性。如果没有这个属性就报异常。如果有这个属性则正常执行！

```java
// 表示这个注解只能出现在类上面
@Target(ElementType.TYPE)
// 该注解可以被反射机制读取到
@Retention(RetentionPolicy.RUNTIME)
public @interface MustHasIdPropertyAnnotation {
}
/*
自定义类
 */
@MustHasIdPropertyAnnotation
class User {
    int id;
    String name;
    String password;
}
/*
自定义异常
 */
class HasNotIdPropertyException extends RuntimeException {
    public HasNotIdPropertyException(){
    }
    public HasNotIdPropertyException(String s){
        super(s);
    }
}
/*
测试
 */
public class Test {
    public static void main(String[] args) throws Exception{
        // 获取类
        Class userClass = Class.forName("com.bjpowernode.java.annotation7.User");
        // 判断类上是否存在Id注解
        if(userClass.isAnnotationPresent(MustHasIdPropertyAnnotation.class)){
            // 当一个类上面有@MustHasIdPropertyAnnotation注解的时候，要求类中必须存在int类型的id属性
            // 如果没有int类型的id属性则报异常。
            // 获取类的属性
            Field[] fields = userClass.getDeclaredFields();
            boolean isOk = false; // 给一个默认的标记
            for(Field field : fields){
                if("id".equals(field.getName()) && "int".equals(field.getType().getSimpleName())){
                    // 表示这个类是合法的类。有@Id注解，则这个类中必须有int类型的id
                    isOk = true; // 表示合法
                    break;
                }
            }

            // 判断是否合法
            if(!isOk){
                throw new HasNotIdPropertyException("被@MustHasIdPropertyAnnotation注解标注的类中必须要有一个int类型的id属性！");
            }

        }
    }
}
```



## 内置注解

- **@Override** - 检查该方法是否是重写方法。如果发现其父类，或者是引用的接口中并没有该方法时，会报编译错误。只能注解方法。
- **@Deprecated** - 标记过时方法。如果使用该方法，会报编译警告。不鼓励程序员使用这样的元素，通常是因为它很危险或存在更好的选择。 
- @SuppressWarnings - 指示编译器去忽略注解中声明的警告。取消显示指定的编译器警告。 

#### 作用在其他**注解的注解**(或者说 **元注解**)是:

- **@Retention** - 标识这个注解怎么保存，是只在代码中，还是编入class文件中，或者是在运行时可以通过反射访问。

> ​		@Retention(RetentionPolicy.SOURCE)：表示该注解只被保留在java源文件中。
> ​		@Retention(RetentionPolicy.CLASS)：表示该注解被保存在class文件中。它是 Annotation 的默认行为。
> ​		@Retention(RetentionPolicy.RUNTIME)：表示该注解被保存在class文件中，并且可以被反射机制所读取。
>
> 

- @Documented - 标记这些注解是否包含在用户文档中。
- **@Target** - 标记这个注解应该是哪种 Java 成员。这个Target注解用来标注“被标注的注解”可以出现在哪些位置上。

> ​	@Target(ElementType.METHOD)
>
> ​			表示**“被标注的注解”**只能出现在方法上。
> ​	@Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, MODULE, PARAMETER, TYPE})
> ​		  表示该注解可以出现在：构造方法上、字段上、局部变量上、方法上....类上.



- @Inherited - 标记这个注解是继承于哪个注解类(默认 注解并没有继承于任何子类)

#### 额外添加了 3 个注解:

- @SafeVarargs - Java 7 开始支持，忽略任何使用参数为泛型变量的方法或构造函数调用产生的警告。
- @FunctionalInterface - Java 8 开始支持，标识一个匿名函数或函数式接口。
- @Repeatable - Java 8 开始支持，标识某注解可以在同一个声明上使用多次。



## Annotation架构

![img](高级知识点.assets/28123151-d471f82eb2bc4812b46cc5ff3e9e6b82.jpg)

**(01) 1 个 Annotation 和 1 个 RetentionPolicy 关联。**

可以理解为：每1个Annotation对象，都会有唯一的RetentionPolicy属性。

**(02) 1 个 Annotation 和 1~n 个 ElementType 关联。**

可以理解为：对于每 1 个 Annotation 对象，可以有若干个 ElementType 属性。

**(03) Annotation 有许多实现类，包括：Deprecated, Documented, Inherited, Override 等等。**

Annotation 的每一个实现类，都 "和 1 个 RetentionPolicy 关联" 并且 " 和 1~n 个 ElementType 关联"。



## 反射注解

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@interface MyAnnotation {
    /*
    username属性
     */
    String username();

    /*
    password属性
     */
    String password();
}


public class MyAnnotationTest {

    @MyAnnotation(username = "admin", password = "456456")
    public void doSome(){

    }
    public static void main(String[] args) throws Exception{
        // 获取MyAnnotationTest的doSome()方法上面的注解信息。
        Class c = Class.forName("com.bjpowernode.java.annotation6.MyAnnotationTest");
        // 获取doSome()方法
        Method doSomeMethod = c.getDeclaredMethod("doSome");
        // 判断该方法上是否存在这个注解
        if(doSomeMethod.isAnnotationPresent(MyAnnotation.class)) {
            MyAnnotation myAnnotation = doSomeMethod.getAnnotation(MyAnnotation.class);
            System.out.println(myAnnotation.username());
            System.out.println(myAnnotation.password());
        }
    }
}
```





# Java 文档注释

Java 支持三种注释方式。前两种分别是 **//** 和 **/\* \*/**，第三种被称作说明注释，它以 **/\**** 开始，以 ***/**结束。

说明注释允许你在程序中嵌入关于程序的信息。你可以使用 javadoc 工具软件来生成信息，并输出到HTML文件中。

说明注释，使你更加方便的记录你的程序信息。

------

## javadoc 标签

javadoc 工具软件识别以下标签：

| **标签**      |                        **描述**                        |                           **示例**                           |
| :------------ | :----------------------------------------------------: | :----------------------------------------------------------: |
| @author       |                    标识一个类的作者                    |                     @author description                      |
| @deprecated   |                 指名一个过期的类或成员                 |                   @deprecated description                    |
| {@docRoot}    |                指明当前文档根目录的路径                |                        Directory Path                        |
| @exception    |                  标志一个类抛出的异常                  |            @exception exception-name explanation             |
| {@inheritDoc} |                  从直接父类继承的注释                  |      Inherits a comment from the immediate surperclass.      |
| {@link}       |               插入一个到另一个主题的链接               |                      {@link name text}                       |
| {@linkplain}  |  插入一个到另一个主题的链接，但是该链接显示纯文本字体  |          Inserts an in-line link to another topic.           |
| @param        |                   说明一个方法的参数                   |              @param parameter-name explanation               |
| @return       |                     说明返回值类型                     |                     @return explanation                      |
| @see          |               指定一个到另一个主题的链接               |                         @see anchor                          |
| @serial       |                   说明一个序列化属性                   |                     @serial description                      |
| @serialData   | 说明通过writeObject( ) 和 writeExternal( )方法写的数据 |                   @serialData description                    |
| @serialField  |             说明一个ObjectStreamField组件              |              @serialField name type description              |
| @since        |               标记当引入一个特定的变化时               |                        @since release                        |
| @throws       |                 和 @exception标签一样.                 | The @throws tag has the same meaning as the @exception tag.  |
| {@value}      |         显示常量的值，该常量必须是static属性。         | Displays the value of a constant, which must be a static field. |
| @version      |                      指定类的版本                      |                        @version info                         |

------

## 文档注释

在开始的 **/\**** 之后，第一行或几行是关于类、变量和方法的主要描述。

之后，你可以包含一个或多个各种各样的 **@** 标签。每一个 **@** 标签必须在一个新行的开始或者在一行的开始紧跟星号(*).

多个相同类型的标签应该放成一组。例如，如果你有三个 **@see** 标签，可以将它们一个接一个的放在一起。

下面是一个类的说明注释的实例：

```java
/*** 这个类绘制一个条形图 
* @author runoob 
* @version 1.2 
*/
```



------

## javadoc 输出什么

javadoc 工具将你 Java 程序的源代码作为输入，输出一些包含你程序注释的HTML文件。

每一个类的信息将在独自的HTML文件里。javadoc 也可以输出继承的树形结构和索引。

由于 javadoc 的实现不同，工作也可能不同，你需要检查你的 Java 开发系统的版本等细节，选择合适的 Javadoc 版本。

### 实例

下面是一个使用说明注释的简单实例。注意每一个注释都在它描述的项目的前面。

在经过 javadoc 处理之后，SquareNum 类的注释将在 SquareNum.html 中找到。





