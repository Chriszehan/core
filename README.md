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

要把任意基础类型或引用类型转换为字符串，可以使用静态方法valueOf()。这是一个重载方法，编写



































































































