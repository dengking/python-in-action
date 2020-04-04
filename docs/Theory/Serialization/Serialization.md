# Serialization

为了使用哪种programming language，都可能会涉及到serialization问题，本文对此进行总结。

## 维基百科[Serialization](https://en.wikipedia.org/wiki/Serialization)

In [computer science](https://en.wikipedia.org/wiki/Computer_science), in the context of data storage, **serialization** (or serialisation) is the process of translating [data structures](https://en.wikipedia.org/wiki/Data_structure) or [object](https://en.wikipedia.org/wiki/Object_(computer_science)) state into a format that can be **stored** (for example, in a [file](https://en.wikipedia.org/wiki/Computer_file) or memory [buffer](https://en.wikipedia.org/wiki/Data_buffer)) or **transmitted** (for example, across a [network](https://en.wikipedia.org/wiki/Computer_network) connection link) and **reconstructed** later (possibly in a different computer environment). When the resulting series of bits is reread according to the [**serialization format**](https://en.wikipedia.org/wiki/Category:Data_serialization_formats), it can be used to create a semantically identical clone of the original object. For many complex objects, such as those that make extensive use of [references](https://en.wikipedia.org/wiki/Reference_(computer_science)), this process is not straightforward. Serialization of object-oriented [objects](https://en.wikipedia.org/wiki/Object_(computer_science)) does not include any of their associated [methods](https://en.wikipedia.org/wiki/Method_(computer_science)) with which they were previously linked.

This process of serializing an object is also called [marshalling](https://en.wikipedia.org/wiki/Marshalling_(computer_science)) an object. The opposite operation, extracting a data structure from a series of bytes, is **deserialization** (also called **unmarshalling**).

### Uses

- A method of transferring data through the wires ([messaging](https://en.wikipedia.org/wiki/Messaging)).
- A method of storing data (in [databases](https://en.wikipedia.org/wiki/Database), on [hard disk drives](https://en.wikipedia.org/wiki/Hard_disk_drive)).
- A method of [remote procedure calls](https://en.wikipedia.org/wiki/Remote_procedure_call), e.g., as in [SOAP](https://en.wikipedia.org/wiki/SOAP).
- A method for distributing objects, especially in [component-based software engineering](https://en.wikipedia.org/wiki/Component-based_software_engineering) such as [COM](https://en.wikipedia.org/wiki/Component_Object_Model), [CORBA](https://en.wikipedia.org/wiki/CORBA), etc.
- A method for detecting changes in time-varying data.

For some of these features to be useful, **architecture independence** must be maintained. For example, for maximal use of distribution, a computer running on a different hardware architecture should be able to reliably reconstruct a **serialized data stream**, regardless of [endianness](https://en.wikipedia.org/wiki/Endianness). This means that the simpler and faster procedure of directly copying the memory layout of the data structure cannot work reliably for all architectures. Serializing the data structure in an **architecture-independent format** means preventing the problems of [byte ordering](https://en.wikipedia.org/wiki/Byte_ordering), memory layout, or simply different ways of representing data structures in different [programming languages](https://en.wikipedia.org/wiki/Programming_language).

Inherent to any serialization scheme is that, because the encoding of the data is by definition serial, extracting one part of the serialized data structure requires that the entire object be read from start to end, and reconstructed. In many applications, this linearity(线性) is an asset(优点), because it enables simple, common I/O interfaces to be utilized to hold and pass on the state of an object. In applications where higher performance is an issue, it can make sense to expend more effort to deal with a more complex, non-linear storage organization.

Even on a single machine, primitive [pointer](https://en.wikipedia.org/wiki/Pointer_(computer_programming)) objects are too fragile to save because the objects to which they point may be reloaded to a different location in memory. To deal with this, the serialization process includes a step called *unswizzling* or *pointer unswizzling*, where direct pointer references are converted to references based on name or position. The deserialization process includes an inverse step called *pointer swizzling*.

Since both serializing and deserializing can be driven from common code (for example, the *Serialize* function in [Microsoft Foundation Classes](https://en.wikipedia.org/wiki/Microsoft_Foundation_Classes)), it is possible for the common code to do both at the same time, and thus, 1) detect differences between the objects being serialized and their prior copies, and 2) provide the input for the next such detection. It is not necessary to actually build the prior copy because differences can be detected on the fly. The technique is called [differential execution](https://en.wikipedia.org/w/index.php?title=Differential_execution&action=edit&redlink=1). This is useful in the programming of user interfaces whose contents are time-varying — graphical objects can be created, removed, altered, or made to handle input events without necessarily having to write separate code to do those things.



## Implementation

比如[`pickle`](https://docs.python.org/3/library/pickle.html#module-pickle) — Python object serialization[¶](https://docs.python.org/3/library/pickle.html#module-pickle)。



## binary serialization vs protoc-buff

在ust项目中，直接使用的binary serialization，为每种请求都涉及一个`struct`，然后client和server之间就使用`struct`来作为protocol。显然这种实现方式是最最高效的，但是这种实现方式所带来的一个问题是：每次新增一个请求，就涉及到client和server的全部的修改。而不是像普通的协议那样。与此相关的是redis的协议、http协议。

https://stackoverflow.com/questions/2966500/protobuf-net-not-faster-than-binary-serialization

https://theburningmonk.com/2011/08/performance-test-binaryformatter-vs-protobuf-net/