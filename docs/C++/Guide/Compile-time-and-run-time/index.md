# Compile-time and run-time

## C++



### [`static_assert`](https://en.cppreference.com/w/cpp/language/static_assert) VS [`assert`](https://en.cppreference.com/w/cpp/error/assert) 

#### [static_assert](https://en.cppreference.com/w/cpp/language/static_assert) 

`static_assert` 是compile-time；

syntax如下：

```c++
static_assert ( bool_constexpr , message )
```

它的第一个入参是：`bool_constexpr`，解释是：

> a contextually converted constant expression of type `bool`

`static_assert` 的第一个入参往往是和 `type_traits` 中的 各种关于 type trait的函数一起使用的，也就是compile time的，所以是constant expression。



#### `assert` 

run-time

### Static array VS dynamic array

在 https://stackoverflow.com/a/4810676 中提及了；



### Compile-time concept

下面总结了C++中的compile-time concept。

#### Constant expression

关于constant expression，参见`C-family-language\C++\Language-reference\Expressions\Operators\Other\Constant-expressions`章节。

#### Template

关于template，参见`C-family-language\C++\Language-reference\Template`章节。

#### Enum

参见`C++\Language-reference\Enum`章节。

### Compile-time context

下面是总结的（部分）compile-time context：

- template-meta programming
- array sizes，参见[Array declaration](https://en.cppreference.com/w/cpp/language/array)
- [switch statement](https://en.cppreference.com/w/cpp/language/switch)

### Run-time concept

`virtual`

### Static polymorphism VS dynamic polymorphism 

参见`C++\Guide\Implementation-of-polymorphism\`章节。



### Static type and dynamic type

参见`C++\Language-reference\Basic-concept\Type-system\Type-system\OOP-class-type\Polymorphic-type`。

#### [`static_cast`](https://en.cppreference.com/w/cpp/language/static_cast) VS [`dynamic_cast`](https://en.cppreference.com/w/cpp/language/dynamic_cast) 

`static_cast` 是 compile-time type conversion

`dynamic_cast` 是 run-time type conversion

#### `decltype` VS `typeid`

参见 `C++\Language-reference\Basic-concept\Type-system\Type-operation\Query-type` 章节。



