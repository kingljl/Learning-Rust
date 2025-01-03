# 六、Rust 常量

常量就是那些值不能被改变的变量。一旦我们定义了一个常量，那么就再也没有任何方法可以改变常量的值了。

Rust 语言中使用 const 关键字来定义一个常量。定义常量时需要明确指定常量的数据类型。

## 6.1 定义常量的语法

Rust 中定义常量的语法格式如下

```
const VARIABLE_NAME:dataType = value;
```

从语法上来看，定义常量和定义变量的语法是类似的：
- 定义常量的关键字是 const ，而定义变量的关键字是 let。
- 定义常量时必须指定数据类型，而定义变量时数据类型可以省略，因为会自动推导。
- 常量名的命名规则可变量的命名规则一样，但常量名一般都是 大写字母。

## 6.2 Rust 常量命名规则

有点重复了，不过我们还是得重复说一下 常量的命名规则，常量命名规则和变量的命名规则是类似的，除了以下几点：
- 常量名通常都是 大写字母。
- 使用 const 关键字而不是 let 关键字来定义一个常量。
- 根据这些命名规则，我们来看一些定义常量的范例

```
fn main() {
   const USER_LIMIT:i32 = 100;    // 定义了一个 i32 类型的常量
   const PI:f32 = 3.14;           // 定义了一个 float 类型的常量

   println!("user limit is {}",USER_LIMIT);  // 显示常量的值
   println!("pi value is {}",PI);            // 显示常量的值
}
```

## 6.3 Rust 中常量与变量的不同之处

Rust 中常量与变量的不同之处是面试必问的经典题目之一。

下面我们就来详细讲讲这个面试题的答案吧。

1. 声明常量使用的是 const 关键字，声明变量使用的是 let 关键字。

2. 声明变量时 数据类型可以忽略，但声明常量时数据类型不可以忽略。这就意味着 const USER_LIMIT=100 这种常量声明会导致编译错误。

3. 虽然声明变量时使用 let 关键字也会导致 变量不可以重新赋值，但我们可以加上 mut 关键字来让变量可以被重新赋值。然而常量却没有这种机制，常量一旦定义就永远不可变更和重新赋值。

4. 常量只能 被赋值为 常量表达式/数学表达式，不能是 函数返回值 或者其它只有在运行时才能确定的值。这是因为 常量 只是一个符号，会在 编译时 替换为具体的值，这个有点类似于 C 语言中的 #define 定义的符号。

5. 常量可以在任意作用域里定义，包括全局作用域。也就是可以在任何地方定义。因此我们可以在需要的地方定义，以明确我们为什么需要定义一个常量。

6. 不能出现同名常量，变量可以（隐藏/屏蔽）

## 6.4 常量和变量的隐藏/屏蔽

Rust 语言中允许重复定义一个相同变量名的变量。这种重名的变量的规则是 后面定义的变量会重写/屏蔽 前面定义的同名变量。

我们使用一个范例来演示下

```
#[allow(unused_variables)]//#[warn(unused_variables)] is default, so we can ignore it
fn main() {
   let salary = 100.00;
   let salary = 1.50 ; 
   // 输出薪水
   println!("salary 变量的值为:{}",salary);
}
```

编译运行以上 Rust 代码，输出结果如下

```
salary 变量的值为:1.5
```

上面的代码，我们定义了两个同名的变量 salary，第一次 salary 我们赋值 100.00 ，第二次 salary 我们赋值为 1.5。

从输出结果中可以看出，第二个 salary 会隐藏/屏蔽第一次定义的变量。

## 6.5 同名变量可以有不同的数据类型

Rust 支持在同一个作用域/内层作用域内定义多个同名变量，而且每个同名变量的类型还可以不一样。

我们使用一个简单的范例来演示下 同名变量可以有不同的数据类型

```
fn main() {
   let uname = "Mohtashim";
   let uname = uname.len();
   println!("uname 字符串的字符数是: {}",uname);
}
```

编译运行以上 Rust 代码，输出结果如下

```
uname 字符串的字符数是:: 9
```

在上面这个范例中，我们定义了两个同名变量 uname，但它们有着不同的数据类型。第一次 uname 的数据类型是 string 也就是字符串，而第二次则变成了 i32 是一个整数了。

> len() 函数返回字符串中字符的个数。

## 6.6 不能出现同名常量

常量与变量的另一个不同点是： 常量不能被屏蔽/遮挡，也不能被重复定义。

也就是说不存在内层/后面作用域定义的常量屏蔽外层/前面定义的同名常量。

这是因为 Rust 语言中不允许有同名的常量。

### 6.6.1 范例

我们将上面的范例改造下，使用 const 关键字来定义同名的常量

```
fn main() {
   const NAME:&str = "Mohtashim";
   const NAME:usize = NAME.len(); 
   //Error : `NAME` already defined
   println!("改变 name 常量的类型: {}",NAME);
}
```
编译运行以上 Rust 代码，输出结果如下
```
error[E0428]: the name `NAME` is defined multiple times
 --> src/main.rs:3:5
  |
2 |     const NAME:&str = "Mohtashim";
  |     ------------------------------ previous definition of the value `NAME` here
3 |     const NAME:usize = NAME.len();
  |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ `NAME` redefined here
  |
  = note: `NAME` must be defined only once in the value namespace of this block
```
