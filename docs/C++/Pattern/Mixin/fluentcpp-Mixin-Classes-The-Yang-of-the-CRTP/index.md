# fluentcpp [Mixin Classes: The Yang of the CRTP](https://www.fluentcpp.com/2017/12/12/mixin-classes-yang-crtp/)

> NOTE: 标题中的"Yang"的意思是: 阳，即太极的"阳"，阅读完原文的内容后可知: 在本文中，它的含义是: mixin classes，是CRTP的反面。其实就是mixin from below。并且原文中的配图，也是非常形象的:

![Mixin classes CRTP](https://www.fluentcpp.com/wp-content/uploads/2017/12/mixin_classes.jpg)

Now that we’re clear on [how the CRTP works](https://www.fluentcpp.com/2017/05/12/curiously-recurring-template-pattern/), let me share with you another technique involving templates that is complementary to the CRTP: **Mixin classes**. I learnt about mixin classes by watching Arthur O’Dwyer’s [Template Normal Programming](https://www.youtube.com/watch?v=vwrXHznaYLA) talk at CppCon (actually you can find them in the [slides](https://github.com/CppCon/CppCon2016/blob/master/Tutorials/Template Normal Programming%2C Part 2/Template Normal Programming%2C Part 2 - Arthur O'Dwyer - CppCon 2016.pdf) because they were skipped over during the presentation).

### Plugging a generic functionality over your type

The main usage of the CRTP is to [add a generic functionality](https://www.fluentcpp.com/2017/05/16/what-the-crtp-brings-to-code/) to a particular class. Mixin classes do that too.

Mixin classes are **template classes** that define a generic behaviour, and are designed to **inherit** from the type you wish to plug their functionality onto.

Here is an example. Let’s take a class representing the name of a person. It has a first name and a last name, and it can print out that name with a specific format:

```C++
class Name
{
public:
    Name(std::string firstName, std::string lastName)
      : firstName_(std::move(firstName))
      , lastName_(std::move(lastName)) {}
    
    void print() const
    {
        std::cout << lastName_ << ", " << firstName_ << '\n';
    }
    
private:
    std::string firstName_;
    std::string lastName_;
};
```

Here is a piece of code using it:

```
Name ned("Eddard", "Stark");
ned.print();
```

which outputs:

```
Stark, Eddard
```

Nothing spectacular so far, but here is a new requirement: we need to be able to print this name several times in a row.

We could add a `repeat` method to the `Name` class. But the concept of repeatedly call the `print` method is something that could apply to other classes, like a `PhoneNumber` class that could also have a `print()` method.

The idea of the mixin class is to isolate the generic functionality into its own class, template this class on the type we want to plug in onto, and derive from that type:

```C++
template<typename Printable>
struct RepeatPrint : Printable
{
    explicit RepeatPrint(Printable const& printable) : Printable(printable) {}
    void repeat(unsigned int n) const
    {
        while (n-- > 0)
        {
            this->print();
        }
    }
};
```

