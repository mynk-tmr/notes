_Variable_ : identifier for a data in memory. `const` variables can’t be bound to other data once they are initialised

_Binding_ : process of converting identifiers into addresses.

_Environment_ : collection of bindings and their values that exist at a given time.

Code Blocks {..} : define statements to be executed together.

_Keywords_: reserved words and cannot be used as a variable name.

_Expression_ : A fragment of code that produces a value.

_Statement_:  An expression with a (;) or newline at end and amounts to a single instruction.

_Sandboxing_ : Isolating a programming environment. For security reasons, JS is sandboxed by browsers


## SOLID Principles

**Single Responsibility** => a method/class/component [entity] should have a single reason to change i.e. what it's responsible for. Common violation -> tightly coupled objects

**Open/Closed** => an entity should be open for extension, but closed for modification.

**Liskov Substitution** => code should work even if objects of a superclass are substituted with objects of its subclass. Failing LSP usually mean bad inheritance ; common indicators to know violations - using instance of checks ; empty methods ; throwing errors that superclass can't

**Interface Segregation** => an entity shouldn’t be forced to implement or depend on methods it never uses. Violations -> very much like LSP ones. Solved via `object composition`

**Dependency Inversion** => high-level modules (complex logic) should not import anything from low-level modules (utilities)  and communicate via `abstract` interfaces (a wrapper around APIs).