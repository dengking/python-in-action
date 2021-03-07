# 关于本章

对于大型项目，如何进行组织？本章对这个问题进行探讨。本章的编写思路如下：我们使用一种programming language编写program的过程可以看做是使用这门programming language所提供的各种construct（或者说是entity）进行定义、引用的过程，在进行定义的时候，我们会指定一个“name”，然后通过这个name来引用这个定义好的construct。对于大型的project，可以预知的是，它会涉及到成千上万的name，那如何来对这些name进行组织管理呢？其实这就是本章所要讨论的对program的组织，可以认为它是对program中所定义的name进行组织。本章首先讨论name的概念，然后讨论各种programming language中使用到的管理方式。

目前我们所遇到的programming language大多数都是formal language，都存在compile过程，知道了programming language的organization，能够帮助我们理解它的compile过程，进而快速地诊断出相关的compile error。在compile过程中，name lookup是基于organization的，所以本章也会对name lookup进行描述。

本章所讨论的内容是建立在多种programming language之上的、通用的内容，关于各种programming language的具体的内容，在各个语言的章节中，会进行具体的分析。