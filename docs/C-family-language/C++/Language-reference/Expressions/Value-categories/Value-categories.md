# [Value categories](https://en.cppreference.com/w/cpp/language/value_category)

Each C++ [expression](https://en.cppreference.com/w/cpp/language/expressions) (an operator with its operands, a literal, a variable name, etc.) is characterized by two independent properties: a *type* and a *value category*. Each expression has some non-reference type, and each expression belongs to exactly one of the three primary value categories: *prvalue*, *xvalue*, and *lvalue*, defined as follows:

- a glvalue (“generalized” lvalue) is an expression whose evaluation determines the identity of an object, bit-field, or function;
- a prvalue (“pure” rvalue) is an expression whose evaluation either



- an xvalue (an “eXpiring” value) is a glvalue that denotes an object or bit-field whose resources can be reused;
- an lvalue (so-called, historically, because lvalues could appear on the left-hand side of an assignment expression) is a glvalue that is not an xvalue;
- an rvalue (so-called, historically, because rvalues could appear on the right-hand side of an assignment expression) is a prvalue or an xvalue.

Note: this taxonomy went through significant changes with past C++ standard revisions, see [History](https://en.cppreference.com/w/cpp/language/value_category#History) below for details.

## Primary categories

### lvalue

The following expressions are *lvalue expressions*:

- the name of a variable, a function, a [template parameter object](https://en.cppreference.com/w/cpp/language/template_parameters#Non-type_template_parameter) (since C++20), or a data member, regardless of type, such as [std::cin](http://en.cppreference.com/w/cpp/io/cin) or [std::endl](http://en.cppreference.com/w/cpp/io/manip/endl). Even if the variable's type is rvalue reference, the expression consisting of its name is an lvalue expression;
- a function call or an overloaded operator expression, whose return type is lvalue reference, such as [std::getline](http://en.cppreference.com/w/cpp/string/basic_string/getline)([std::cin](http://en.cppreference.com/w/cpp/io/cin), str), [std::cout](http://en.cppreference.com/w/cpp/io/cout) << 1, str1 = str2, or ++it;
- a = b, a += b, a %= b, and all other built-in [assignment and compound assignment](https://en.cppreference.com/w/cpp/language/operator_assignment) expressions;
- ++a and --a, the built-in [pre-increment and pre-decrement](https://en.cppreference.com/w/cpp/language/operator_incdec#Built-in_prefix_operators) expressions;
- *p, the built-in [indirection](https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_indirection_operator) expression;
- a[n] and p[n], the built-in [subscript](https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_subscript_operator) expressions, where one operand in a[n] is an array lvalue (since C++11);
- a.m, the [member of object](https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_member_access_operators) expression, except where `m` is a member enumerator or a non-static member function, or where `a` is an rvalue and `m` is a non-static data member of non-reference type;
- p->m, the built-in [member of pointer](https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_member_access_operators) expression, except where `m` is a member enumerator or a non-static member function;
- a.*mp, the [pointer to member of object](https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_pointer-to-member_access_operators) expression, where `a` is an lvalue and `mp` is a pointer to data member;
- p->*mp, the built-in [pointer to member of pointer](https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_pointer-to-member_access_operators) expression, where `mp` is a pointer to data member;
- a, b, the built-in [comma](https://en.cppreference.com/w/cpp/language/operator_other#Built-in_comma_operator) expression, where `b` is an lvalue;
- a ? b : c, the [ternary conditional](https://en.cppreference.com/w/cpp/language/operator_other#Conditional_operator) expression for some `b` and `c` (e.g., when both are lvalues of the same type, but see [definition](https://en.cppreference.com/w/cpp/language/operator_other#Conditional_operator) for detail);
- a [string literal](https://en.cppreference.com/w/cpp/language/string_literal), such as "Hello, world!";
- a cast expression to lvalue reference type, such as static_cast<int&>(x);

| a function call or an overloaded operator expression, whose return type is rvalue reference to function;a cast expression to rvalue reference to function type, such as static_cast<void (&&)(int)>(x). | (since C++11) |
| ------------------------------------------------------------ | ------------- |
|                                                              |               |

Properties:

- Same as glvalue (below).
- Address of an lvalue may be taken: &++i[[1\]](https://en.cppreference.com/w/cpp/language/value_category#cite_note-1) and &[std::endl](http://en.cppreference.com/w/cpp/io/manip/endl) are valid expressions.
- A modifiable lvalue may be used as the left-hand operand of the built-in assignment and compound assignment operators.
- An lvalue may be used to [initialize an lvalue reference](https://en.cppreference.com/w/cpp/language/reference_initialization); this associates a new name with the object identified by the expression.

### prvalue

The following expressions are *prvalue expressions*:

- a [literal](https://en.cppreference.com/w/cpp/language/expressions#Literals) (except for [string literal](https://en.cppreference.com/w/cpp/language/string_literal)), such as 42, true or nullptr;
- a function call or an overloaded operator expression, whose return type is non-reference, such as str.substr(1, 2), str1 + str2, or it++;
- a++ and a--, the built-in [post-increment and post-decrement](https://en.cppreference.com/w/cpp/language/operator_incdec#Built-in_postfix_operators) expressions;
- a + b, a % b, a & b, a << b, and all other built-in [arithmetic](https://en.cppreference.com/w/cpp/language/operator_arithmetic) expressions;
- a && b, a || b, !a, the built-in [logical](https://en.cppreference.com/w/cpp/language/operator_logical) expressions;
- a < b, a == b, a >= b, and all other built-in [comparison](https://en.cppreference.com/w/cpp/language/operator_comparison) expressions;
- &a, the built-in [address-of](https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_address-of_operator) expression;
- a.m, the [member of object](https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_member_access_operators) expression, where `m` is a member enumerator or a non-static member function[[2\]](https://en.cppreference.com/w/cpp/language/value_category#cite_note-pmfc-2), or where `a` is an rvalue and `m` is a non-static data member of non-reference type (until C++11);
- p->m, the built-in [member of pointer](https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_member_access_operators) expression, where `m` is a member enumerator or a non-static member function[[2\]](https://en.cppreference.com/w/cpp/language/value_category#cite_note-pmfc-2);
- a.*mp, the [pointer to member of object](https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_pointer-to-member_access_operators) expression, where `mp` is a pointer to member function[[2\]](https://en.cppreference.com/w/cpp/language/value_category#cite_note-pmfc-2), or where `a` is an rvalue and `mp` is a pointer to data member (until C++11);
- p->*mp, the built-in [pointer to member of pointer](https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_pointer-to-member_access_operators) expression, where `mp` is a pointer to member function[[2\]](https://en.cppreference.com/w/cpp/language/value_category#cite_note-pmfc-2);
- a, b, the built-in [comma](https://en.cppreference.com/w/cpp/language/operator_other#Built-in_comma_operator) expression, where `b` is an rvalue;
- a ? b : c, the [ternary conditional](https://en.cppreference.com/w/cpp/language/operator_other#Conditional_operator) expression for some `b` and `c` (see [definition](https://en.cppreference.com/w/cpp/language/operator_other#Conditional_operator) for detail);
- a cast expression to non-reference type, such as static_cast<double>(x), [std::string](http://en.cppreference.com/w/cpp/string/basic_string){}, or (int)42;
- the [`this`](https://en.cppreference.com/w/cpp/language/this) pointer;
- an [enumerator](https://en.cppreference.com/w/cpp/language/enum);
- non-type [template parameter](https://en.cppreference.com/w/cpp/language/template_parameters) unless its type was a class or (since C++20) an lvalue reference type;

| a [lambda expression](https://en.cppreference.com/w/cpp/language/lambda), such as [](int x){ return x * x; }; | (since C++11) |
| ------------------------------------------------------------ | ------------- |
|                                                              |               |

| a [requires-expression](https://en.cppreference.com/w/cpp/language/constraints), such as requires (T i) { typename T::type; };a specialization of a [concept](https://en.cppreference.com/w/cpp/language/constraints), such as [EqualityComparable](http://en.cppreference.com/w/cpp/concepts/EqualityComparable)<int>. | (since C++20) |
| ------------------------------------------------------------ | ------------- |
|                                                              |               |

Properties:

- Same as rvalue (below).
- A prvalue cannot be [polymorphic](https://en.cppreference.com/w/cpp/language/object#Polymorphic_objects): the [dynamic type](https://en.cppreference.com/w/cpp/language/type#Dynamic_type) of the object it identifies is always the type of the expression.
- A non-class non-array prvalue cannot be [cv-qualified](https://en.cppreference.com/w/cpp/language/cv). (Note: a function call or cast expression may result in a prvalue of non-class cv-qualified type, but the cv-qualifier is immediately stripped out.)
- A prvalue cannot have [incomplete type](https://en.cppreference.com/w/cpp/language/type#Incomplete_type) (except for type void, see below, or when used in [decltype specifier](https://en.cppreference.com/w/cpp/language/decltype))
- A prvalue cannot have [abstract class type](https://en.cppreference.com/w/cpp/language/abstract_class) or an array thereof.

### xvalue

The following expressions are *xvalue expressions*:

- a function call or an overloaded operator expression, whose return type is rvalue reference to object, such as std::move(x);
- a[n], the built-in [subscript](https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_subscript_operator) expression, where one operand is an array rvalue;
- a.m, the [member of object](https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_member_access_operators) expression, where `a` is an rvalue and `m` is a non-static data member of non-reference type;
- a.*mp, the [pointer to member of object](https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_pointer-to-member_access_operators) expression, where `a` is an rvalue and `mp` is a pointer to data member;
- a ? b : c, the [ternary conditional](https://en.cppreference.com/w/cpp/language/operator_other#Conditional_operator) expression for some `b` and `c` (see [definition](https://en.cppreference.com/w/cpp/language/operator_other#Conditional_operator) for detail);
- a cast expression to rvalue reference to object type, such as static_cast<char&&>(x);

| any expression that designates a temporary object, after [temporary materialization](https://en.cppreference.com/w/cpp/language/implicit_conversion#Temporary_materialization). | (since C++17) |
| ------------------------------------------------------------ | ------------- |
|                                                              |               |

Properties:

- Same as rvalue (below).
- Same as glvalue (below).

In particular, like all rvalues, xvalues bind to rvalue references, and like all glvalues, xvalues may be [polymorphic](https://en.cppreference.com/w/cpp/language/object#Polymorphic_objects), and non-class xvalues may be [cv-qualified](https://en.cppreference.com/w/cpp/language/cv).

## Mixed categories

### glvalue

A *glvalue expression* is either lvalue or xvalue.

Properties:

- A glvalue may be implicitly converted to a prvalue with lvalue-to-rvalue, array-to-pointer, or function-to-pointer [implicit conversion](https://en.cppreference.com/w/cpp/language/implicit_cast).
- A glvalue may be [polymorphic](https://en.cppreference.com/w/cpp/language/object#Polymorphic_objects): the [dynamic type](https://en.cppreference.com/w/cpp/language/type#Dynamic_type) of the object it identifies is not necessarily the static type of the expression.
- A glvalue can have [incomplete type](https://en.cppreference.com/w/cpp/language/type#Incomplete_type), where permitted by the expression.

### rvalue

An *rvalue expression* is either prvalue or xvalue.

Properties:

- Address of an rvalue may not be taken: &int(), &i++[[3\]](https://en.cppreference.com/w/cpp/language/value_category#cite_note-3), &42, and &std::move(x) are invalid.
- An rvalue can't be used as the left-hand operand of the built-in assignment or compound assignment operators.
- An rvalue may be used to [initialize a const lvalue reference](https://en.cppreference.com/w/cpp/language/reference_initialization), in which case the lifetime of the object identified by the rvalue is [extended](https://en.cppreference.com/w/cpp/language/reference_initialization#Lifetime_of_a_temporary) until the scope of the reference ends.

| An rvalue may be used to [initialize an rvalue reference](https://en.cppreference.com/w/cpp/language/reference_initialization), in which case the lifetime of the object identified by the rvalue is [extended](https://en.cppreference.com/w/cpp/language/reference_initialization#Lifetime_of_a_temporary) until the scope of the reference ends.When used as a function argument and when [two overloads](https://en.cppreference.com/w/cpp/language/overload_resolution) of the function are available, one taking rvalue reference parameter and the other taking lvalue reference to const parameter, an rvalue binds to the rvalue reference overload (thus, if both copy and move constructors are available, an rvalue argument invokes the [move constructor](https://en.cppreference.com/w/cpp/language/move_constructor), and likewise with copy and move assignment operators). | (since C++11) |
| ------------------------------------------------------------ | ------------- |
|                                                              |               |

## Special categories

### Pending member function call

The expressions a.mf and p->mf, where `mf` is a [non-static member function](https://en.cppreference.com/w/cpp/language/member_functions), and the expressions a.*pmf and p->*pmf, where `pmf` is a [pointer to member function](https://en.cppreference.com/w/cpp/language/pointer#Pointers_to_member_functions), are classified as prvalue expressions, but they cannot be used to initialize references, as function arguments, or for any purpose at all, except as the left-hand argument of the function call operator, e.g. (p->*pmf)(args).

### Void expressions

Function call expressions returning void, cast expressions to void, and [throw-expressions](https://en.cppreference.com/w/cpp/language/throw) are classified as prvalue expressions, but they cannot be used to initialize references or as function arguments. They can be used in discarded-value contexts (e.g. on a line of its own, as the left-hand operand of the comma operator, etc.) and in the returnstatement in a function returning void. In addition, throw-expressions may be used as the second and the third operands of the [conditional operator ?:](https://en.cppreference.com/w/cpp/language/operator_other).

| Void expressions have no *result object*. | (since C++17) |
| ----------------------------------------- | ------------- |
|                                           |               |

### Bit fields

An expression that designates a [bit field](https://en.cppreference.com/w/cpp/language/bit_field) (e.g. a.m, where `a` is an lvalue of type struct A { int m: 3; }) is an lvalue expression: it may be used as the left-hand operand of the assignment operator, but its address cannot be taken and a non-const lvalue reference cannot be bound to it. A const lvalue reference can be initialized from a bit-field lvalue, but a temporary copy of the bit-field will be made: it won't bind to the bit field directly.

## History

### CPL

The programming language [CPL](https://en.wikipedia.org/wiki/CPL_(programming_language)) was first to introduce value categories for expressions: all CPL expressions can be evaluated in "right-hand mode", but only certain kinds of expression are meaningful in "left-hand mode". When evaluated in right-hand mode, an expression is regarded as being a rule for the computation of a value (the right-hand value, or *rvalue*). When evaluated in left-hand mode an expression effectively gives an address (the left-hand value, or *lvalue*). "Left" and "Right" here stood for "left of assignment" and "right of assignment".

### C

The C programming language followed a similar taxonomy, except that the role of assignment was no longer significant: C expressions are categorized between "lvalue expressions" and others (functions and non-object values), where "lvalue" means an expression that identifies an object, a "locator value"[[4\]](https://en.cppreference.com/w/cpp/language/value_category#cite_note-4).

### C++98

Pre-2011 C++ followed the C model, but restored the name "rvalue" to non-lvalue expressions, made functions into lvalues, and added the rule that references can bind to lvalues, but only references to const can bind to rvalues. Several non-lvalue C expressions became lvalue expressions in C++.

### C++11

With the introduction of move semantics in C++11, value categories were redefined to characterize two independent properties of expressions[[5\]](https://en.cppreference.com/w/cpp/language/value_category#cite_note-5):

- *has identity*: it's possible to determine whether the expression refers to the same entity as another expression, such as by comparing addresses of the objects or the functions they identify (obtained directly or indirectly);
- *can be moved from*: [move constructor](https://en.cppreference.com/w/cpp/language/move_constructor), [move assignment operator](https://en.cppreference.com/w/cpp/language/move_assignment), or another function overload that implements move semantics can bind to the expression.

In C++11, expressions that:

- have identity and cannot be moved from are called *lvalue* expressions;
- have identity and can be moved from are called *xvalue* expressions;
- do not have identity and can be moved from are called *prvalue* ("pure rvalue") expressions;
- do not have identity and cannot be moved from are not used[[6\]](https://en.cppreference.com/w/cpp/language/value_category#cite_note-6).

The expressions that have identity are called "glvalue expressions" (glvalue stands for "generalized lvalue"). Both lvalues and xvalues are glvalue expressions.

The expressions that can be moved from are called "rvalue expressions". Both prvalues and xvalues are rvalue expressions.

### C++17

In C++17, [copy elision](https://en.cppreference.com/w/cpp/language/copy_elision) was made mandatory in some situations, and that required separation of prvalue expressions from the temporary objects initialized by them, resulting in the system we have today. Note that, in contrast with the C++11 scheme, prvalues are no longer moved from.

## Footnotes

1. [↑](https://en.cppreference.com/w/cpp/language/value_category#cite_ref-1) Assuming i has built-in type or the pre-increment operator is [overloaded](https://en.cppreference.com/w/cpp/language/operators) to return by lvalue reference.
2. ↑ [2.0](https://en.cppreference.com/w/cpp/language/value_category#cite_ref-pmfc_2-0) [2.1](https://en.cppreference.com/w/cpp/language/value_category#cite_ref-pmfc_2-1) [2.2](https://en.cppreference.com/w/cpp/language/value_category#cite_ref-pmfc_2-2) [2.3](https://en.cppreference.com/w/cpp/language/value_category#cite_ref-pmfc_2-3) Special rvalue category, see pending member function call.
3. [↑](https://en.cppreference.com/w/cpp/language/value_category#cite_ref-3) Assuming i has built-in type or the postincrement operator is not [overloaded](https://en.cppreference.com/w/cpp/language/operators) to return by lvalue reference.
4. [↑](https://en.cppreference.com/w/cpp/language/value_category#cite_ref-4) "A difference of opinion within the C community centered around the meaning of lvalue, one group considering an lvalue to be any kind of object locator, another group holding that an lvalue is meaningful on the left side of an assigning operator. The C89 Committee adopted the definition of lvalue as an object locator." -- ANSI C Rationale, 6.3.2.1/10.
5. [↑](https://en.cppreference.com/w/cpp/language/value_category#cite_ref-5) ["New" Value Terminology](http://www.stroustrup.com/terminology.pdf) by Bjarne Stroustrup, 2010.
6. [↑](https://en.cppreference.com/w/cpp/language/value_category#cite_ref-6) const prvalues (only allowed for class types) and const xvalues do not bind to T&& overloads, but they bind to the const T&& overloads, which are also classified as "move constructor" and "move assignment operator" by the standard, satisfying the definition of "can be moved from" for the purpose of this classification. However, such overloads cannot modify their arguments and are not used in practice; in their absence const prvalues and const xvalues bind to const T& overloads.

### Defect reports