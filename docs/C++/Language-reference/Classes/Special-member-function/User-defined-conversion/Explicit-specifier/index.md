# explicit specifier

`explicit`供programmer对conversion进行控制，它体现了C++的灵活性。

## cppreference [explicit specifier](https://en.cppreference.com/w/cpp/language/explicit)



## Purpose of `explicit` 

参考：

- [What does the explicit keyword mean?](https://stackoverflow.com/questions/121162/what-does-the-explicit-keyword-mean)



[A](https://stackoverflow.com/a/121163)

The compiler is allowed to make one implicit conversion to resolve the parameters to a function. What this means is that the compiler can use constructors callable with a **single parameter** to convert from one type to another in order to get the right type for a parameter.

Here's an example class with a constructor that can be used for implicit conversions:

```C++
#include <iostream>

class Foo
{
public:
	// single parameter constructor, can be used as an implicit conversion
	Foo(int foo)
			: m_foo(foo)
	{
	}

	int GetFoo()
	{
		return m_foo;
	}

private:
	int m_foo;
};

void DoBar(Foo foo)
{
	int i = foo.GetFoo();
	std::cout << i << std::endl;
}
int main()
{
	DoBar(42);
}
// g++ --std=c++11 test.cpp
```

> NOTE:  输出如下：
>
> ```
> 42
> ```
>
> 

The argument is not a `Foo` object, but an `int`. However, there exists a constructor for `Foo` that takes an `int` so this constructor can be used to convert the parameter to the correct type.

The compiler is allowed to do this once for each parameter.

Prefixing the `explicit` keyword to the constructor prevents the compiler from using that constructor for implicit conversions. Adding it to the above class will create a compiler error at the function call `DoBar (42)`. It is now necessary to call for conversion explicitly with `DoBar (Foo (42))`

The reason you might want to do this is to avoid accidental construction that can hide bugs. Contrived example:

- You have a `MyString(int size)` class with a constructor that constructs a string of the given size. You have a function `print(const MyString&)`, and you call `print(3)` (when you *actually* intended to call `print("3")`). You expect it to print "3", but it prints an empty string of length 3 instead.



```c++
#include <iostream>

class Foo
{
public:
	// single parameter constructor, can be used as an implicit conversion
	explicit Foo(int foo)
			: m_foo(foo)
	{
	}

	int GetFoo()
	{
		return m_foo;
	}

private:
	int m_foo;
};

void DoBar(Foo foo)
{
	int i = foo.GetFoo();
	std::cout << i << std::endl;
}
int main()
{
	DoBar( { 42 });
}
// g++ --std=c++11 test.cpp
```

> NOTE: 编译报错如下：
>
> ```c++
> test.cpp: 在函数‘int main()’中:
> test.cpp:28:15: 错误：从初始化列表转换为‘Foo’将使用显式构造函数‘Foo::Foo(int)’
>   DoBar( { 42 });
> ```

```c++
#include <iostream>

class Foo
{
public:
	// single parameter constructor, can be used as an implicit conversion
	explicit Foo(int foo)
			: m_foo(foo)
	{
	}

	int GetFoo()
	{
		return m_foo;
	}

private:
	int m_foo;
};

void DoBar(Foo foo)
{
	int i = foo.GetFoo();
	std::cout << i << std::endl;
}
int main()
{
	DoBar(Foo { 42 });
}

```



[A](https://stackoverflow.com/a/121216)

```c++
class String
{
public:
	String(int n) // allocate n bytes to the String object
	{

	}
	String(const char *p) // initializes object with char *p
	{

	}
};

int main()
{
	String mystring = 'x';
}

```

The character `'x'` will be implicitly converted to `int` and then the `String(int)` constructor will be called. But, this is not what the user might have intended. So, to prevent such conditions, we shall define the constructor as `explicit`:

```c++
class String
{
public:
	explicit String(int n) // allocate n bytes to the String object
	{

	}
	String(const char *p) // initializes object with char *p
	{

	}
};

int main()
{
	// String mystring = 'x'; // 编译报错
	String mystring { 2 };
}

```



## Example

### `libstdc++ unique_ptr`

```c++
       /** Takes ownership of a pointer.
        *
        * @param __p  A pointer to an object of @c element_type
        *
        * The deleter will be value-initialized.
        */
       template <typename _Up = _Dp,
                 typename = _DeleterConstraint<_Up>>
         explicit
         unique_ptr(pointer __p) noexcept
         : _M_t(__p)
         { }
```

下面是错误用法

```c++
	static std::unique_ptr<UstStockOpt::CHSAMUSTTraderApi> New(const char *WorkPath)
	{
		return { NewStockOptApi(WorkPath)};
	}
```
编译报错如下：

```c++
In file included from main.cpp:8:
./../common.h:99:10: fatal error: chosen constructor is explicit in copy-initialization
                return { NewStockOptApi(WorkPath)};
                       ^~~~~~~~~~~~~~~~~~~~~~~~~~~
/usr/bin/../lib/gcc/x86_64-redhat-linux/4.8.5/../../../../include/c++/4.8.5/bits/unique_ptr.h:141:7: note: constructor declared
      here
      unique_ptr(pointer __p) noexcept

```