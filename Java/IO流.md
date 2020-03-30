## 关闭流

### 1.在 try 中关闭

在 try 的作用域里关闭文件输入流，在前面的示例中都是使用这种方式，这样做有一个弊端；
如果文件不存在，或者读取的时候出现问题而抛出异常，那么就不会执行这一行关闭流的代码，存在巨大的资源占用隐患。`不推荐`使用

```java
package stream;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class TestStream {

    public static void main(String[] args) {
        try {
            File f = new File("d:/lol.txt");
            FileInputStream fis = new FileInputStream(f);
            byte[] all = new byte[(int) f.length()];
            fis.read(all);
            for (byte b : all) {
                System.out.println(b);
            }
            // 在try 里关闭流
            fis.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```

### 2.在 finally 中关闭

这是标准的关闭流的方式

1. 首先把流的引用声明在 try 的外面，如果声明在 try 里面，其作用域无法抵达 finally.
2. 在 finally 关闭之前，要先判断该引用是否为空
3. 关闭的时候，需要再一次进行 try catch 处理

这是标准的严谨的关闭流的方式，但是看上去很繁琐，所以写不重要的或者测试代码的时候，都会采用上面的`有隐患`try 的方式，因为不麻烦~

```java
package stream;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class TestStream {

    public static void main(String[] args) {
        File f = new File("d:/lol.txt");
        FileInputStream fis = null;
        try {
            fis = new FileInputStream(f);
            byte[] all = new byte[(int) f.length()];
            fis.read(all);
            for (byte b : all) {
                System.out.println(b);
            }

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 在finally 里关闭流
            if (null != fis)
                try {

                    fis.close();
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
        }

    }
}
```

### 3.使用 try()的方式

把流定义在 try()里,try,catch 或者 finally 结束的时候，会自动关闭
这种编写代码的方式叫做`try-with-resources`， 这是从 JDK7 开始支持的技术

所有的流，都实现了一个接口叫做`AutoCloseable`，任何类实现了这个接口，都可以在 try()中进行实例化。 并且在 try, catch, finally 结束的时候自动关闭，回收相关资源。

```java
package stream;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class TestStream {

    public static void main(String[] args) {
        File f = new File("d:/lol.txt");

        //把流定义在try()里,try,catch或者finally结束的时候，会自动关闭
        try (FileInputStream fis = new FileInputStream(f)) {
            byte[] all = new byte[(int) f.length()];
            fis.read(all);
            for (byte b : all) {
                System.out.println(b);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```
