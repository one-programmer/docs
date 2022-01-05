## Spock入门

Peter Niederwieser，Leonard Brünings，Spock 框架团队 - 2.0版

---

本章假设您具有 Groovy 和单元测试的基本知识。如果您是 Java 开发人员但还没有听说过 Groovy，请不要担心 - Groovy 会让您觉得很熟悉！
事实上，Groovy 的主要设计目标之一是成为与 Java 一起的脚本语言。因此，只要您愿意，只需跟随并查阅 [Groovy 文档](http://groovy-lang.org/documentation.html) 即可。

本章的目标是教您足够的 Spock 来编写真实世界的 Spock 规范，并激发您对更多内容的兴趣。

要了解有关 Groovy 的更多信息，请访问[http://groovy-lang.org/]()。

要了解有关单元测试的更多信息，请访问[http://en.wikipedia.org/wiki/Unit_testing]()。

### 术语

让我们从几个定义开始：Spock 允许您编写 [规范](https://en.wikipedia.org/wiki/Specification_by_example) 来描述感兴趣的系统展示的预期特征（属性、方面）。
感兴趣的系统可以是单个类和整个应用程序之间的任何东西，也称为 `规范下的系统` 或 `SUS`。特征的描述从 `SUS` 及其合作者的特定快照开始；此快照称为特征的`装置`。

以下章节将带您了解所有可以组成 Spock 规范的构建块。一个典型的规范仅会使用到其中的一个子集。

### 导入

```groovy
import spock.lang.*
```
包spock.lang包含用于编写规范的最重要的类。

### 规范

```groovy
class MyFirstSpecification extends Specification {
  // fields
  // fixture methods
  // feature methods
  // helper methods
}
```
规范表示为一个继承自spock.lang.Specification类的Groovy类。
规范的名称通常与规范所描述的系统或系统操作有关。例如，`CustomerSpec`、 `H264VideoPlayback`和`ASpaceshipAttackedFromTwoSides`都是规范的合理名称。

类`Specification`包含许多用于编写规范的有用方法。此外，它指定 JUnit 使用 Spock的JUnit运行器 `Sputnik` 运行规范。
多亏了 Sputnik，大多数现代 Java IDE 和构建工具都可以运行 Spock 规范。

### 字段

```groovy
def obj = new ClassUnderSpecification()
def coll = new Collaborator()
```
实例字段是存储属于规范固定组件的对象的好地方。一种最佳实践是在声明时初始化它们。（从语义上讲，这相当于在`setup()`方法的最开始初始化它们。）
存储在实例字段中的对象在特征方法之间不共享。相反，每个特征方法都有自己的对象。这有助于将特征方法彼此隔离，这通常是一个理想的目标。

```groovy
@Shared res = new VeryExpensiveResource()
```
有时您需要在特征方法之间共享一个对象。例如，创建对象可能非常昂贵，或者您可能希望您的特征方法之间有交互。
实现这点，需要声明一个`@Shared`字段。同样，最好在声明时初始化字段。（从语义上讲，这相当于在`setupSpec()`方法的最开始初始化字段。）

```groovy
static final PI = 3.141592654
```
静态字段应该只用于常量。否则共享字段更可取，因为它们在共享方面的语义更明确。

### 固定方法

```groovy
def setupSpec() {}    // 运行一次 - 在第一个特征方法之前
def setup() {}        // 在每个特征方法之前运行
def cleanup() {}      // 在每个特征方法之后运行
def cleanupSpec() {}  // 运行一次 - 在最后一个特征方法之后
```

固定方法负责设置和清理运行特征方法的环境。通常，为每个特征方法使用新的特定设置是个好主意，这就是`setup()`和`cleanup()`方法的用途。

所有的固定方法都是可选的。

有时，特征方法共享一个设置是有意义的，这是通过将共享字段与`setupSpec()`和`cleanupSpec()`方法一起使用来实现的。
请注意，`setupSpec()`和`cleanupSpec()` *不可以*引用实例字段，除非它们用`@Shared`标注了。

#### 调用顺序

如果在规范的子类中覆盖了固定方法`setup()`，则超类的`setup()`将在子类之前运行。`cleanup()`以相反的顺序工作，即子类的`cleanup()`将在超类`cleanup()`之前执行。
`setupSpec()`和`cleanupSpec()`以同样的方式。无需显式调用super.setup()或super.cleanup()Spock 会自动查找并执行继承层次结构中所有级别的夹具方法。

1. `super.setupSpec`
2. `sub.setupSpec`
3. `super.setup`
4. `sub.setup`
5. feature method
6. `sub.cleanup`
7. `super.cleanup`
8. `sub.cleanupSpec`
9. `super.cleanupSpec`

### 特征方法

```groovy
def "pushing an element on the stack"() {
  // blocks go here
}
```

特征方法是规范的核心。它们描述了在规范下的系统中你预期得到的特征（属性、切面）。按照惯例，特征方法以字符串的字面量来命名。
尝试为您的特征方法选择好的名称，并可以随意使用您喜欢的任何字符！

从概念上讲，特征方法由四个阶段组成：

1. 设置特征的装置

2. 对规范下的系统提供一个*触发*

3. 描述系统预期的*响应*

4. 清理特征的装置

第一阶段和最后阶段是可选的，但触发和响应阶段必须存在（交互特征方法除外），并且可能发生多次。

### 块

Spock 内置了支持来实现特征方法的各个概念阶段。为此，特征方法被构造成所谓的*块*。
块以一个标签开始，并延伸到下一个块的开头，或者方法的结尾。有六个种类的块：`given`，`when`，`then`，`expect`，`cleanup`，和`where`块。
方法开头和第一个显式的块之间的任何语句都属于隐式`given`块。

一个特征方法必须至少有一个显式（即标记）的块 —— 事实上，显式块的存在才使得方法成为特征方法。块将方法划分为不同的部分，并且不能嵌套。

右图显示了块是如何映射到一个特征方法的概念阶段的。其中`where`区块有一个特殊的作用，很快就会揭晓。但首先，让我们仔细看看其他块。

![块和阶段](https://spockframework.org/spock/docs/2.0/images/Blocks2Phases.png)

#### Given 块

```groovy
given:
def stack = new Stack()
def elem = "push me"
```

这个`given`块是您为所描述的特征进行各种设置工作的地方。前面不能有其他块，也不能重复。一个`given`块不具有任何特殊的语义。
这个`given:`标签是可选的可以省略，这样就是隐式的`given`块。最初，别名`setup:`是首选的块名称，但使用`given:`可以让特征方法的描述更加易读（参见[规范即文档](#规范即文档) ）。

#### When 和 then 块

```groovy
when:   // stimulus
then:   // response
```

when和then块总是一起出现。他们描述了一种触发以及预期的响应。其中`when`块可以包含任意代码，`then`块仅限于*条件*、*异常条件*、*交互*和变量定义。
一个特征方法可能包含多对`when-then`块。

##### 条件

条件描述了一个预期的状态，很像 JUnit 的断言。然而，条件被写成简单的布尔表达式，消除了对断言 API 的依赖。
（更准确地说，一个条件也可能产生一个非布尔值，然后会按照 Groovy truth 规则进行求值。）让我们看看一些实际的条件：

```groovy
when:
stack.push(elem)

then:
!stack.empty
stack.size() == 1
stack.peek() == elem
```
> 提示：尽量减少每个特征方法的条件数量。一到五个条件是一个好的指导方针。
> 如果您有更多，问问自己是否一次指定了多个不相关的特征。如果答案是肯定的，那么将特征方法分解为几个较小的方法。
> 如果您的条件只有值不同，请考虑使用[数据表](data_driven_testing.md#数据表)。

如果条件被违反，Spock 会提供什么样的反馈？让我们尝试将第二个条件更改为 `stack.size() == 2`。这是我们得到的：

```groovy
Condition not satisfied:

stack.size() == 2
|     |      |
|     1      false
[push me]
```

正如您看到的，Spock 捕获了在条件求值过程中产生的所有值，并以方便理解的形式展现出来。不错吧？

##### 隐式和显式条件

条件是`then`块和`expect`块的基本组成部分。除了对`void`方法和表达式的调用被归类为交互外，这些块中的所有顶级表达式都被隐式地视为条件。
要在其他地方使用条件，您需要使用 Groovy 的 assert 关键字来指定它们：

```groovy
def setup() {
  stack = new Stack()
  assert stack.empty
}
```

如果显式条件被违反，它将产生与隐式条件相同的好用的调试消息。

##### 异常条件

异常条件用于描述`when`块应该抛出异常。它们使用`thrown()`方法来定义，传递预期的异常类型。
例如，要描述从一个空栈中弹出应该抛出一个`EmptyStackException`，您可以这样写：
```groovy
when:
stack.pop()

then:
thrown(EmptyStackException)
stack.empty
```

正如您所看到的，异常条件之后可能会出现其他条件（甚至是其他块）。这对于指定异常的预期内容特别有用。
要访问异常，首先将它绑定到一个变量：
```groovy
when:
stack.pop()

then:
def e = thrown(EmptyStackException)
e.cause == null
```

或者，您可以使用上述语法的一种简单变体：
```groovy
when:
stack.pop()

then:
EmptyStackException e = thrown()
e.cause == null
```

这种语法有两个小优点: 首先，异常变量是强类型的，这使 IDE 更容易提供代码补全。其次，这个条件读起来更像是一个句子（“然后抛出一个 EmptyStackException”）。
请注意，如果没有将异常类型传递给该`thrown()`方法，则会从左侧的变量类型推断出异常类型。

有时候我们需要表达一个异常不应该被抛出。例如，让我们尝试来表示`HashMap`应该接受`null`键：
```groovy
def "HashMap accepts null key"() {
  setup:
  def map = new HashMap()
  map.put(null, "elem")
}
```

这是可行的，但并没有展示代码的意图。是不是某人还没实现完这个方法就撒手不管了？毕竟，条件在哪里呢？幸运的是，我们可以做得更好：
```groovy
def "HashMap accepts null key"() {
  given:
  def map = new HashMap()

  when:
  map.put(null, "elem")

  then:
  notThrown(NullPointerException)
}
```

通过使用`notThrown()`，我们明确表示了不应该抛出`NullPointerException`。
（根据`Map.put()`的约定，对于不支持`null`键的map，这是正确的做法。）但是，如果抛出任何其他异常，该方法也会失败。


### 规范即文档