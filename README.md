# core

## String

在Java中，String是一个引用类型，本身是一个class。

但是java编译器对String有特殊的处理，既可以直接使用"..."来表示一个字符串：

String s1 = "Hello!";

实际上字符串在String内部是通过一个char[] 数组表示的，因此，按照下面的写法也是可以的：

String s2 = new String(new char[] {'H', 'e', 'l', 'l', 'o', '!'});

因为String太常用了，所以Java提供了"..."这种字符串字面量表示方法。

java字符串的一个重要特点就是字符串 不可变。 这种不可变性是通过内部的private final char[]字段，以及没有任何修改char[]的方法实现的。

### 字符串比较

当我们想要比较两个字符串是否相同时，要特别注意，我们实际上时想比较字符串的内容是否相同。
必须使用equals()方法而不能用==。

// String
public class Main {
    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = "hello";
        System.out.println(s1 == s2);
        System.out.println(s1.equals(s2));
    }
}

表面上看用==和equals比较都为true，但实际上只是因为Java编译器在编译期，会自动把相同的字符串当作一个对象放入常量池，自然s1和S2的引用就是相同的。

换一种写法，==比较就会失败：

// String
public class Main {
    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = "HELLO".toLowerCase();
        System.out.println(s1 == s2);
        System.out.println(s1.equals(s2));
    }
}

String类还提供了多种方法来搜索字串、提取字串。常用的方法有：

// 是否包含子串:
"Hello".contains("ll"); // true

注意到contain()方法的参数是CharSequence而不是String，因为CharSequence是String实现的一个接口。

搜索子串的更多例子：

"Hello".indexOf("l"); // 2
"Hello".lastIndexOf("l"); // 3
"Hello".startsWith("He"); // true
"Hello".endsWith("lo"); // true

提取字串的例子：
"Hello".subString(2);  //"llo"
"Hello".subString(2,4); //"ll"

### 去除首尾空白字符

使用trim()方法可以移除字符串首尾空白字符。空白字符包括空格，\t,\r,\n:
" \tHello\r\n".trim();  //"Hello"

注意trim()并没有改变字符串的内容，而是返回了一个新的字符串。

另一个方法strip()方法也可以移除字符串首尾空白字符。它和trim()不同的是，类似中文的空个字符\u3000也会被移除

"\u3000Hello\u3000".strip(); // "Hello"
" Hello ".stripLeading(); // "Hello "
" Hello ".stripTrailing(); // " Hello"

String 还提供了isEmpty()和isBlank()来判断字符串是否为空和空白字符串：

"".isEmpty(); // true，因为字符串长度为0
"  ".isEmpty(); // false，因为字符串长度不为0
"  \n".isBlank(); // true，因为只包含空白字符
" Hello ".isBlank(); // false，因为包含非空白字符

### 替换子串
要在字符串中替换字串，有两种方法。一种是根据字符或字符串替换：
String s = "hello";
s.replace('l', 'w'); // "hewwo"，所有字符'l'被替换为'w'
s.replace("ll", "~ ~"); // "he~ ~o"，所有子串"ll"被替换为"~~"

另一种则是通过正则表达式替换：
String s = "A,,B;C ,D";
s.replaceAll("[\\,\\;\\s]+", ","); // "A,B,C,D"

### 分割字符串

要分割字符串，使用split()方法，并且传入的也是正则表达式：
String s = "A,B,C,D";
String[] ss = s.split("\\,"); //{"A","B","C","D"}


### 拼接字符串

拼接字符串使用静态方法join(),它用指定的字符串连接字符串数组：

String[] arr = {"A", "B", "C"};
String s = String.join("** *", arr); // "A** *B** *C"

### 格式化字符串

字符串提供了formatted() 方法和format()静态方法，可以传入其他参数，替换占位符，然后生成新的字符串：

// String
public class Main {
    public static void main(String[] args) {
        String s = "Hi %s, your score is %d!";
        System.out.println(s.formatted("Alice", 80));
        System.out.println(String.format("Hi %s, your score is %.2f!", "Bob", 59.5));
    }
}

有几个占位符，后面就需要传入几个参数。参数类型要和占位符一致。我们经常用这个方法来格式化信息。常用的占位符有：

%s：显示字符串；
%d：显示整数；
%x：显示十六进制整数；
%f：显示浮点数。

占位符还可以带格式，例如%.2f表示显示两位小数。如果你不确定用啥占位符，那就始终用%s，因为%s可以显示任何数据类型。要查看完整的格式化语法，请参考JDK文档。

### 类型转换

要把任意基础类型或引用类型转换为字符串，可以使用静态方法valueOf()。这是一个重载方法，编译器会根据参数自动选择合适的方法：

String.valueOf(123); // "123"
String.valueOf(45.67); // "45.67"
String.valueOf(true); // "true"
String.valueOf(new Object()); // 类似java.lang.Object@636be97c

要把字符串转换为其他类型，就需要根据情况。例如，把字符串转换为int类型：

int n1 = Integer.parseInt("123"); // 123
int n2 = Integer.parseInt("ff", 16); // 按十六进制转换，255

把字符串转换为boolean类型：
boolean b1 = Boolean.parseBoolean("true");//true
boolean b2 = boolean.parseBoolean("False");//false

注意 Integer有个getInteger(String)方法，它不是将字符串转换为int，而是把字符串对应的系统变量转换为Integer

Integer.getInteger("java.version"); // 版本号，11

### 转换为char[]

String和char[]类型可以互相转换方法是：

char[] cs = "Hello".toCharArray(); //String ->char[]
String s = new String(cs); // char[]-->String

如修改了char[] 数组，String并不会改变：

// String <-> char[]
public class Main {
    public static void main(String[] args) {
        char[] cs = "Hello".toCharArray();
        String s = new String(cs);
        System.out.println(s);
        cs[0] = 'X';
        System.out.println(s);
    }
}

这是因为通过new String(char[]) 创建新的String实例时，它并不会直接引用传入的char[]数组，而是会复制一份，所以修改外部的char[]数组不会影响String实例内部的char[]数组，因为这是两个不同的数组。

从String不变性设计，如果传入的对象有可能改变，我们需要复制而不是直接引用。

// int[]
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] scores = new int[] { 88, 77, 51, 66 };
        Score s = new Score(scores);
        s.printScores();
        scores[2] = 99;
        s.printScores();
    }
}

class Score {
    private int[] scores;
    public Score(int[] scores) {
        this.scores = scores;
    }

    public void printScores() {
        System.out.println(Arrays.toString(scores));
    }
}

观察两次输出，由于Score内部直接引用了外部传入的int[]数组，这会造成外部代码对int[]数组的修改，影响到score

### 字符编码

全球统一编码Unicode 
变长编码UTF-8  因为unicode编码高字节总是00 

在java中 char类型实际上就是两个字节的Unicode编码。如果我们要手动把字符串转换成其他编码，可以这样：

byte[] b1 = "Hello".getBytes(); // 按系统默认编码转换，不推荐
byte[] b2 = "Hello".getBytes("UTF-8"); // 按UTF-8编码转换
byte[] b2 = "Hello".getBytes("GBK"); // 按GBK编码转换
byte[] b3 = "Hello".getBytes(StandardCharsets.UTF_8); // 按UTF-8编码转换

注意 转换编码后就不再是char类型，而是byte类型表示的数组。

如果要把已知编码的byte[]转换为String，可以这样：

String s = new String(b,"GBK"); //按 GBK转换
String s2 = new String(b,StandardCharsets.UTF_8); //按UTF-8转换

谨记 Java的String和char在内存中总是以Unicode编码表示。


























































































