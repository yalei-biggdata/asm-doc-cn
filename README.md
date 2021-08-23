# asm-doc-cn
asm的中文翻译文档，英文原版地址https://asm.ow2.io/asm4-guide.pdf

# 2.第二章 类
本章将解释如何使用核心 ASM API 生成和转换已编译的 Java 类。 首先会介绍已编译的类的组成，然后介绍相应的 ASM 接口、组件和工具来生成和转换它们，并附有许多说明性示例。 方法、注解和泛型的内容在接下来的章节中解释。
## 2.1 结构
### 2.1.1 概览
编译类的整体结构非常简单。 实际上，与本机编译的应用程序不同，编译后的类保留了源代码中的结构信息和几乎所有符号。 事实上，编译后的类包含：
- 描述修饰符（例如公共或私有）、名称、超类、接口和类的注释的部分。
- 此类中声明的每个字段一节。 每个部分都描述了字段的修饰符、名称、类型和注释。
- 此类中声明的每个方法和构造函数一节。 每个部分都描述了修饰符、名称、返回和参数类型以及方法的注释。 它还包含方法的编译代码，以 Java 字节码指令序列的形式。
然而，源类和编译类之间存在一些差异：
- 一个编译的类只描述一个类，而一个源文件可以包含多个类。 例如，一个描述具有一个内部类的源文件被编译为两个类文件：一个用于主类，一个用于内部类。 然而，主类文件包含对其内部类的引用，而在方法内部定义的内部类包含对其封闭方法的引用。
- 编译后的类当然不包含注释，但可以包含类、字段、方法和代码属性，这些属性可用于将附加信息与这些元素相关联。 从 Java 5 中引入用于相同目的注解以来，属性几乎变得毫无用处。
- 编译后的类不包含包和导入部分，因此所有类型名称都必须是完全限定的。
另一个非常重要的结构差异是编译后的类包含一个常量池部分。 这个池是一个数组，包含出现在类中的所有数字、字符串和类型常量。 这些常量仅在常量池中定义一次，并且在类文件的所有其他部分，持有它们的索引引用。 ASM 隐藏了与常量池相关的所有细节，这样您就不必费心了。 图 2.1 总结了编译类的整体结构。 Java 虚拟机规范的第 4 节中描述了确切的结构。
![Uploading image.png…]()

