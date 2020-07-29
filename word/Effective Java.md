# Effective Java  3rd（Java高效编程第三版）

## 																						Chapter 1

#### 																										Introduction

​	This book is designed to help you make effective use of the Java programming language and its funddamental libraries : java.lang,java.util,and java.io and subpackages such as java.util concurrent and java.util.funciton . Other libraries are discussed from time to time .

​	本书旨在帮助你高效使用Java编程语言和基本类库：java.lang包、java.util包和分包比如java.util.concurrent和java.util.function . 其他库也不时讨论。

​	This book consists of  ninety items,each of which conveys one rule . The rules capture practices generally held to be beneficial by the best and most experienced programmers . The items are loosely grouped into eleven chapters , each covering  one broad aspect  of software design. The book is not intended to be read from cover to cover : each items stands on its own , more or less . The items are heavily cross-referenced you can easily plot you course through the book.

​	本书由90个条例组成，每个条例讨论一个规则，这些规则反映了最有经验的优秀程序员，在实践中常见一些有益的做法。全书以松散的方式将这些条目组成11章，每个章节都涉及软件设计的一个主要方面。本书不需要按部就班的从头到尾的阅读，因为每个条目之间是相互独立的，每个条目交叉引用，通过本书你可以很容易学到你想要的内容。

​	Many new features  were added to  the platform since the last edition of this book was published. Most of the items in this book  use these features in some ways. This table shows you where to go for primary coverage of key features:

​	自从本书最后一版发布以来，Java平台已添加了许多新特性。本书中大多数章节中也使用新特性的方法。下表显示了主要功能的主要覆盖范围：

![1589554689814](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1589554689814.png)

​	Most items are illustrated with program examples . A key features of this book is that it contains code examples illustrating  many design patterns and idioms . Where appropriate , they are cross-referenced to the standard reference work in this area【 Gamma95 】.

​	许多条例中包含了程序示例。本书突出的特点，许多设计模式和习惯用法在代码实例中，在适当的情况下，它们是交叉引用与标准参照这个区域（Gamma95 ）。

​	Many items contain one or more program examples illustrating some practice to be avoided ,Such examples , sometimes known as antipatterns , are clearly labeled with a comment such as //**Never do this** . In each case , the item  explains why  the examples is bad and suggests an alternative approach.

​	许多条例中包含一个或者多个，在一些实践中应该避免的程序代码示例，像这样的例子，我们称之为“反模式”，清晰的标记了注释为“千万别这样做”，在每种情况下，条例中解释此例为什么不好，并且建议一个可供选择的方法。

​	This book is not for beginners: it assumes that  you are already comfortable with Java. If you are not , consider one of the many fine introductory texts ,such as  Peter Sestoft’s Java Precisely [Sestoft16]. While Effective Java designed to be accessible to anyone with a working knowledge of the  language ,it should provide food for thought even for advanced programmers.

