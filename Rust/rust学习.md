# rRust 学习

## 环境搭建

我选择了wsl2+vscode的模式

### Cargo

是Rust的构建系统和包管理器，先了解好配套工具再学Rust也不迟

### 基本操作

`cargo build` 在**target/debug**中生成可执行文件

`cargo run` 执行`cargo build`的操作并**运行**可执行文件

`cargo check` 快速检查代码确保可编译，但**不产生可执行文件**

`cargo build/run --release` 会在**target/release**下生成可执行文件，性能会提升10倍以上，但是**编译速度会变慢**。

## Rust基础语法

### 不可变变量

### mut关键字

### 重影(Shadowing)

重影就是指变量的名称可以被重新使用的机制。

```rust
fn main() {
    let x = 5;
    let x = x + 1;
    let x = x * 2;
    println!("The value of x is: {}", x);
}
// 运行结果为
// The value of x is: 12
```

重影与可变变量的赋值不是一个概念，重影是指用同一个名字重新代表另一个变量实体，其类型、可变属性和值都可以变化。但可变变量赋值仅能发生值的变化。

### 函数

#### 函数返回值

Rust 函数声明返回值类型的方式：在参数声明之后用 `->` 来声明函数返回值的类型（不是 `:` ）。

#### 条件函数

```rust
// 通过if-else结构实现类似三元条件运算表达式（A?B:C）
fn main() {
    let a = 3;
    // a大于0输出1，否则输出-1
    let number = if a > 0 { 1 } else { -1 };
    println!("number 为 {}", number);
}
```

#### 循环函数

##### while循环

在 C 语言中 for 循环使用三元语句控制循环，但是 Rust 中没有这种用法，需要用 while 循环来代替：

```c
// C语言
int i;
for (i = 0; i < 10; i++) {
    // 循环体
}
```

```rust
// Rust语言
let mut i = 0;
while i < 10 {
    // 循环体
    i += 1;
}
```

##### for循环

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];
    for i in a.iter() {  // iter()代表a的迭代器
        println!("值为 : {}", i);
    }
}

// 值为 : 10
// 值为 : 20
// 值为 : 30
// 值为 : 40
// 值为 : 50
```

```rust
fn main() {
let a = [10, 20, 30, 40, 50];
    for i in 0..5 {
        println!("a[{}] = {}", i, a[i]);
    }
}

// a[0] = 10
// a[1] = 20
// a[2] = 30
// a[3] = 40
// a[4] = 50
```



##### loop循环

Rust 语言有原生的无限循环结构 —— loop：

```rust
fn main() {
    let s = ['R', 'U', 'N', 'O', 'O', 'B'];
    let mut i = 0;
    loop {
        let ch = s[i];
        if ch == 'O' {
            break;
        }
        println!("\'{}\'", ch);
        i += 1;
    }
}
```

loop 循环可以通过 break 关键字类似于 return 一样使整个循环退出并给予外部一个返回值。这是一个十分巧妙的设计，因为 loop 这样的循环常被用来当作查找工具使用，如果找到了某个东西当然要将这个结果交出去。

> Rust 不支持 **++** 和 **--**，因为这两个运算符出现在变量的前后会影响代码可读性，减弱了开发者对变量改变的意识能力。