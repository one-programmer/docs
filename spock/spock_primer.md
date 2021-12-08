## Spock入门

Peter Niederwieser，Leonard Brünings，Spock 框架团队 - 2.0版

---

本章假设您具有 Groovy 和单元测试的基本知识。如果您是 Java 开发人员但还没有听说过 Groovy，请不要担心 - Groovy 会让您觉得很熟悉！
事实上，Groovy 的主要设计目标之一是成为与 Java 一起的脚本语言。因此，只要您愿意，只需跟随并查阅 [Groovy 文档](http://groovy-lang.org/documentation.html) 即可。

本章的目标是教您足够的 Spock 来编写真实世界的 Spock 规范，并激发您对更多内容的兴趣。

要了解有关 Groovy 的更多信息，请访问[http://groovy-lang.org/]()。

要了解有关单元测试的更多信息，请访问[http://en.wikipedia.org/wiki/Unit_testing]()。

### 术语

让我们从几个定义开始：Spock 允许您编写 [规范](https://en.wikipedia.org/wiki/Specification_by_example) 来描述感兴趣的系统展示的预期特性（属性、方面）。
感兴趣的系统可以是单个类和整个应用程序之间的任何东西，也称为 `规范下的系统` 或 `SUS`。功能的描述从 `SUS` 及其合作者的特定快照开始；此快照称为功能的`装置`。

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
存储在实例字段中的对象在功能方法之间不共享。相反，每个功能方法都有自己的对象。这有助于将功能方法彼此隔离，这通常是一个理想的目标。

```groovy
@Shared res = new VeryExpensiveResource()
```
有时您需要在功能方法之间共享一个对象。例如，创建对象可能非常昂贵，或者您可能希望您的功能方法之间有交互。
实现这点，需要声明一个`@Shared`字段。同样，最好在声明时初始化字段。（从语义上讲，这相当于在`setupSpec()`方法的最开始初始化字段。）

```groovy
static final PI = 3.141592654
```
静态字段应该只用于常量。否则共享字段更可取，因为它们在共享方面的语义更明确。

### 固定方法

```groovy
def setupSpec() {}    // 运行一次 - 在第一个功能方法之前
def setup() {}        // 在每个功能方法之前运行
def cleanup() {}      // 在每个功能方法之后运行
def cleanupSpec() {}  // 运行一次 - 在最后一个功能方法之后
```

固定方法负责设置和清理运行功能方法的环境。通常，为每个功能方法使用新的特定设置是个好主意，这就是`setup()`和`cleanup()`方法的用途。

所有的固定方法都是可选的。

有时，功能方法共享一个设置是有意义的，这是通过将共享字段与`setupSpec()`和`cleanupSpec()`方法一起使用来实现的。
请注意，`setupSpec()`和`cleanupSpec()` *不可以*引用实例字段，除非它们用`@Shared`标注了。

#### 调用顺序

如果在规范的子类中覆盖了固定方法`setup()`，则超类的`setup()`将在子类之前运行。`cleanup()`以相反的顺序工作，即子类的`cleanup()`将在超类`cleanup()`之前执行。
`setupSpec()`和`cleanupSpec()`以同样的方式。无需显式调用super.setup()或super.cleanup()Spock 会自动查找并执行继承层次结构中所有级别的夹具方法。



