# 关于本章

本章描述的是c++中的constant expression。本章首先描述constant expression概念，然后描述c++11中引入的`constexp`，对这两者的介绍都是基于cppreference的，cppreference的对两者的讲解中会涉及到彼此，所以需要两者一起阅读。

传统的constant expression（比如`const int i = 0`）是非常任意理解的，但是从c++11开始，对这个概念进行了generalize（推广），引入了[`constexp`](https://en.cppreference.com/w/cpp/language/constexpr) 、[`consteval`](https://en.cppreference.com/w/cpp/language/consteval) ，显然阅推广就越抽象，越难以理解，所以我第一次直接阅读cppreference的时候，并没有抓住引入这些新feature的意图所在，而是直接阅读cppreference中介绍这个语言特性的各种详细细节，花费了很多时间，但是最终还是没有完全搞清楚这个特性所解决的问题，这种做法是非常愚蠢的。应该是先阅读what's new in c++（一般是在这个特性的提案中描述这部分内容， 但是提案往往非常漫长，最快捷的方式是阅读维基百科中所总结的），先搞清楚引入这些feature的意图，然后再去了解这些feature的grammar等详情。下面是基于维基百科所总结的what's in c++：



## What's in c++

### `constexpr`

维基百科[C++11#`constexpr` – Generalized constant expressions](https://en.wikipedia.org/wiki/C++11#constexpr_%E2%80%93_Generalized_constant_expressions)

维基百科的这一段所总结地非常好。

维基百科[Compile time function execution#Examples](https://en.wikipedia.org/wiki/Compile_time_function_execution)



```c++
template <int N>
struct Factorial {
  enum { value = N * Factorial<N - 1>::value };
};

template <>
struct Factorial<0> {
  enum { value = 1 };
};

// Factorial<4>::value == 24
// Factorial<0>::value == 1
void Foo() {
  int x = Factorial<0>::value;  // == 1
  int y = Factorial<4>::value;  // == 24
}
```

```c++
#include <cstdio>

constexpr int Factorial(int n) { return n ? (n * Factorial(n - 1)) : 1; }

constexpr int f10 = Factorial(10);

int main() {
  printf("%d\n", f10);
  return 0;
}
```

```c++
// Iterative factorial at compile time.
constexpr int Factorial(int n) {
  int result = 1;
  while (n > 1) {
    result *= n--;
  }
  return result;
}

int main() {
  constexpr int f4 = Factorial(4);  // f4 == 24
}
```



维基百科[Template metaprogramming#Compile-time class generation](https://en.wikipedia.org/wiki/Template_metaprogramming#Compile-time_class_generation)：

> In [C++11](https://en.wikipedia.org/wiki/C%2B%2B11) and [C++20](https://en.wikipedia.org/wiki/C%2B%2B20), [constexpr](https://en.wikipedia.org/wiki/Constexpr) and [`consteval`](https://en.cppreference.com/w/cpp/language/consteval) were introduced to let the compiler execute code. Using `constexpr` and `consteval`, one can use the usual recursive factorial definition with the non-templated syntax.

上述两篇中，给出的example是非常好的，非常具有启发性。

### `consteval`

[Immediate functions in C++20](https://awfulcode.io/2019/01/26/immediate-functions-in-c20/)

[What is consteval?](https://stackoverflow.com/questions/53347298/what-is-consteval)



### SUMMARY

`constexpr`和`consteval`的引入，是为了强化C++的[Compile time function execution](https://en.wikipedia.org/wiki/Compile_time_function_execution)