​	本书不是针对初学者，它认为你已经掌握了Java编程语言。假如你不熟悉，请考虑 先参阅 本好的 Java 入门书籍，比如 Peter Sesto仕的《 Java Precisely > [ Sestoftl 6 本书适用于任何具有实际 Java 工作经验的程序员，对于高级程序员，也应该能够提供一些发人深思的东西。

​	Most of the rules  in this book derive from a few fundamental principles. Clarity and simplicity are of paramount importance. The user of component should never be surprised by its behavior .Components should be as small as possible but no smaller .(As used in this book , the term component refers to any reusable software element , from an individual method to a complex framework consisting of multiple packages. ) Code should be reused rather than copied . The dependencies between components should be to a kept minimum .  Errors should be  be detected as soon as possible after they are made , ideally at compile time.

​	本书上大多数规则源于少数的基本的原则。清晰与简洁是最为重要的，用户使用组件不应该被其行为所迷惑。组件应该尽可能的小，但是有不能太小。（本书中使用术语“组件”，是指任何可以复用的软件元素，从一个方法再到一个包含多个包复杂框架，都可以称作为“组件”）。代码应该复用而不是拷贝。组件之间的依赖也应该降到最小。错误应该尽可能早的发现，最好在编译的时候就解决问题。

​	While the rules in this book  do not apply 100 percent of the time . They do characterize best programming practices in the great majority of cases . You should not slavishly follow these rules , but violate them only occasionally and with good reason . Learning the art of programming , like most other disciplines consists of first learning the rules and then learning when to break the them. 

​	本书中的规则并不是任何场景都可以百分百应用。在大多数情况下，它们是最佳编程实践的特性。你不能一味的遵从这些规则，但是有充分理由的时候，可以打破规则。学习编程艺术，像大多数其他学科组成一样，先学习这些基本规则，然后在学会在什么时候打破规则。

​	For the most part , this book is not about performance . It is writing programs that are clear, correct,usable,robust,flexible, and maintainable . If you can do that , it's usually a relatively simple matter to get the performance you need (Item 67). Some items do discuss performance concerns . and a few of these items provide performance numbers .These numbers , which are introduced with the phrase "on my machine" , should be regarded as approximate the best.

​	本书大多数章节中，不是关于程序性能问题，它是让你写的程序更加清晰的，正确的，可用的，健壮的，灵活和可维护的。如果你能按照这样做，那么想获得需要的性能就会水到渠成了。有些条例也关注讨论性能问题，也提供一些性能指标。但是提及这些指标的时候，会使用“在我的机器上”，可以把这些指标视同为近似值。

​	 For what it’s worth, my machine is an aging homebuilt 3.5GHz quad-core Intel Core i7-4770K with 16 gigabytes of DDR3-1866 CL9 RAM, running Azul’s Zulu 9.0.0.15 release of OpenJDK, atop Microsoft Windows 7 Professional SP1 (64-bit). 

​	有必要提及的是，我的机器是一台过时的家用电脑， CPU 是四核 Intel Core i7-4770K,  主频 GHz ，内存 DDR3 1866 CL9 16G ，在 Micro so Windows 7 Professional SP 1 ( 64 位）操作系统平台上运行 Azu 公司发行的OpenJDK: Zulu 9.0.0.15 版本。

![1590415967010](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1590415967010.png)

​	The examples are reasonably complete , but favor readability over completeness .  They freely use classes from packages java.util and java.io .  In order to  compile examples , you may have to add one or more declarations , or other boilerplate . The book's website,   http://joshbloch.com/effectivejava,  contains an expended version of each example , which you can compile an run.

​	这些示例都比较完整，但是更加注重可读性更甚于完整性。它们可以直接使用java.util和java.io这些包中的类。为了编译这些示例，你可能不得不导入一个或者多个声明，或者其他样板代码。这本书的网站，http://joshbloch.com/effectivejava，每个示例中包含一些扩展的版本信息，你可以直接编译和运行它。

​	For the most part , this book uses technical terms  they  are defined in  The Java Language Specification, Java SE 8 Edition [JLS].  A few terms deserve special mention . The language supports four kinds of types : interface (including annotations ) , classes (including enums) , arrays , primitives . The first three are known as reference types . Class instances and arrays are objects , primitive values are not. A class's member consists of  its fields , methods,members classes , and members interfaces . A method's signature consist of      its name and the types of formal parameters , the signature's does not include the method's return types.

​	本书在大多数章节，使用技术术语定义与《The Java Language Specification, Java SE 8 Edition [JLS]》，有些术语值得我们特别提及。这门语言支持4中类型：接口（包含注解），类（包含枚举），数组，基本类型，前三个我们称作为引用类型。类实例和数组是对象类型，基本类型值不是。一个类成员组成有字段，方法，成员类，和成员接口。一个签名方法由名字和参数组成的，签名不包括方法的返回类型。

​	This book uses a few terms differently from The Java Language Specification .  Unlike The Java Language Specification , this book uses inheritance as a synonym of subclassing . Instead of uses the term inheritance for interfaces , this book simply states that a class implements an interface or that  one interface extends another. To describe the access level that applies when none is specified, this book uses the traditional packages private instead of the technically correct package access .

​	本书也使用了一些与 The Java Language Specification 不同的术语。例如，本书用术语“继承”作为“子类化”的同义词。本书不再使用“接口继承” 这种说 ，而是简单地说个类实现（ implement ）了个接口，或者一个接口扩展 ( extend ）了 另一个接 为了描述“在没有指定访问级别的情况下所使用的访问级别”，本 书使用了传统的描述性术语“包级私有”，而不是技术性术语“包级访问” 级别。

​	This book uses a few technical terms  that are not defined in The Java Language Specification . The term exported API , or simply API,refers in the classes , interfaces ,constructors ,members ,and serialized forms by which a programmer access a class, interfaces ,or package. (The terms API , which is short for Application programming interface, is used in preference to the otherwise preferable term interface to avoid confusion with the language construct of that name). A programmer who writes a program that uses an API is referred to a user of the API. A class whose implementation uses an API is a client of the API.

​	本书有一些技术术语没有定义在The Java Language Specification中，术语导出API，或者简单API，是指类、接口、构造器、成员和序列化形式，程序员可以通过访问类、接口或者包。术语API，是简称应用程序接口，这里之所以不使用API，是为了不与Java语言中的接口混淆。使用API编写程序的程序员被称为API的用户，在类实现中使用了API的类称为该API的客户端。

​	Classes, interfaces , constructors ,members and serialized forms are collectively knows as  API elements . An exports API consists of the API elements that are accessible outside of the package that defines the API. These are the API elements that any client can use and the author of the API elements commit to supports. Not coincidentally , they are also the elements for which the Javadoc utility generates documentation in its default mode of operation. Loosely speaking , the exported API of a package consists of the public and protected members and constructors of every public class or interface in the package.  

​	类、接口、构造器成员以及序列化形式被统称为 API 元素（ API element API 由所有可在定义该 PI 包之外访问的 PI 元素组成 任何客户端都可以使用这些 API 元素，API 的创建者则负责支持这些 API Javadoc 工具类在它的默认操作模式下也正是为这些元 生成文档，这绝非偶然 不严格地讲，个包的导出 API 是由该包中的每个公有public 类或者接口中所有公有的或者受保护 protected ）成员和构造器组成。

​	In Java 9 , a module system was  added to  the platform. If a library make uses of the module system, its exported API is the union of the exported APIs of all the packages exported by the library's module declaration.

​	在Java 9 中 新增了模块系统，如果类库使用了模块系统，其API就是类库 模块声明导出的所有包的 API 组合。

## Chapter 2

#### Creating and Destroying Objects 

（创建和销毁对象）	

​	This chapter concerns creating and destroying objects : when and how to create them , when and how to avoid creating them, how to ensure they are destroyed in a timely manner, and how to manage any cleanup actions that  must precede their destruction.

​	本章关注创建与销毁对象：何时以及如何创建对象，何时以及如何避免创建对象，如何及时安全的销毁对象，如何保证在销毁之前做各种进行清理动作。

##### Item 1: Consider static factory methods instead of constructors 

**(考虑用静态工厂方法代替构造器)**

​	The traditional way for a class to allow a client to obtain an instance is to provide a public constructors .There  is another technique that should be a part of every programmer's toolkit. A class can provide a  public static factory method, which is simply an static method that returns an instance of the class . Here's a  simple example from Boolean (the boxed primitive class for boolean). This method translates a boolean primitive value into a Boolean object reference :

​	对于类而言，为了让客户端获取自身的一个实例，传统方法就是一个类提供公有构造器。还有一种方法，也是程序员工具箱中应该占有一席之地。一个类能提供公有的静态工厂方法，它只是简单返回一个类实例的静态方法。这是来自Boolean简单示例（boolean基本类型装箱类），这个方法是基本类型的boolean值转换成对象引用型Boolean。

​	示例代码：

​	 public static Boolean valueOf(boolean b) { 

​			return b ? Boolean. TRUE : Boolean .FALSE; 

​	} 

​	Note that a static factory method is not the same as  the Factory Method  pattern from Design Patterns . The static factory method described in this item  has not direct equivalent in Design patterns.

​	注意静态工厂方法与设计模式中的工厂方法是不同的，本条目描述的静态工厂方法，不能直接等价于设计模式中的工厂方法。

​	A class can provide its clients with static factory method instead of , in addition to , public constructors . Providing a static factory  method instead of  a public constructors has  both advantages and disadvantages.

​	除了提供公有的构造器之外，还可以给客户端提供一个静态工厂方法。提供一个静态工厂方法代替公有构造器有利与弊。

​	**One advantage of static factory methods is that , unlike constructors,they have names .** If  the parameters to a constructors do not , in and of  themselves describe the object being returned , a static factory with a well-chosen name is easier to use and the resulting client code easier to read . For examples , the constructor BigInteger(int , int ,Random) , which returns a BigInteger that is probably prime , would have been better expressed as a static factory method named BigInteger.probablyPrime () .(This method was added in Java 4 ).

​	**第一个有利是静态工厂方法，不像构造器，它可以定义自己的方法名。**如果构造器参数本身没有确切描述正在被返回的对象，那么有名称静态工厂方法更容易使用，客户端代码也更具可读性。例如，构造器BigInteger（int，int, Random）方法，返回时一个BigInteger素数，我们用BigInteger.probablyPrime方法名，静态工厂方法表示，更为清楚。（在Java4的时候添加方法）。

​	A class can have only a single constructor with a given signature . Programmers have been know to get around restriction by providing two constructors whose parameter lists differ only in the order of their parameters types. This is  a  really bad idea . The user of such an API will never be able to remember which constructors is which and will end up calling the wrong one by mistake . People reading code that uses these constructors will not know what the code does without referring to the class documentation.

​	一个类只能指定一个签名的构造器，程序员知道如何避免这个限制：提供两个构造器，它们的参数列表只是参数类型顺序不同。这实际上不是好主意。用户使用API的时候，永远也记不住该用哪个构造器，最终调用错误的构造器。人们读代码和使用构造，如果没有写类的说明注释，往往不知所云。

​	Because they have names , static factory methods don't share restriction discussed in the previous paragraph .In cases where a class seems to require multiple constructors with the same signature , replace the constructors with static factory methods and carefully chosen names to highlight their differences.

​	由于静态工厂方法可以有名称，所以它们就不受上述限制。在很多情况下，一个类必须多个带有相同构造器时，就用静态工厂方法代替构造器，并且仔细地选择名称突显它们的不同。

​	**A  second advantage of static factory methods is that , unlike constructors , they are not required to creating a new object each time they're invoked .** This allows immutable classes (Item 17) to use preconstructed instances , or to cache instances as they're constructed , and dispense them repeatedly to avoid creating unnecessary duplicate objects . The Boolean.valueOf(boolean) method illustrates this technique : it never creates an object . This technique is similar to the Flyweight patterns . It can greatly improve performance if equivalent objects are requested often , especially if they are expensive to create .

​	**静态工厂方法与构造器不同的第二优势在于，不必在每次调用都创建一个新对象。**这使得不可变类使用预先构建的实例，或者使用将构建好的实例缓存起来，进行重复的使用，应该避免没必要创建重复对象。这个Boolean中valueOf方法说明这个技术：它重来不创建对象。这个技术有点类似设计模式中的享元模式，如果每次请求的对象都是相同对象，并且创建对象有特别大的代价，那么这项技术可以有非常大的性能提升。

​	The ability of static factory methods to return the same object form repeated invocations allows classes to maintain strict control over what instances exist at any time . Classes that do this are said to be instance-controlled . There are several reasons to written instance-controlled classes . Instance control allows a class to guarantee that it is a singleton or noninstantiable . Also , it allows an immutable value classes to make the guarantee that no two equal instances exist : a. equals (b) if and only if a == b . This is the basis of the Flyweight pattern . Enum types provide this guarantee .

​	静态工厂方法能够为重复调用请求返回相同的对象，这样有助于在任何时间运行严格控制实例的存在。这种类我们称作为实例受控的类。编写实例控制的类有几个原因。实例受控使得可以确保一个类是单例或者不可实例化，它使得不可变值的类，可以确保不会存在两个相同实例存在：只有a==b时，a. equal(b)才是true . 这个是基于享元模式的基础。枚举类也能够保证。

​	**A third advantage of static factory methods is that , unlike constructors , they are can return an object of any subtype of their return type.** This gives you greatly flexibility in choosing the class of the returned objects.

​	**静态工厂方法与构造器不同的第三优势在于，它可以返回任何该类的任何子类的对象。**这是给你在选择返回对象时，带来更大灵活性。

​	One application of this flexibility is that  an API can return object without making their classes public . Hiding implementation classes in this fashion leads of a very compact API. This technique lends itself to a interface-base frameworks (Item 20), where interfaces provide natural return types for static factory methods.

​	一个应用的灵活性是，API可以返回对象，也能使该对象不用让该类变成公有。隐藏实现类的比较领袖前沿写法，且可以使API非常紧凑。这种技术是基于接口基础框架，静态工厂方法在接口提供自然返回类型。

​	Prior to Java 8 , interfaces couldn't have static factory methods , By convention , static factory methods for an interfaces named Type were put in a noninstantiable  companion classes (Item 4) named Types. For example ,  the Java Collection framework has forty-five utility implementations of its interfaces , providing unmodifiable collections , synchronized collections and the like . Nearly all of these implementations are exported via static factory method in one noninstantiable class  (java.util.collections). The classes of the returned objects are all the nonpublic.

​	在Java8之前，接口不能有静态工厂方法。因此按照惯例，接口Type静态工厂方法被放置在不能实例化的伴生类Types中，例如，Java Collection 框架中有45个工具实现，提供不可修改集合、同步集合、等等，不可实例化的类java.util.collections中，几乎所有这些实现是通过静态工厂方法提供输出。这些类返回的对象都不是公有的。

​	The collections Framework API is much smaller than it would have been had it exported forty-five separate public classes , one for each convenience implementation . It is not just the bulk of the API that is reduced but the conceptual weight: the number and difficulty of the concepts that programmers must master in order to user the API .The programmers know that the returned object has precisely the API specified by its interface, so there is no need to read additional class documentation for the implementation class .Furthermore , using such a static factory method requires the client to refer to the returned object by interface rather than implementation class, which is generally good practice (Item 64).

​	在Collections框架API比导出45个分开的公有的类，要小得多，每个便利实现对应一个类。不仅仅是API体积上减少，也是概念意义上得减少。为了使用这个API，用户必须在数量和观念困难上减少。程序员知道接口返回对象，是特殊指定返回类型，所以这里不需要添加或阅读一个类文档。此外，使用这样静态工厂方法时，要求客户端引用接口返回的对象类型，而不是去使用实现类返回的对象，这通常是一个好的实践方式。

 	As of Java 8 , the restriction that interfaces cannot contain static methods was eliminated , so there is typically little reason to provide a noninstantiable companion class for an interface . Many public static members that would have been at home in such a class should instead be put in the interface itself. Note , however , that it may still be necessary to put the bulk of the implementation code behind these static methods in a separate package-private class . This is because Java 8 requires all static members of an interface to be public . Java 9 allows private static methods , but static fields and static member classes are still required to be public .

​	 由于Java 8中，接口消除了接口中不能包含静态方法限制，所以特别少原因在接口中提供不可实例化的伴生类。许多公有静态成员放在这种类中，可以使用接口替代。但是要注意，仍然有必要将这些静态方法背后的大部分实现代码， 单独放进一个包级私有的类中 。这个是因为Java 8 要求接口中静态成员必须是公有的，在Java 9 中允许私有的静态方法，但是静态字段和静态成员类，仍然要求是公有的。

​	**A fourth advantage of static factories is that the class of the returned object can vary from call to call as a function of the input parameters .** Any subtype of  the declared return type is permissible . The class of the returned object also can vary from release to release .

​	**静态工厂的第四大优势在于，所返回的对象的随着每次调用而发生变化，这取决于静态工厂方法输入参数值。**只要已声明返回类型的子类型，都是允许的。这个返回对象也能随版本发布变化而变化。

​	The Enumset class (Item 36) has no public constructors , only static factories .In the OpenJDK implementation , they return an instance of one of two subclasses , depending on the size of the underlying enum type : if it has sixty-four or fewer elements , as most enum types do , the static factories return a RegularEnumSet instance , which is backed by a single long;if the enum type has sixty-five or more elements , the factories return a JumboEnumSet instance , backed by a long array .

​	EnumSet （详见第 36 条）没有公有的构造器，只有静态工厂方法 OpenJDK 实现中，它们返回两种子类之一的 个实例，具体则取决于底层枚举类型的大小：如果它的元素有 64 个或者更少，就像大多数枚举类型一样，静态工厂方法就会返回一个 RegalarEumSet 实例，用单个 long 进行支持；如果枚举类型有 65 个或者更多元素，工厂就返回 JumboEnumSet实例，用一个 long 数组进行支持;

​	The existence of these two implementation classes is invisible to clients . If  RegularEnumSet ceased to offer performance  advantage for small enum types, it could be eliminated from a  future release with no ill effects . Similarly , a future release could add a third or fourth implementation of EnumSet if it prove beneficial for performance. Clients neither know nor care about  the class of the object they get back from the factory . they care only that is some subclass of  EnumSet.

​	客户端看不见这个类有两个实现的存在，如果RegularEnumSet 不能给更小枚举类提供性能优势，就可能在未来的版本上删除，不会有任何影响。同样地，如果能证明对性能有益，在未来版本中能添加第三甚至第四Enumset实现。客户端不知道也不关心他们在工厂对象中返回的类，他们只需要关注EnumSet的子类。

​	**A fifth advantage of static factories is that the class of the returned object need not exist when the class containing the method is written.** Such flexible static factory method s form the basic of service provider framework , like the Java Database Connectivity (JDBC) .A server provider framework is a system in which providers implement a service , and the system makes the implementations available to clients , decoupling the clients form the implementations .

​	**静态工厂第五优势在于，方法返回类所属对象，在编写包含该静态方法类时可以不存在。**这种灵活的静态工厂方法是形成服务提供框架基础，例如 Java JDBC设计，一个服务提供框架指的是系统，一个服务多个提供者，系统为客户端提供多个实现提供者，客户端也与实现提供者进行解耦。