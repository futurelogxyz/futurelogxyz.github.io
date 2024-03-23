---
title: Mojo 编程手册
date: 2023-10-01
authors:
  - name: wallezen
    link: https://github.com/wallezen
    image: https://futurelog-1251943639.cos.accelerate.myqcloud.com/img/wallezen-avatar.jpg
excludeSearch: false
toc: true
---

![](https://futurelog.xyz/img/imgMojo-750x422_clipdrop-enhance-20231107-1400x600.webp)

> Mojo 编程语言的目标是在像 Python 一样易于使用的同时拥有类似 C++ 和 Rust 的性能。并且，还能利用整个 Python 生态的能力。
> 
> *- wallezen*

*参考自 Mojo 官方文档：[Mojo🔥 programming manual](https://docs.modular.com/mojo/programming-manual.html)*

Mojo 编程语言的目标是在像 Python 一样易于使用的同时拥有类似 C++ 和 Rust 的性能。并且，还能利用整个 Python 生态的能力。

为实现这些特性，Mojo 利用了下一代的编译器技术，如：集成缓存、多线程 和 云分发。此外，Mojo 的 autotuning 和 编译时元编程（compile-time metaprogramming）特性，使得编写的代码能够能够移植到各类硬件上。

更重要的是， Mojo 被设计为 Python 的超集，通过集成整个 Python 生态的能力，让开发人员继续使用熟悉的工具和库，在保留 Python 动态特性的同时增加了系统编程能力。这些新增加的系统编程能力，让开发人员也能够使用 Mojo 编写那些目前需要依赖 C/C++/Rust/CUDA 等才能实现的高性能的代码和库。通过结合动态编程语言和系统编程语言的优势，Mojo 提供了一种统一的编程模型，对新手友好，并能扩展应用到从底层加速器（accelerators）到应用开发、脚本开发等诸多场景。

本手册并不是完整的 Mojo 编程语言指南，我们假设读者已经熟悉 Python 语言和系统编程的相关概念，将介绍 Mojo 编程语言相关的基础知识。当前，Mojo 编程语言还处在开发中，本手册的主要目标读者是那些拥有系统编程经验的开发人员。随着 Mojo 编程语言的发展和普及，我们希望 Mojo 对所有人（甚至初学者）都友好且易于使用。

## 1. Mojo 编译器
和运行 Python 代码一样，我们也可以在命令行中运行 Mojo 代码：
```
$ cat hello.mojo
(out)def main():
(out)    print("hello world")
(out)    for x in range(9, 0, -3):
(out)        print(x)
(out)

$ mojo hello.mojo
(out)hello world
(out)9
(out)6
(out)3
(out)
```
*文件名后缀可以是 `.mojo` 或 `.🔥`。*

Mojo SDK 预计在 2023 年 09 月初开放下载，可以在官网 [登记预约](https://www.modular.com/mojo)。


## 2. 系统编程基础支持
Python 对系统编程的支持需要依赖于 C。在 Mojo 中，我们希望提供一个统一的编程模型。本节将详细介绍 Mojo 的主要组件和功能特性，并通过示例描述如何使用它们。

### 2.1 `let` 和 `var` 变量声明
与 Python 类似，在 Mojo 的 `def` 函数内部，我们也可以直接为一个变量名赋值，从而隐式创建一个函数作用域内的变量。这种动态且简单的代码编写方式对于系统编程存在以下问题：
1. 系统编程开发人员为保证类型安全和性能，通常希望值是不可变的；
2. 如果输错了变量名称，他们也希望得到相应的错误提示。

为了解决这些问题，Mojo 引入了 `let` 和 `var` 关键字，用于显式声明变量。

**`let` 表示不可变，`var` 表示可变。**

```python {hl_lines=[3,4]}
def your_function(a, b):
    let c = a
    # Uncomment to see an error:
    # c = b  # error: c is immutable

    if c != b:
        let d = b
        print(d)

your_function(2, 3)
```

`let` 和 `var` 声明也支持类型说明符，后期初始化(late initialization) 和 变量遮蔽(variable shadowing)。
```python
def your_function():
    let x: Int = 42  # type annotation
    let y: Float64 = 17.0

    var one: Float32 = 1.0

    let z: Float32
    if x != 0:
        one = 2.0  # variable shadowing
        z = one  # late initialization
    else:
        z = foo()
    print(z)

def foo() -> Float32:
    return 3.14

your_function()
```

{{< callout type="info" >}}
注意：在 `def` 函数中， `let` 和 `var` 完全是可选的，但在 `fn` 函数中，它们是必须的。另外，在 REPL（Read Eval Print Loop，交互式解释器）环境中， 对位于函数和结构体之外的全局变量声明，不需要 `let` 和 `var`，也不需要类型说明符，这与 Python 在 REPL 中的行为是一致的。
{{< /callout >}}

### 2.2 `struct` 结构体类型
Mojo 基于 [MLIR](https://mlir.llvm.org/) 和 [LLVM](https://llvm.org/) 这两个编译器基础设施，它们同时也是很多其他编程语言的基石。现代系统编程语言的一个重要特征是能够在复杂的低级操作之上构建高级安全的抽象，并且不会造成任何性能损失。在 Mojo 中，我们通过 `struct` 结构体类型来实现这一目标。

Mojo 的 `struct` 类似于 Python 中的 `class`，它们都支持 方法、属性、运算符重载、元编程装饰器 等。但也有如下区别：
- Python 的 `class` 是动态的。它允许动态调度（dynamic dispatch），monkey-patching，以及在运行时动态绑定示例属性 等。
- Mojo 的 `struct` 是静态的。它在编译时绑定，不能在运行时添加方法等，牺牲灵活性换取性能，安全且易于使用。

```python
struct MyPair:
    var first: Int
    var second: Int

    # We use 'fn' instead of 'def' here - we'll explain that soon
    fn __init__(inout self, first: Int, second: Int):
        self.first = first
        self.second = second

    fn __lt__(self, rhs: MyPair) -> Bool:
        return self.first < rhs.first or
              (self.first == rhs.first and
               self.second < rhs.second)
```
从语法上讲，上述的 `struct` 定义和 Python 中的 `class` 最大的区别是所有属性都必须使用 `var` 或 `let` 声明。

在 Mojo 中，`struct` 的结构和内容是预先设置好的，是静态的，在程序运行时无法更改。这样做的好处是，编译器可以在编译时就知道结构体的大小和布局，确切地知道在哪里可以找到结构体的信息以及如何使用它，无需任何额外的步骤或延迟，使得 Mojo 可以更快地运行代码。

Mojo 中的标准类型，如 `Int`、`Bool`、`String` 和 `Tuple` 等都是使用 `struct` 结构体创建的。

### 2.3 `Int` 与 `int`
在 Mojo 中我们使用的 `Int` 类型是一个结构体，它与 Python 中的 `int` 不同。这种差异是我们特意设计的。在 Python 中， `int` 类型可以处理非常大的数字，并且具有一些额外的功能，例如检查两个数字是否是同一个对象。但这带来了一些额外的负担，可能会减慢速度。 Mojo 的 `Int` 设计简单、快速，并针对您的计算机硬件进行了调整，以便快速处理。

我们如此设计，主要有两个原因：
1. 我们希望为需要与计算机硬件密切合作的程序员（系统程序员）提供一种透明且可靠的与硬件交互的方式。我们不想依靠花哨的技巧（比如 JIT 编译器）来让事情变得更快；
2. 我们希望 Mojo 能够与 Python 很好地配合，而不会引起任何问题。通过使用不同的名称（`Int` 而不是 `int`），我们可以在 Mojo 中保留这两种类型，而无需更改 Python `int` 的工作方式。

### 2.4 强类型检查
尽管您仍然可以像在 Python 中一样使用灵活的类型，但 Mojo 允许使用严格的类型检查。类型检查可以使您的代码更可预测、更易于管理且更安全。

Mojo 主要是通过 `struct` 来应用强类型检查的。如以下关于 `MyPair` 结构体的示例：
```python {hl_lines=[3,4]}
def pair_test() -> Bool:
    let p = MyPair(1, 2)
    # Uncomment to see an error:
    # return p < 4 # gives a compile time error
    return True
```
在上述示例中，`p` 是一个 `MyPair` 类型的变量，它不能与 `Int` 类型的 `4` 相比较，因为 `MyPair` 类型没有定义 `<` 运算符。如果取消注释 `return p < 4` 这一行，编译器将会报错。

在系统编程语言中，强类型检查是常见且熟悉的惯例。但 Python 并非如此，虽然 Python 可以使用 [MyPy](https://mypy.readthedocs.io/en/stable/) 类型标注能力来进行类型检查，但这只是静态分析提示，并不是编译器强制执行的，我们甚至可以忽略这类错误提示继续运行代码。在 Mojo 中，通过将类型绑定到特定声明，Mojo 可以处理经典类型注释提示和强类型规范，而不会破坏兼容性。

类型检查并不是强类型的唯一用途。由于知道准确的类型，因此我们可以根据这些类型优化代码，在寄存器中传递值，并在参数传递和其他低级细节处理方面与 C 一样高效。这是 Mojo 为系统程序员提供安全性和可预测性保证的基础。

### 2.5 函数重载和方法重载
函数重载和方法重载在 C++ 和 Java 等编程语言中是常见的特性。得益于上文提到的类型安全特性，Mojo 也为函数重载和方法重载提供了全面的支持。

所谓 *重载* 是指在同一个作用域中，可以定义多个同名的函数或方法，但它们的参数类型或参数个数不同。在调用时，编译器会根据参数类型或参数个数的不同，自动选择合适的函数或方法。

以下是一个函数重载的示例：
```python
struct Complex:
    var re: Float32
    var im: Float32

    fn __init__(inout self, x: Float32):
        """Construct a complex number given a real number."""
        self.re = x
        self.im = 0.0

    fn __init__(inout self, r: Float32, i: Float32):
        """Construct a complex number given its real and imaginary components."""
        self.re = r
        self.im = i
```

{{< callout type="error" >}}
注意：Mojo 不支持仅在结果类型上重载，并且不使用结果类型或上下文类型信息进行类型推断，从而使事情保持简单、快速和可预测。同样，如果您的参数名称不带类型定义，则该函数的行为就像具有动态类型的 Python 一样。一旦定义了单个参数类型，Mojo 将查找重载候选者并解析函数调用。
{{< /callout >}}

### 2.6 `fn` 函数
`def` 函数的定义是动态的，灵活的，并且与 Python 兼容（参数可变，局部变量在首次使用时隐式声明 等）。这对于系统编程来说不是一个好的特性，因此，Mojo 引入了 `fn` 函数，类似于严格模式的 `def`。

`fn` 函数有更多的规范限制，主要有以下几点：
1. 函数内的参数(argument)默认是不可变的，包括传入参数；
2. 所有参数都需要明确指定类型，缺少返回类型说明符会被解释为返回 None 而不是未知的返回类型；
3. 所有局部变量都需要使用 `let` 或 `var` 显式声明；
4. 触发异常需要显式使用 `raise` 关键字；

{{< callout type="info" >}}
不同团队的编程模式会有很大差异，这种严格程度并不适合所有人。我们希望习惯 C++ 并已在 Python 中使用 MyPy 样式类型注释的人们更喜欢使用 fn ，但更高级别的程序员和 ML 研究人员会继续使用 def . Mojo 允许您自由混合 def 和 fn 声明，例如使用一种方法实现某些方法，使用另一种方法实现另一些方法，并允许每个团队或程序员决定什么最适合他们的用例。
{{< /callout >}}

### 2.7 `__copyinit__` 方法 和 `__moveinit__` 方法
Mojo 允许自定义构造函数（`__init__` 方法），析构函数（`__del__` 方法），拷贝构造函数（`__copyinit__` 方法）和移动构造函数（`__moveinit__` 方法）。这些特性在进行实现底层系统编程时非常有用，如手动内存管理等。

以下是一个动态字符串类型，它在构造时为字符串数据分配内存，并在销毁实例时释放内存：
```python {hl_lines=[34,35]}
from memory.unsafe import Pointer

struct HeapArray:
    var data: Pointer[Int]
    var size: Int
    var cap: Int

    fn __init__(inout self):
        self.cap = 16
        self.size = 0
        self.data = Pointer[Int].alloc(self.cap)

    fn __init__(inout self, size: Int, val: Int):
        self.cap = size * 2
        self.size = size
        self.data = Pointer[Int].alloc(self.cap)
        for i in range(self.size):
            self.data.store(i, val)

    fn __del__(owned self):
        self.data.free()

    fn dump(self):
        print_no_newline("[")
        for i in range(self.size):
            if i > 0:
                print_no_newline(", ")
            print_no_newline(self.data.load(i))
        print("]")


var a = HeapArray(3, 1)
a.dump()   # Should print [1, 1, 1]
# Uncomment to see an error:
# var b = a  # ERROR: Vector doesn't implement __copyinit__

var b = HeapArray(4, 2)
b.dump()   # Should print [2, 2, 2, 2]
a.dump()   # Should print [1, 1, 1]
```

在上述例子中，如果您尝试使用 `=` 运算符复制 HeapArray 的实例，Mojo 不允许我们复制： HeapArray 包含 Pointer 的实例（相当于低级 C 指针），Mojo 不知道它指向什么类型的数据或如何复制它。

为了实现数组的可复制，我们必须实现 `__copyinit__` 方法，它将在复制 HeapArray 实例时被调用。以下是实现 `__copyinit__` 方法的示例：
```python {hl_lines=[18,19,20,21,22,23,38,39]}
struct HeapArray:
    var data: Pointer[Int]
    var size: Int
    var cap: Int

    fn __init__(inout self):
        self.cap = 16
        self.size = 0
        self.data = Pointer[Int].alloc(self.cap)

    fn __init__(inout self, size: Int, val: Int):
        self.cap = size * 2
        self.size = size
        self.data = Pointer[Int].alloc(self.cap)
        for i in range(self.size):
            self.data.store(i, val)

    fn __copyinit__(inout self, other: Self):
        self.cap = other.cap
        self.size = other.size
        self.data = Pointer[Int].alloc(self.cap)
        for i in range(self.size):
            self.data.store(i, other.data.load(i))
            
    fn __del__(owned self):
        self.data.free()

    fn dump(self):
        print_no_newline("[")
        for i in range(self.size):
            if i > 0:
                print_no_newline(", ")
            print_no_newline(self.data.load(i))
        print("]")

var a = HeapArray(3, 1)
a.dump()   # Should print [1, 1, 1]
# This is no longer an error:
var b = a

b.dump()   # Should print [1, 1, 1]
a.dump()   # Should print [1, 1, 1]
```
上述代码可以正常工作，并且 `var b = a` 会生成不同的数组实例 `b`，它具有自己的生命周期和数据：

Mojo 还支持 `__moveinit__` 方法，该方法允许 Rust 风格的移动（在生命周期结束时获取一个值）和 C++ 风格的移动（其中值的内容被删除，但析构函数仍然运行），并允许定义自定义移动逻辑。详细信息请参阅下文的 [6. 生命周期]({{< ref "#6. 生命周期：值的诞生、存在与消亡" >}}) 部分。

## 3. 参数传递与所有权
让我们回顾一下有关 Python 和 Mojo 如何传递函数参数（argument）的一些细节：
- 在 Python 中， `def` 函数的所有参数都是 **引用语义**。这意味着该函数可以修改传递给它的可变对象，并且这些更改在函数外部可见。
- 在 Mojo 中， 默认情况下，`def` 函数的所有参数都是 **值语义**。与 Python 相比，这是一个重要的区别：Mojo `def` 函数接收所有参数的副本 - 它可以修改函数内部的参数，但更改在函数外部不可见。
- 在 Mojo 中，默认情况下，`fn` 函数的所有值都是不可变引用。这意味着该函数可以读取原始对象（它不是副本），但它根本无法修改该对象。这种在 Mojo fn 中传递不可变参数的约定称为 **“借用（borrowing）”**。

### 3.1 参数传递约定的重要性
TODO...

### 3.2 不可变参数(`borrowed` arguments)
`borrowed` 参数是对函数接收的参数的不可变引用，因此，被调用函数具有对该对象的完全读取和执行访问权限，但无法修改它（调用者仍然拥有该对象的独占“所有权”）。

我们来看看以下示例：
```python
# Don't worry about this code yet. It's just needed for the function below.
# It's a type so expensive to copy around so it does not have a
# __copyinit__ method.
struct SomethingBig:
    var id_number: Int
    var huge: HeapArray
    fn __init__(inout self, id: Int):
        self.huge = HeapArray(1000, 0)
        self.id_number = id

    # self is passed by-reference for mutation as described above.
    fn set_id(inout self, number: Int):
        self.id_number = number

    # Arguments like self are passed as borrowed by default.
    fn print_id(self):  # Same as: fn print_id(borrowed self):
        print(self.id_number)


fn use_something_big(borrowed a: SomethingBig, b: SomethingBig):
    """'a' and 'b' are both immutable, because 'borrowed' is the default."""
    a.print_id()
    b.print_id()

let a = SomethingBig(10)
let b = SomethingBig(20)
use_something_big(a, b)
```

将 SomethingBig 的实例传递给函数时，需要传递引用，因为 SomethingBig 无法复制（它没有 __copyinit__ 方法）。并且，如上文所述，`fn` 函数的默认参数约定就是 `borrowed`，当然您也可以使用 `borrowed` 关键字显式定义。

Mojo 的这种参数借用约定在某些方面类似于 C++ 中通过 `const&` 传递参数，这避免了值的拷贝并禁止被修改。然而，Mojo 的参数借用约定在两个重要方面与 C++ 中的 `const&` 不同：
1. Mojo 编译器实现了一个借用检查器（类似于 Rust），该检查器可以防止代码在存在未完成的不可变引用时动态形成对某个值的可变引用，并且它可以防止对同一值有多个可变引用。您可以进行多次借用（如上面对 use_something_big 的调用一样），但不能通过可变引用传递某些内容并同时借用。
2. 像 `Int` 、 `Float` 和 `SIMD` 这样的小值直接在机器寄存器中传递，而不是通过额外的间接传递（因为它们使用了 `@register_passable` 装饰器）。与 C++ 和 Rust 等语言相比，这是一个显着的性能增强。

与 Rust 类似，Mojo 的借用检查器强制执行不变量的排他性。 Rust 和 Mojo 之间的主要区别在于，Mojo 不需要调用方有一个印记来传递借用。此外，Mojo 在传递小值时效率更高，而 Rust 默认移动值而不是通过借用传递它们。这些策略和语法决策使 Mojo 能够提供更易于使用的编程模型。

### 3.3 可变参数(`inout` arguments)
如果您定义 `fn` 函数并希望参数可变，则必须使用 `inout` 关键字将参数声明为可变。

{{< callout type="info" >}}
Tips：当您看到 inout 时，这意味着对函数内部参数所做的任何更改在函数外部都是可见的。
{{< /callout >}}

以下是一个使用 `inout` 参数的示例，其中 `__iadd__` 函数（实现就地添加操作，例如 x += 2 ）尝试修改 self ：
```python {hl_lines=[14,15,16,20,25,26]}
struct MyInt:
    var value: Int
    
    fn __init__(inout self, v: Int):
        self.value = v
  
    fn __copyinit__(inout self, other: MyInt):
        self.value = other.value
        
    # self and rhs are both immutable in __add__.
    fn __add__(self, rhs: MyInt) -> MyInt:
        return MyInt(self.value + rhs.value)

    # delete `inout` to see the error:
    fn __iadd__(inout self, rhs: Int):
        self = self + rhs


var x: MyInt = 42
# if delete `inout` before `self` argument of `__iadd__` method, will get an error
x += 1
print(x.value) # prints 43 as expected

# However...
let y = x
# Uncomment to see the error:
# y += 1       # ERROR: Cannot mutate 'let' value
```

{{< callout type="info" >}}
注意，我们不将此参数称为“通过引用”传递。尽管 `inout` 约定在概念上是相同的，但我们不将其称为按引用传递，因为实现实际上可能使用指针传递值。
{{< /callout >}}


### 3.4 所有权转移(`owned` 和 `^`)
`owned` 参数约定用于想要获得值的独占所有权的函数，并且通常与后缀 `^` 运算符一起使用。

以下是一个使用 `owned` 和 `^` 进行所有权转移的示例：
```python {hl_lines=[15,26,28,29]}
# This is not really a unique pointer, we just model its behavior here
# to serve the examples below.
struct UniquePointer:
    var ptr: Int
    
    fn __init__(inout self, ptr: Int):
        self.ptr = ptr
    
    fn __moveinit__(inout self, owned existing: Self):
        self.ptr = existing.ptr
        
    fn __del__(owned self):
        self.ptr = 0

fn take_ptr(owned p: UniquePointer):
    print("take_ptr")
    print(p.ptr)

fn use_ptr(borrowed p: UniquePointer):
    print("use_ptr")
    print(p.ptr)
    
fn work_with_unique_ptrs():
    let p = UniquePointer(100)
    use_ptr(p)    # Pass to borrowing function.
    take_ptr(p^)  # Pass ownership of the `p` value to another function.

    # Uncomment to see an error:
    # use_ptr(p) # ERROR: p is no longer valid here!

work_with_unique_ptrs()
```

`^` 运算符结束值绑定的生命周期，并将值所有权转移给其他对象（在上述示例中，所有权转移给 `take_ptr()` 函数）。为了支持这一点，`take_ptr()` 函数定义为采用 `owned` 参数。
在上述示例中,如果第二次调用 `use_ptr()` ，则会报错，因为 `p` 值所有权已经转移到 `take_ptr()` 函数。


## 4. 集成 Python
在 Mojo 中使用现在的 Python 模块非常简单，您可以将任何 Python 模块导入到 Mojo 程序中。

### 4.1 导入 Python 模块
要在 Mojo 中导入 Python 模块，只需使用模块名称调用 Python.import_module() 即可：

```python {hl_lines=[1,4]}
from python import Python

# This is equivalent to Python's `import numpy as np`
let np = Python.import_module("numpy")

# Now use numpy as if writing in Python
array = np.array([1, 2, 3])
print(array)
```

{{< callout type="info" >}}
注意：目前无法导入单个成员（例如单个 Python 类或函数），您必须导入整个 Python 模块，然后通过模块名称访问成员。
{{< /callout >}}

### 4.2 Python 中使用 Mojo 类型
Mojo 的原始类型（如 列表、元组、整数、浮点数、布尔值和字符串 等）可以隐式转换为 Python 对象。

```python
def type_printer(my_list, my_tuple, my_int, my_string, my_float):
    print(type(my_list))
    print(type(my_tuple))
    print(type(my_int))
    print(type(my_string))
    print(type(my_float))

type_printer([0, 3], (False, True), 4, "orange", 3.4)
```

Mojo 目前还不支持字典类型，因此还无法从 Mojo 字典创建 Python 字典。不过，您可以在 Mojo 中使用 Python 字典！要创建 Python 字典，请使用 `dict` 方法：

```python {hl_lines=[4]}
from python import Python
from python.object import PythonObject

var dictionary = Python.dict()
dictionary["fruit"] = "apple"
dictionary["starch"] = "potato"

var keys: PythonObject = ["fruit", "starch", "protein"]
var N: Int = keys.__len__().__index__()
print(N, "items")

for i in range(N):
    if Python.is_type(dictionary.get(keys[i]), Python.none()):
        print(keys[i], "is not in dictionary")
    else:
        print(keys[i], "is included")
```

### 4.3 导入本地 Python 模块
如果您想在 Mojo 中使用一些本地 Python 代码，只需将目录添加到 Python 路径，然后导入模块即可。

例如，假设您有一个名为 `mypython.py` 的 Python 文件：
```python
import numpy as np

def my_algorithm(a, b):
    array_a = np.random.rand(a, a)
    return array_a + b
```

导入它并在 Mojo 文件中使用它的方法：
```python {hl_lines=[3,4]}
from python import Python

Python.add_to_path("path/to/module")
let mypython = Python.import_module("mypython")

let c = mypython.my_algorithm(2, 3)
print(c)
```

{{< callout type="info" >}}
Tips: 在 Mojo 中使用 Python 时无需担心内存管理问题,因为 Mojo 从一开始就是为 Python 设计的。
{{< /callout >}}


## 5. 参数化：编译时的元编程
TODO...

## 6. 生命周期：值的诞生、存在与消亡
TODO...

## 7. 析构函数
TODO...

## 8. lifetimes
TODO...

## 9. Type traits
TODO...

## 10. Advanced/Obscure Mojo features
TODO...


