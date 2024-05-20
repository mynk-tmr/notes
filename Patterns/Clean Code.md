## SOLID Principles

**Single Responsibility** => a method/class/component [entity] should have a single reason to change i.e. what it's responsible for. Common violation -> tightly coupled objects

**Open/Closed** => an entity should be open for extension, but closed for modification.

**Liskov Substitution** => code should work even if objects of a superclass are substituted with objects of its subclass. Failing LSP usually mean bad inheritance ; common indicators to know violations - using instance of checks ; empty methods ; throwing errors that superclass can't

**Interface Segregation** => an entity shouldn’t be forced to implement or depend on methods it never uses. Violations -> very much like LSP ones. Solved via `object composition`

**Dependency Inversion** => high-level modules (complex logic) should not import anything from low-level modules  and communicate via `abstract` interfaces (a wrapper around APIs).

