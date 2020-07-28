# Value and reference semantics

在阅读[如何评价 C++11 的右值引用（Rvalue reference）特性？ - zihuatanejo的回答 - 知乎](https://www.zhihu.com/question/22111546/answer/31929118) 时，发现了其中对value semantic的讨论，其中“[值语义](https://link.zhihu.com/?target=http%3A//www.parashift.com/c%2B%2B-faq/val-vs-ref-semantics.html)”的链接的文章在下文中收录了。



## isocpp faq Reference and Value Semantics [¶](https://isocpp.org/wiki/faq/value-vs-ref-semantics) [Δ](https://isocpp.org/wiki/faq/value-vs-ref-semantics#)

### What is value and/or reference semantics, and which is best in C++? [¶](https://isocpp.org/wiki/faq/value-vs-ref-semantics#val-vs-ref-semantics) [Δ](https://isocpp.org/wiki/faq/value-vs-ref-semantics#)

With **reference semantics**, assignment is a pointer-copy (i.e., a *reference*). Value (or “copy”) semantics mean assignment copies the value, not just the pointer. C++ gives you the choice: use the assignment `operator` to copy the value (copy/value semantics), or use a pointer-copy to copy a pointer (reference semantics). C++ allows you to override the assignment `operator` to do anything your heart desires, however the default (and most common) choice is to copy the *value.*

> NOTE: 在java、python中，assignment的含义是bind；但是在c++中，assignment的含义是copy；使用本文的观点来看待这个问题就是：python、java都是reference semantic的，而c++是value semantic的。

> NOTE: C++默认是采用的value semantic，只有当我们显式地使用`*`来表示指针，使用`&`来表示reference。

Pros of reference semantics: flexibility and dynamic binding (you get **dynamic binding** in `C++` only when you pass by pointer or pass by reference, not when you pass by value).

> NOTE: C++中，只有当使用pointer、reference的时候，才能够实现dynamic binding。

Pros of value semantics: speed. “Speed” seems like an odd benefit for a feature that requires an object (vs. a pointer) to be copied, but the fact of the matter is that one usually accesses an object more than one copies the object, so the cost of the occasional copies is (usually) more than offset by the benefit of having an actual object rather than a pointer to an object.

> NOTE:value语义的优点:速度。
> “速度”似乎是一个奇怪的对一个功能需要一个对象(比一个指针)被复制,但事实是,通常一个对象访问对象的多个副本,所以偶尔的副本的成本(通常)超过抵消的好处有一个实际的对象而不是一个指向对象的指针。

There are three cases when you have an actual object as opposed to a pointer to an object: local objects, global/`static` objects, and fully contained member objects in a class. The most important of these is the last (“composition”).

> NOTE: composition 对应的是 [cppreference Subobjects](https://en.cppreference.com/w/cpp/language/object#Subobjects) 。

More info about copy-vs-reference semantics is given in the next FAQs. Please read them all to get a balanced perspective. The first few have intentionally been slanted toward value semantics, so if you only read the first few of the following FAQs, you’ll get a warped perspective.

Assignment has other issues (e.g., shallow vs. deep copy) which are not covered here.

### What is “`virtual` data,” and how-can / why-would I use it in C++? [¶](https://isocpp.org/wiki/faq/value-vs-ref-semantics#virt-data) [Δ](https://isocpp.org/wiki/faq/value-vs-ref-semantics#)

For example, `class` `Stack` might have an `Array` member object (using a pointer), and derived `class` `StretchableStack` might **override** the base class member data from `Array` to `StretchableArray`. For this to work, `StretchableArray` would have to inherit from `Array`, so `Stack` would have an `Array*`. `Stack`’s normal constructors would initialize this `Array*` with a `new Array`, but `Stack` would also have a (possibly `protected`) constructor that would accept an `Array*` from a derived class. `StretchableStack`’s constructor would provide a `new StretchableArray` to this special constructor.

```c++
#include <iostream>

class Array
{
public:
	Array()
	{
		std::cout << __PRETTY_FUNCTION__ << std::endl;
	}
	virtual ~Array()
	{
		std::cout << __PRETTY_FUNCTION__ << std::endl;
	}
};

class StretchableArray: public Array
{
public:
	StretchableArray()
	{
		std::cout << __PRETTY_FUNCTION__ << std::endl;
	}
	virtual ~StretchableArray()
	{
		std::cout << __PRETTY_FUNCTION__ << std::endl;
	}
};

class Stack
{
public:
	Stack()
	{

	}
	virtual void InitArray()
	{
		m_A = new Array();
	}
	virtual ~Stack()
	{
		if (m_A)
		{
			delete m_A;
		}
	}
protected:
	Array* m_A { nullptr };
};

class StretchableStack: public Stack
{
public:
	StretchableStack()
	{

	}
	virtual void InitArray()
	{
		m_A = new StretchableArray();
	}
};

int main()
{
	{
		Stack s;
		s.InitArray();
	}
	std::cout << "----------------------" << std::endl;
	{
		StretchableStack ss;
		ss.InitArray();
	}
}
// g++ --std=c++11 test.cpp 
```

> NOTE: 上述程序的输出如下：
>
> ```c++
> Array::Array()
> virtual Array::~Array()
> ----------------------
> Array::Array()
> StretchableArray::StretchableArray()
> virtual StretchableArray::~StretchableArray()
> virtual Array::~Array()
> 
> ```
>
> 



### Sounds like I should never use reference semantics, right? [¶](https://isocpp.org/wiki/faq/value-vs-ref-semantics#ref-semantics-sometimes-good) [Δ](https://isocpp.org/wiki/faq/value-vs-ref-semantics#)





### Does the poor performance of reference semantics mean I should pass-by-value? [¶](https://isocpp.org/wiki/faq/value-vs-ref-semantics#pass-by-value) [Δ](https://isocpp.org/wiki/faq/value-vs-ref-semantics#)