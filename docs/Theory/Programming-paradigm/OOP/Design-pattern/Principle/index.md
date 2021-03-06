# OOP design principle

本章所描述的内容是理念型的，属于philosophy and design，而不是tips and tricks。我们描述目标，然后描述原则，坚持这些原则能够让我们更容易达到目标。

软件工程的先驱提出了众多principle，因此无法面面俱到，我觉得我们需要重点关注那些基本principle，很多principle本质上是相通的，因此我们需要融会贯通: 给出与它相关联的principle。

在进入具体的design pattern之前，有必要对oop中的design principle进行阐述，因为各种的design pattern其实都是贯彻着这些思想的，经典书籍[Design Patterns: Elements of Reusable Object-Oriented Software](https://en.wikipedia.org/wiki/Design_Patterns)对其有着非常好的总结，所以本文的很多内容都是从其中摘录的。

## Abstraction principle

OOP中的各种design pattern其实都遵循abstraction principle(参见文章Abstraction-principle):

1) program to an abstraction

2) design to an abstraction

在`Theory\Programming-paradigm\Abstraction-and-polymorphism`中提出了:

> **Program to an abstraction and polymorphism**

### Polymorphism

Polymorphism(OOP主要使用subtyping polymorphism(dynamic dispatch))是连接抽象与具体的桥梁。

正如在fluentcpp [On Design Patterns in C++](https://www.fluentcpp.com/2020/12/18/on-design-patterns-in-cpp/) 中所述:

> The main reason is that design patterns rely heavily on polymorphism, and the book exclusively uses **runtime polymorphism** in its examples, that is inheritance and virtual methods.

### Good abstraction 

各种design pattern，其实提供了对一些常见问题的非常好的abstraction，这些abstraction经历过时间的考验的。





### [Design by contract](https://en.wikipedia.org/wiki/Design_by_contract) and [Interface-based programming](https://en.wikipedia.org/wiki/Interface-based_programming)

两者其实本质上都在描述相同的内容，面向抽象，而不是面向具体。抽象是科学的思考方式，其实，这一段的描述，需要从对抽象的描述开始：解决问题，我们往往是先建立起抽象模型，这个抽象模型来解决具体的问题。





### Design pattern让我们避免使用if-else分支

“Design pattern让我们避免使用if-else分支”，这是我学习了各种各样的设计模式后，产生的一种想法，在下面文章中，都谈到了这一点：

- [Java设计模式——状态模式（STATE PATTERN）](https://blog.csdn.net/u012401711/article/details/52675873)
- [Refactoring.Guru](https://refactoring.guru/)的[Visitor](https://refactoring.guru/design-patterns/visitor)

这让我反思，使用if-else的坏处：

- 如果情况少，还比较好处理，一旦情况非常多，那么无论是编程、还是维护都非常难
- 使用if是不好扩展的
- 性能（这一点需要证明），使用if条件判断，是否有dynamic dispatch或者static dispatch性能好呢？



design pattern充分利用dynamic dispatch和static dispatch，只要我们遵循design-by-context，那么就可以在不修改的design（抽象模型）的情况下，进行扩展，dispatch是建立抽象与具体的桥梁。



## wikipedia [Design Patterns: Elements of Reusable Object-Oriented Software](https://en.wikipedia.org/wiki/Design_Patterns)

> NOTE: 原书是这个领域的开山之作。

### Introduction, Chapter 1

Chapter 1 is a discussion of [object-oriented](https://en.wikipedia.org/wiki/Object-oriented) design techniques, based on the authors' experience, which they believe would lead to good object-oriented software design, including:

1) "Program to an 'interface', not an '**implementation'**." (Gang of Four 1995:18)

> NOTE: 
>
> 上述principle其实是"Abstraction principle"在OOP design pattern中的实践；interface是abstraction，implementation是concrete。
>
> 关于abstraction、concrete，参见文章Abstract-and-concrete。
>
> SOLID中的“D”即[Dependency inversion principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle)表达的是和这相同的含义。
>
> 它是实现[Loose coupling](https://en.wikipedia.org/wiki/Loose_coupling)的关键实现。

2) [Composition over inheritance](https://en.wikipedia.org/wiki/Composition_over_inheritance): "Favor '[object composition](https://en.wikipedia.org/wiki/Object_composition)' over '[class inheritance](https://en.wikipedia.org/wiki/Inheritance_(computer_science))'." (Gang of Four 1995:20)



> NOTE: 下面的标题原文中并没有，是我添加上去的，便于阅读

### Program to an 'interface', not an 'implementation'

The authors claim the following as advantages of [interfaces](https://en.wikipedia.org/wiki/Interface_(computer_science)) over implementation:

1) clients remain unaware of the specific types of objects they use, as long as the object adheres to the interface

2) clients remain unaware of the classes that implement these objects; clients only know about the abstract class(es) defining the interface

Use of an interface also leads to [dynamic binding](https://en.wikipedia.org/wiki/Dynamic_dispatch) and [polymorphism](https://en.wikipedia.org/wiki/Polymorphism_in_object-oriented_programming), which are central features of object-oriented programming.

> NOTE: Program to an 'interface', not an 'implementation'是OOP的核心principle，它是符合"program to an abstraction"的，参见`Theory\Programming-paradigm\Abstraction-and-polymorphism\Program-to-an-abstraction`章节。
>
> polymorphism是OOP的核心特性。

> NOTE: 在`Theory\Programming-paradigm\Abstraction-and-polymorphism\Program-to-an-abstraction`中，对interface进行了描述。

### Composition over inheritance

> NOTE: 
>
> 这将在`Theory\Design-pattern\OOP-design-pattern\Principle\Composition-over-inheritance`中进行详细说明。
>
> 在`Theory\Programming-paradigm\Object-oriented-programming\Assemble`中，对Composition、inheritance进行了描述。



### Parameterized types

> NOTE: "Parameterized types"即"参数化类型"，它是generic programming的核心思想。

The authors also discuss so-called **parameterized types**, which are also known as [generics](https://en.wikipedia.org/wiki/Generic_programming) (Ada, Eiffel, [Java](https://en.wikipedia.org/wiki/Generics_in_Java), C#, VB.NET, and Delphi) or templates (C++). These allow any type to be defined without specifying all the other types it uses—the unspecified types are supplied as 'parameters' at the point of use.

The authors admit that delegation and parameterization are very powerful but add a warning:

### Aggregation

The authors further distinguish between '[Aggregation](https://en.wikipedia.org/wiki/Object_composition#Aggregation)', where one object 'has' or 'is part of' another object (implying that an aggregate object and its owner have identical lifetimes) and **acquaintance**, where one object merely 'knows of' another object. Sometimes acquaintance is called 'association' or the 'using' relationship. Acquaintance objects may request operations of each other, but they aren't responsible for each other. Acquaintance is a weaker relationship than aggregation and suggests much [looser coupling](https://en.wikipedia.org/wiki/Loose_coupling) between objects, which can often be desirable for maximum maintainability in a design.





## TODO: wikipedia [Object-oriented design](https://en.wikipedia.org/wiki/Object-oriented_design)





## TODO: wikipedia [Programming principles](https://en.wikipedia.org/wiki/Category:Programming_principles)