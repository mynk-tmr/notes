OOPs : programming model where logic is implemented in terms of “**OBJECTS**” , their characteristics and behavior. It's **main principles** are encapsulation, inheritance, polymorphism, and abstraction.

- Encapsulation ensures that the internal *state* of an object is hidden and can only be accessed through *public* methods. 
- Inheritance allows one class to inherit properties and behavior from another. Inheritance supports “reusability” and "extensibility"
- Polymorphism : ability of an entity to take many forms based on context. 
- Abstraction focuses on hiding implementation details and showing only necessary information to the outside world.

### Why OOP?

OOPs helps in organizing code, making it easier to maintain and scale applications. It also promotes code reusability, modularity, and flexibility. OOPs enables developers to model real-world entities effectively

OOPs is implemented in Java using classes, objects and principles of OOP

## Encapsulation

Bind together **data** and **functions** that operate on them in a single unit like a class to achieve *data hiding* (expose only relevant information)

A [class](https://www.geeksforgeeks.org/classes-objects-java) is a user-defined blueprint for objects. It represents the set of properties or methods that are common to all objects of one type. 

An object is basic unit of OOP with state & methods of its class and a unique identity. An object is necessary only if class has non-static methods.

Constructors are special methods that initialize objects. 

Destructors are special methods that are automatically called when an object is being destroyed. Through garbage collection, unwanted memory is freed up by removing the objects that are no longer needed.

Constructor types
- *default* : no param
- *parameterised*
- *copy* : uses instance of same class to create another

**Class vs Structure**
- structure is saved in the stack memory, whereas the class is saved in the heap memory.
- Data Abstraction cannot be achieved with structure

### Access specifiers
- special type of keywords to control accessibility of entities like classes, methods, etc. 
- significance : **Encapsulation**
- 4 types in Java
	- `public` : anywhere
	- `protected`: package, subclass including outside package
	- `private`: only in class where it is defined
	- `default`: same class & package


**Message Passing** : by which objects communicate. A message is a request for method execution in receiving object. It involves specifying object name, method name and info to be sent.

## Inheritance

bad inheritance -> tight coupling of base & sub classes

| Class inheritance                                              | Object Composition                                                                             |
| :------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| creates **is-a** relationships viz restrictive                 | allows **has-a, uses-a, or can-do** relationships. simpler, more expressive, and more flexible |
| used when you decide objects based on what they are            | … based on what they do                                                                        |
| animal sees, dog is animal that barks, cat is animal that meow | animal is seer, dog is seer+barker, cat is seer+meower                                         |

The various types of inheritance include:
- **Single** : one super one sub
- **Multiple**: multiple super one sub
- **Multi-level** : super itself sub of another
- **Hierarchical** : multiple sub-classes , 1 super
- **Hybrid**: 

## Abstraction

Data abstraction is accomplished with the help of [interfaces](https://www.geeksforgeeks.org/interfaces-in-java) and [abstract classes](https://www.geeksforgeeks.org/abstract-classes-in-java). Both are special types of classes that contain abstract methods (only signature, no implementation)

**Interface vs Abstract class**
- Abstract class can have non-abstract methods, interfaces do not
- To use an interface, you cannot create objects. You must implement it via a class
- We can achieve 100% abstraction using interfaces.

## Polymorphism

In Java, entities with same name can be used in different ways using *overloading* or *overriding*.

**Overloading** allows methods/operators with same name, but different parameters, to be defined for a single class. It enables *compile*-time or *static* binding.

**Overriding** allows child classes to modify behavior of parent classes by redefining their methods. It enables *run*-time or *dynamic* binding.

## Exceptions

An exception is a special event, which is raised during runtime, that brings the execution to a halt. To handle them and prevent halting, we do exception handling. We identify undesirable states that program can reach and specifying the desirable outcomes of such states.  Try-catch is the most common method.

https://www.javatpoint.com/job-interview-questions