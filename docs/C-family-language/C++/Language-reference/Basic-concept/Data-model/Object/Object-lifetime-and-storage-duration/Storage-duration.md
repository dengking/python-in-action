# Object storage duration and lifetime



storage duration和lifetime是[object](https://en.cppreference.com/w/cpp/language/object)的重要属性，这两个属性是密切相关的，在cppreference [Object](https://en.cppreference.com/w/cpp/language/object)中对此进行了介绍，本文讨论object的storage duration和lifetime。

object的storage duration和lifetime是两个非常重要的概念，是理解后续很多内容的基础：

| 步骤             | 说明                                                        | 章节                                    |
| ---------------- | ----------------------------------------------------------- | --------------------------------------- |
| allocation       | 为object分配内存区域                                        |                                         |
| initialization   | 初始化object<br>- 如果是OOP object，会调用合适的constructor | `C++\Language-reference\Initialization` |
| deinitialization | 反初始化object<br>- 如果是OOP object，会调用destructor      |                                         |
| deallocation     | 回收object的内存                                            |                                         |

需要注意的是：上面是按照发生顺序进行排列的，即：allocation->initialization->deinitialization->deallocation。

## cppreference [Lifetime](https://en.cppreference.com/w/cpp/language/lifetime)



### Temporary object lifetime



### Storage reuse



### Access outside of lifetime





## cppreference [Storage class specifiers](https://en.cppreference.com/w/cpp/language/storage_duration)

> NOTE: 原文的内容是比较杂乱的，既包含了**storage duration**又包含了**linkage**，实际上它们两者是independent property of object，所以应该分开来进行讨论，对linkage的讨论，在`C-family-language\C-and-C++\From-source-code-to-exec\Link\Linkage`章节；
>
> 原文之所以将它们放到一起是因为：C++和C并没有提供专门分别描述这两种property的specifier，而是提供的合并的specifier，关于这一点，在[Storage class specifiers](https://en.cppreference.com/w/cpp/language/storage_duration)中进行了详细的讨论。
>
> 我们按照在`Theory\Programming-language\How-to-master-programming-language.md#`中总结的：首先学习property，然后学习描述这些property的specifier的方式来进行学习。
>
> 本文仅仅关注原文中object storage duration相关的内容，我们以如下的思路来组织内容：
>
> - C++ object有哪几种storage duration
> - C++ language提供了哪些specifier来供programmer对storage duration进行控制/描述，即C++ language中，有哪些Storage class specifiers
>
> 所以，本文与原文的组织有所差异。

### [Storage duration](https://en.cppreference.com/w/cpp/language/storage_duration#Storage_duration)

> NOTE: 下面描述该表格的组织思路: 
>
> 对于object，它的storage duration属性，决定了它的“allocation time point”（何时分配）和 “dealloaction time point”（何时被回收）。
>
> 列说明: 
>
> 列object: 描述哪些object具备对应的storage duration；
>
> 列scope: 从OS的角度来分析storage duration；
>
> 列"allocation": 描述的是"allocation time point"；
>
> 列"dealloaction": 描述的是"dealloaction time point"；

| storage duration | allocation                                                   | dealloaction                                                 | objects                                                      | scope               |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------- |
| automatic        | allocated at the beginning of the enclosing code **block**   | `C++`: deallocated at the beginning of the enclosing code **block**<br>C: deallocated when it is exited by any means ([goto](https://en.cppreference.com/w/c/language/goto), [return](https://en.cppreference.com/w/c/language/return), reaching the end) | 1. local objects, **except** those declared <br>`static` （**static object**）, <br>`extern`（**extern object**） or<br> `thread_local`. | stack;<br>function; |
| thread           | allocated when the thread begins                             | deallocated when the thread ends                             | 1. objects declared `thread_local` have this storage duration | stack;<br/>thread;  |
| static           | allocated when the **program** begins                        | deallocated when the **program** ends                        | 1. objects declared at namespace scope (including **global namespace**) <br>2. those declared with `static` or `extern` （包括:<br>**static local object**、<br>**extern local object**） | process;            |
| dynamic          | allocated by using [dynamic memory allocation](https://en.cppreference.com/w/cpp/memory) function | deallocated by using [dynamic memory deallocation](https://en.cppreference.com/w/cpp/memory) function |                                                              | heap;               |



#### 统一描述方式: object with `***`storage duration

本节标题的含义是: 后面为了描述的统一性，我们统一使用"object with `***`storage (duration)"格式，括号括起来的部分，表示是optional；下面是例子: 

描述具备automatic storage duration的object: **object with automatic storage** ;

描述具备static storage duration的object: object with static storage ;



#### Automatic storage duration

我们平时所说的"自动变量"就具备这种storage duration；

##### Automatic storage and RAII

object with automatic storage的lifetime is bound by "`{}`"，对这个特性的一个非常重要的应用就是: RAII，参见`C++\Idiom\OOP\RAII`。

#### Dynamic storage duration

> TODO: 添加一些内容



#### Static storage duration

对于object with static storage duration，相比于其它类型的object，它的initialization是比较复杂的，后面会进行专门的描述；



#### Thread storage duration(since C++11)

这是C++11引入的，源于"C++11 introduced a standardized memory model"，关于此，参见`C++\Language-reference\Basic-concept\Abstract-machine\Memory-model`。



#### Initialization of object

C++语言中，对object的initialization是受到了object的storage duration属性的影响的，对于上述四种storage duration，由于它们的allocation time point不同，就造成了它们的initialization time point的不同；其中比较特殊的是static storage duration和thread storage duration，cppreference中，对它们的描述是比较分散的，我将它们进行了整理，统一放到了initialization章节中。



#### TODO: Control of object

从control theory的角度来思考上面描述的几种object。

deallocation往往是out of control的；

对于OOP object，它的deinitialization对应的是它的destructor；而destructor的调用，在某些情况下，是out of control的。

对于Non-OOP object，它们一般没有deinitialization；



#### Temporary object

Temporary object的storage duration是什么呢？

关于temporary object，参见cppreference [Lifetime](https://en.cppreference.com/w/cpp/language/lifetime) `#`  “Temporary object lifetime” 章节；









#### Example: ***automatic*** storage duration: extern local object

来源：[How does linker handle variables with different linkages?](https://stackoverflow.com/questions/51737002/how-does-linker-handle-variables-with-different-linkages) `#` [A](https://stackoverflow.com/a/51737215)

```c++
#include <iostream>

static int x; // a namespace scope, so `x` has internal linkage, static storage duration

int f()
{
	extern int x; // static storage duration
	++x;
}

int g()
{
	extern int x; // static storage duration
	std::cout << x << '\n';
}

int main()
{
	g();
	f();
	g();
}
// g++ test.cpp

```



### [Storage class specifiers](https://en.cppreference.com/w/cpp/language/storage_duration)

> NOTE: 本节描述描述storage duration的specifier

| specifier                   | storage duration                                             | linkage    | C++ version                                                  | Explanation                                                  | function         |
| --------------------------- | ------------------------------------------------------------ | ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ---------------- |
| `auto` (until C++11)        | *automatic*                                                  | no linkage | 在[Storage class specifiers](https://en.cppreference.com/w/cpp/language/storage_duration)的Notes有说明: Since C++11, `auto` is no longer a storage class specifier; it is used to indicate type deduction. |                                                              | 不可修饰function |
| `register` (until C++17)    | *automatic*                                                  | no linkage |                                                              |                                                              | 不可修饰function |
| `static`                    | *static* or *thread*                                         | *internal* |                                                              |                                                              | 可以修饰function |
| `extern`                    | *static* or *thread*                                         | *external* |                                                              | It specifies **external linkage**, and does not technically affect **storage duration**, but it cannot be used in a **definition** of an **automatic storage duration** object, so all `extern` objects have **static** or **thread** durations. In addition, a variable declaration that uses `extern` and has no initializer is not a [definition](https://en.cppreference.com/w/cpp/language/definition).<br> 上面这段话的意思是：`extern` variable只能够link to **object with static storage** or **object with `thread_local` storage**，所以`extern` variable的storage duration是static的，这是因为显然它需要compiler和linker在编译阶段就能够找到这个object。 | 可以修饰function |
| `thread_local`(since C++11) | *thread*                                                     |            |                                                              |                                                              | 不可修饰function |
| `mutable`                   | does not affect storage duration or linkage. See [const/volatile](https://en.cppreference.com/w/cpp/language/cv) for the explanation. |            |                                                              |                                                              | 不可修饰function |

> NOTE: 
>
> 无论是`C++`还是C，都没有专门描述linkage的specifier，而是将描述**storage duration**和描述**linkage**的specifier合并在一起，对于linkage，并没有单独描述它的specifier，但是，compiler提供了default linkage；关于这一点，我们需要仔细阅读cppreference [Storage class specifiers](https://en.cppreference.com/w/cpp/language/storage_duration) 和 creference [Storage-class specifiers](https://en.cppreference.com/w/c/language/storage_duration)：
>
> cppreference [Storage class specifiers](https://en.cppreference.com/w/cpp/language/storage_duration) 中，对specifiers的描述如下：
>
> > The **storage class specifiers** are a part of the *decl-specifier-seq* of a name's [declaration syntax](https://en.cppreference.com/w/cpp/language/declarations). Together with the [scope](https://en.cppreference.com/w/cpp/language/scope) of the name, they control two independent properties of the name: its *storage duration* and its *linkage*.
>
> 在`C++`中，将这些specifier称为 storage class specifier。
>
> creference [Storage-class specifiers](https://en.cppreference.com/w/c/language/storage_duration) 中，对specifiers的描述如下：
>
> > Specify *storage duration* and *linkage* of objects and functions
>
> 在`C`中，将这些specifier称为 storage class specifier。
>
> 
>
> 我们需要深入思考：为什么将linkage和storage duration的specifier合并？
>
> 关于此的原因之一可以参看上述table中`extern`的Explanation，原因之二则是出于语言设计者出于对语言简便性的考虑（在`Theory\Programming-languageDesign-of-programming-language.md\#Design of specifier`中进行了讨论）



#### linkage and storage duration of function

需要注意的是：对于function而言，它没有**storage duration** property，只有**linkage** property，对于function而言，讨论它的storage duration是没有意义的。对于object而言，它既有**storage duration** property，又有**linkage** property。

`static` 、 `extern` 也可以 修饰 function，来控制它的linkage。



#### `static` specifier and static storage duration

使用`static` specifier修饰的object具有static storage duration，但是具有static storage duration的object，不一定要使用`static` specifier来修饰。

### Static local variables

> NOTE: 对于static local variable，

Variables declared at **block scope** with the specifier `static` or `thread_local` (since C++11) have static or thread (since C++11) storage duration but are initialized the first time control passes through their declaration (unless their initialization is [zero-](https://en.cppreference.com/w/cpp/language/zero_initialization) or [constant-initialization](https://en.cppreference.com/w/cpp/language/constant_initialization), which can be performed before the block is first entered). On all further calls, the declaration is skipped.

> NOTE: 上面这段话关于initialization的描述是不易理解的？它的意思是：对于static local variable，它的initialization的发生时间如下：
>
> - [zero-](https://en.cppreference.com/w/cpp/language/zero_initialization) or [constant-initialization](https://en.cppreference.com/w/cpp/language/constant_initialization) can be performed before the block is first entered
> - others are initialized the first time control passes through their declaration



#### Initialization of static local variable concurrently (since C++11)

> NOTE: 这种情况的典型就是：线程执行函数中声明了一个static local variable。

If multiple threads attempt to initialize the same **static local variable** concurrently, the **initialization** occurs exactly once (similar behavior can be obtained for arbitrary functions with [std::call_once](../thread/call_once.html)).

Note: usual implementations of this feature use variants of the double-checked locking pattern, which reduces runtime overhead for already-initialized local statics to a single non-atomic boolean comparison.

> NOTE: double-checked locking pattern在工程parallel-computing的`Synchronization\Lock`章节描述。




