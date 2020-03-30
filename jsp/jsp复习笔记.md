# jsp 复习笔记

> jsp 实用教程(第三版)

## jsp

### jsp 的语法

jsp 页面可由五种元素组合而成：

1. 普通的 HTML 标记符
2. jsp 标记，如指令标记，动作标记
3. 变量和方法的声明
4. java 程序片
5. java 表达式

定义方法：
`<%!`方法`%>`
java 程序片:
`<%`程序片`%>`
java 表达式:
`<%=`java 表达式`%>`
jsp 注释
`<%--`注释内容`--%>`

### 指令标记

#### page 指令标记

#### include 指令标记

### 动作标记

#### include 动作标记

#### param 动作标记

#### forward 动作标记

#### useBean 动作标记

### jsp 内置对象

#### request 对象

#### response 对象

#### session 对象

#### application 对象

#### out 对象

## javaBean

### 获取和修改 bean 的属性

#### getProperty 动作标记

#### setProperty 动作标记

## Servlet

### 编写部署文件 web.xml

### servlet 工作原理

servlet 由 tomcat 服务器负责管理，tomcat 服务器通过读取 web.xml 创建并运行 servlet

#### servlet 对象的生命周期

1. 初始化 servlet。servlet 第一次被请求加载时，服务器初始化这个 servlet，即创建一个 servlet，这个 servlet 调用 init 方法完成必要的的初始化工作
2. 新诞生的 servlet 再调用 service 方法响应用户的请求
3. 当服务器关闭时候，调用 destroy 方法摧毁 serlet

#### init 方法

```java
public void init(ServletConfig config)throws ServletException
```

#### service 方法

```java
public void service(HttpServletRequest request, HttpServletResponse response)throw ServletException,IOException
```

#### destroy 方法

```java
public destroy()
```

#### 通过 jsp 页面访问 servlet

#### doGet 和 doPost 方法

### 重定向与转发

#### sendRedirect 方法

#### RequestDispatcher 对象

#### session

## MVC 模式

Model-View-Controller

1. 模型(Model)：一个或多个 javabean 对象，用于存储数据
2. 视图(View)：一个活多个 jsp 页面，作用是向控制器提交必要的数据和显示数据
3. 控制器(Controller)：一个活多个 servlet 对象，根据视图提交的要求进行数据处理操作，并将有关结果存储到 javabean 中，然后 servlet 使用转发或重定向的方式请求视图中的某个 jsp 页面显示数据

### 模型的生命周期与视图更新

#### request 周期的 javabean

#### session 周期的 javabean

#### application 周期的 javabean

## 数据库操作

## 文件操作
