# Python Design & Test Wiki

Welcome to the Python Design & Test Wiki. This wiki contains interlinked documentation for Python design patterns and their corresponding testing strategies.

## Design Patterns

### Creational
- [[Abstract_Factory]] - Provide an interface for creating families of related or dependent objects.
- [[Builder]] - Separate the construction of a complex object from its representation.
- [[Factory_Method]] - Define an interface for creating an object, but let subclasses decide which class to instantiate.
- [[Prototype]] - Specify the kinds of objects to create using a prototypical instance.
- [[Singleton]] - Ensure a class only has one instance.

### Structural
- [[Adapter]] - Convert the interface of a class into another interface clients expect.
- [[Bridge]] - Decouple an abstraction from its implementation.
- [[Composite]] - Compose objects into tree structures.
- [[Decorator]] - Attach additional responsibilities to an object dynamically.
- [[Facade]] - Provide a unified interface to a set of interfaces in a subsystem.
- [[Flyweight]] - Use sharing to support large numbers of fine-grained objects efficiently.
- [[Proxy]] - Provide a surrogate or placeholder for another object.

### Behavioral
- [[Chain_of_Responsibility]] - Pass the request along a chain of handlers.
- [[Command]] - Encapsulate a request as an object.
- [[Interpreter]] - Define a representation for a language's grammar.
- [[Iterator]] - Access elements of an aggregate object sequentially.
- [[Mediator]] - Encapsulate how a set of objects interact.
- [[Memento]] - Capture and externalize an object's internal state.
- [[Observer]] - Notify all dependents when an object changes state.
- [[State]] - Alter behavior when internal state changes.
- [[Strategy]] - Define a family of algorithms and make them interchangeable.
- [[Template_Method]] - Define the skeleton of an algorithm.
- [[Visitor]] - Represent an operation to be performed on elements of an object structure.

### Pythonic Idioms
Language-specific patterns that leverage Python's unique features.
- [[Borg]] - Shared state among all instances (Monostate).
- [[Context_Manager]] - Resource management via the `with` statement.
- [[Descriptor]] - Managed attribute access (get/set logic).
- [[Dependency_Injection]] - Decoupling via constructor or property injection.

## Testing Strategies

### Pytest Core Patterns
Structural and behavioral patterns for robust Python testing.
- [[Factory_Fixture]] - Create multiple object instances with varying state.
- [[Yield_Fixture]] - Manage resource lifecycles (RAII).
- [[Parametrization]] - Data-driven testing for interchangeable scenarios.
- [[Mock_Injection]] - Isolate units using Spies, Stubs, and Mocks.
- [[Property_Based_Testing]] - Generative testing for invariants and edge cases.
- [[Snapshot_Testing]] - Regression testing for complex data structures.
- [[Object_Mother_Builder]] - Centralized and flexible test data generation.
- [[BDD_GWT]] - Behavioral structure mapping to business requirements.

### Design Pattern Harnesses
Specific Pytest implementation strategies for GoF and Pythonic patterns.
- [[Abstract_Factory_Harness]], [[Builder_Harness]], [[Factory_Method_Harness]], [[Prototype_Harness]], [[Singleton_Harness]]
- [[Adapter_Harness]], [[Bridge_Harness]], [[Composite_Harness]], [[Decorator_Harness]], [[Facade_Harness]], [[Flyweight_Harness]], [[Proxy_Harness]]
- [[Chain_of_Responsibility_Harness]], [[Command_Harness]], [[Interpreter_Harness]], [[Iterator_Harness]], [[Mediator_Harness]], [[Memento_Harness]], [[Observer_Harness]], [[State_Harness]], [[Strategy_Harness]], [[Template_Method_Harness]], [[Visitor_Harness]]
- [[Borg_Harness]], [[Context_Manager_Harness]], [[Descriptor_Harness]], [[Dependency_Injection_Harness]]
