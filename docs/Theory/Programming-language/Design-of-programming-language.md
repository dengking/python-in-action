# Design of programming language

本文接着上一篇进行描述。programming language虽然多，但是其实在设计一门programming language的时候，会涉及很多相同主题的内容，这也是我们在阅读各种programming language的Language reference时，会发现它们涉及了很多相同的主题。所以如果我们对这些common主题有一定的了解，那么掌握一门programming language会变得相对容易。

一般一门programming language的设计，可以分为两大块：

- language reference
- standard library

## Language reference

### Syntax 

programming language属于[formal language](https://en.wikipedia.org/wiki/Formal_language)，所以都会定义严格的syntax，“syntax"即语法。正如维基百科[Syntax (programming languages)](https://en.wikipedia.org/wiki/Syntax_(programming_languages))的[Levels of syntax](https://en.wikipedia.org/wiki/Syntax_(programming_languages)#Levels_of_syntax)段所总结的：

> Computer language syntax is generally distinguished into three levels:
>
> - Words – the lexical level, determining how characters form tokens;
> - Phrases – the grammar level, narrowly speaking, determining how tokens form phrases;
> - Context – determining what objects or variables names refer to, if types are valid, etc.

上述“Words”即词法，“Phrases”即语法，关于syntax，可以参考工程[compiler-principle](https://dengking.github.io/compiler-principle/)。

expression、statement等都是属于此范轴。

### Semantics 

参见维基百科[Semantics (computer science)](https://en.wikipedia.org/wiki/Semantics_(computer_science))。



### Type system

语言的设计者，需要考虑这门语言的type system，参见[Type system](../../Type-system/Type-system.md)。

语言的设计者还会向开发者提供对type system进行操作的接口，比如c++提供了`typeid`，`dynamic_cast`，python提供了`isinstance`，Java提供了`isinstanceof`。



### Runtime model

前面都是language的静态时，还需要对run time进行说明，比如data model、程序的运行模型等。

#### Data model

语言的设计者需要为 这门语言定义统一的data model，比如The Python Language Reference[¶](https://docs.python.org/3/reference/index.html#the-python-language-reference)中就有专门描述Data model[¶](https://docs.python.org/3/reference/datamodel.html#data-model)的 章节，与此类似的是，在cppreference的[Object](https://en.cppreference.com/w/cpp/language/object)中，对c++语言的data model进行了总结。



#### Run model

语言的设计者会假定该使用该语言所编写的程序运行与一个abstract machine上以便对run model进行描述，比如在cppreference的[Memory model](https://en.cppreference.com/w/cpp/language/memory_model)中有这样的描述：

> Defines the semantics of computer memory storage for the purpose of the C++ abstract machine.



在The Python Language Reference[¶](https://docs.python.org/3/reference/index.html#the-python-language-reference)的Execution model[¶](https://docs.python.org/3/reference/executionmodel.html#execution-model)中对python程序的run model进行了描述。



##### Abstract machines

一般在设计一门programming language的时候，都是使用abstract machine来进行描述的。在工程[automata-and-formal-language](https://dengking.github.io/automata-and-formal-language)中对这方面内容进行了描述。

[Stack machine](https://en.wikipedia.org/wiki/Stack_machine)





## Standard library

各种programming language都提供了大量的library，所以在学习一门programming language的时候，尤其需要注意其standard library。

一般standard library都会涉及到如下内容：

### Container

container指各种常见的数据结构，一般programming language的standard library都会包含这部分内容。



### Language support library

这是我在阅读[cppreference](https://en.cppreference.com/w/cpp/)的时候发现的一个概念，其中给出的[Language support library](https://en.cppreference.com/w/cpp/utility#Language_support)解释如下：

> Language support libraries provide classes and functions that interact closely with language features and support common language idioms.

依据此，python标准库中的很多library都可以归入此范轴：

- [Python Language Services](https://docs.python.org/3/library/language.html)
- [Python Runtime Services](https://docs.python.org/3/library/python.html)
- built-in



#### Run time info

比如python的标准库提供了Python Runtime Services[¶](https://docs.python.org/3/library/python.html#python-runtime-services)来供用户进行run time。



## Philosophy

programming language的设计者往往是遵循着一定的philosophy来设计这门语言的，作为使用者，了解这门语言的philosophy，也有助于我们对它的掌握。

TODO: 

添加Philosophy of python

添加Philosophy of c++（在https://en.wikipedia.org/wiki/C++中对此有描述）



## Specification of expectation of type in class-based OOP language

本节标题的含义是：在class-based OOP language中，如何描述对类型的expectation （期望或要求）。这是我在阅读[python doc](https://docs.python.org/3/)和[cppreference](https://en.cppreference.com/w/cpp/named_req)时，发现两者都使用"able"来描述对类型的期望。下面对此进行详细分析：

python和`c++`都是是class-based OOP语言，类可以看做是一种类型，阅读这两种语言的language reference，你就会发现：language reference需要描述类型的**feature**（**特性**），或者说当对某种类型的对象进行操作的时候，期望它具备某种**feature**，以使这种操作可以进行，定义这些特性，能够使对语言的表述非常便利，清晰，易懂。两种语言的normative text of standard中，都使用“able”来这些**feature**，比如callable、iterable、awaitable。c++通过[named requirement](https://en.cppreference.com/w/cpp/named_req)来定义这些特性，python中也有类似的概念，但是貌似python并没有像`c++`这样进行显式地定义。

相同的是，这两种语言都是让user-defined class通过实现**magic function**来为这个类型添加某种**特性**，所以在学习时，需要将**feature**和对应的**magic function**关联起来。



### [C++ Named requirements](https://en.cppreference.com/w/cpp/named_req)



### Python able

[iterable](https://docs.python.org/3/glossary.html#term-iterable)

[asynchronous iterable](https://docs.python.org/3/glossary.html#term-asynchronous-iterable)

[awaitable](https://docs.python.org/3/glossary.html#term-awaitable) 

[hashable](https://docs.python.org/3/glossary.html#term-hashable)

[immutable](https://docs.python.org/3/glossary.html#term-immutable)

[mutable](https://docs.python.org/3/glossary.html#term-mutable)

executable 

callable 

### [Design by contact](https://en.wikipedia.org/wiki/Design_by_contract) and expectation and generic programming

python通过duck type来实现generic programming，duck type是python的核心，python的很多standard library都是建立在这个机制上，python通过`able`来define the expectations of the standard library。

`c++`通过template来实现generic programming，template是c++的核心，c++的很多standard library都是建立在此机制上，c++通过[named requirement](https://en.cppreference.com/w/cpp/named_req)来define the expectations of the standard library。

通过上面的分析，我们已经知道python able和c++ named requirement本质上是相同的东西，

如果从[Design by contact](https://en.wikipedia.org/wiki/Design_by_contract)的角度来看的话，上面所说的expectation就是一种contact，programmer只有遵循了这个contact才能够正确地使用standard library。



python标准库的设计和c++标准库的设计是一种典范，值的借鉴学习。