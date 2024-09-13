**Why OOP?**
- helps in organizing code, making it easier to maintain and scale applications. 
- promotes code reusability, modularity, and flexibility
- enables developers to model real-world entities effectively

**OOPs** : programming model where logic is implemented in terms of “**OBJECTS**” , their characteristics and behavior. It's main principles are *encapsulation, inheritance, polymorphism, abstraction.*

**Class** : user-defined blueprint for objects. It represents properties or methods common to all objects of one type. 

**Class vs Structure**
- structure is saved in the stack memory, whereas class in HEAP.
- Data Abstraction cannot be achieved with structure

**Object** : basic unit of OOP with state & methods of its class and a unique identity. Objects communicate via *message passing* (request for method execution) without knowing internal details of each other

**Aggregation** : defines relationships between objects. eg. teacher has many students

**Association** : special type of association. Used to denote "HAS-A" relationship.

**Composition** : special association where contained object can't exist on its own.
##### Inheritance
- allows one class to inherit properties and behavior from another. 
- supports “reusability” and "extensibility"
- bad inheritance -> tight coupling of base & sub classes
- Types
	- **Single** : one super one sub
	- **Multi-level** : super itself sub of another
	- **Multiple**: multiple super one sub
	- **Hierarchical** : multiple sub-classes , 1 super
	- **Hybrid**: 

##### Encapsulation
- bind together **data** and **functions** that operate on them, in a single unit like a class to achieve *data hiding* (expose only relevant information)

##### Access specifiers
- special type of keywords to control accessibility of entities like classes, methods, etc. 
- significance : **Encapsulation**
- `public` : anywhere
- `protected`: subclasses
- `private`: only in class where it is defined

##### Abstraction
- hiding implementation details and simplifying interaction with outside world.
- obtained via encapsulation, interfaces & abstract classes
- *abstract classes* : that atleast 1 abstract method. They can't be instantiated. Must be inhertied.

##### Polymorphism
- ability of an entity to take many forms based on context. 
- 2 ways
	- **Overloading** allows methods/operators with same name, but different parameters, to be defined in a class. It enables *compile*-time or *static* binding.
	- **Overriding** allows sub-classes to modify behavior of super classes by redefining their methods. It enables *run*-time or *dynamic* binding.

**Constructors**
- special methods that initialize objects. 
- Destructors are special methods that are automatically called when an object is being destroyed
- Constructor types
	- *default* : no param
	- *parameterised*
	- *copy* : uses instance of same class to create another

| Class inheritance                                              | Object Composition                                                                             |
| :------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| creates **is-a** relationships viz restrictive                 | allows **has-a, uses-a, or can-do** relationships. simpler, more expressive, and more flexible |
| used when you decide objects based on what they are            | … based on what they do                                                                        |
| animal sees, dog is animal that barks, cat is animal that meow | animal is seer, dog is animal+barker, cat is animal+meower                                     |

**Static properties** : cannot be directly accessed on instances of class. Instead, they're accessed on the class itself.

##### SOLID Principles
- 5 design principles to write clean code and improve OOP practices.
- **Single Responsibility** : entity should have a single reason to change i.e. what it's responsible for
- **Open/Closed** : entity should be open for extension, but closed for modification.
- **Liskov Substitution** 
	- code should work even if *superclass* objects are replaced by subclass objects
	- Failing LSP usually mean bad inheritance
	- Fails : `instance of` ; empty methods ; throwing errors that superclass can't
- **Interface Segregation**  : entity shouldn’t implement or depend on methods it never uses. Violations -> like LSP. Solved via object composition
- **Dependency Inversion** : high-level modules (complex logic) should not import anything from low-level modules  and communicate via `interface`

##### Design patterns
- formalized best practices that programmer can use to solve common problems.

## Examples

**ENCAPSULATION & ABSTRACTION** 

```ts
class Task {
  readonly public completed : boolean;
  public complete() { this.completed = true}
  public incomplete() { this.completed = false }
}
```

**INHERITANCE**

```ts
abstract class Animal {
  constructor(protected name : string) {} //shortcut constructor
  abstract shout() : string;
}

interface Herbivore {
	eats() : void;
}

class Cow extends Animal implements Herbivore {
  constructor(name) { super(name) }
  public shout() { return 'moww' }
  public eats() { return 'grass' }
}
```

**POLYMORPHISM**

```ts
class Rectangle extends Shape {	
  calculateArea() {
    return this.width * this.height;
  }
}

class Circle extends Shape {
  calculateArea() {
    return Math.PI * this.radius ** 2;
  }
}

function calculateShapeArea(shape: Shape): number {
  return shape.calculateArea();
}

rect = calculateShapeArea(new Rectangle(5,4))
circ = calculateShapeArea(new Circle(4))
```