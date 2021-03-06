# Class template SFINAE



## cpppatterns [Class template SFINAE](https://cpppatterns.com/patterns/class-template-sfinae.html)

```c++
#include <type_traits>
template <typename T, typename Enable = void>
class foo;
template <typename T>
class foo<T, typename std::enable_if<std::is_integral<T>::value>::type>
{ };
template <typename T>
class foo<T, typename std::enable_if<std::is_floating_point<T>::value>::type>
{ };
```

### Requires

[c++11](https://cpppatterns.com/#/search/c++11) or newer.

> NOTE: 需要c++11的原因是`type_traits`library是C++11中添加的

### INTENT

Conditionally instantiate a class template depending on the template arguments.

### DESCRIPTION

We provide two partial specializations of the `foo` class template:

1. The template on [lines 6–8](https://cpppatterns.com/patterns/class-template-sfinae.html#line6) will only be instantiated when `T` is an integral type.
2. The template on [lines 10–12](https://cpppatterns.com/patterns/class-template-sfinae.html#line10) will only be instantiated when `T` is a floating point type.

This allows us to provide different implementations of the `foo` class depending on the template arguments it is instantiated with.

We have used [`std::enable_if`](http://en.cppreference.com/w/cpp/types/enable_if) on [line 7](https://cpppatterns.com/patterns/class-template-sfinae.html#line7) and [line 11](https://cpppatterns.com/patterns/class-template-sfinae.html#line11) to force instantiation to succeed only for the appropriate template arguments. This relies on [Substitution Failure Is Not An Error](https://en.wikipedia.org/wiki/Substitution_failure_is_not_an_error) (SFINAE), which states that failing to instantiate a template with some particular template arguments does not result in an error and simply discards that instantiation.

> NOTE: 如果对[`std::enable_if`](http://en.cppreference.com/w/cpp/types/enable_if)不熟悉，则可能会对上面这段话的解释产生质疑，我第一次就是如此。我以为`enable_if`为第二个template parameter `T`提供了default argument `void`，则在第一个template parameter `B`的argument为`false`的时候，则它提供的类型是`void`，则上述代码会与第一个class template匹配。这是错误的。阅读[`std::enable_if`](http://en.cppreference.com/w/cpp/types/enable_if)可知：
>
> If `B` is true, **std::enable_if** has a public member typedef `type`, equal to `T`; otherwise, there is no member typedef.
>
> 所以上述程序的实现所依赖的是SFINAE。

If you want to simply prevent a template from being instantiated for certain template arguments, consider using [`static_assert`](http://en.cppreference.com/w/cpp/language/static_assert) instead.

[Template specialization and enable_if problems [duplicate]](https://stackoverflow.com/questions/29502052/template-specialization-and-enable-if-problems)



## 实现分析

SFINAE作用于argument of type template parameter。