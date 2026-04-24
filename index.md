# Python Design & Test Wiki

Welcome to the Python Design & Test Wiki. This wiki contains interlinked documentation for Python design patterns and their corresponding testing strategies.

## Design Patterns

### Creational
- [Abstract_Factory](./patterns/Abstract_Factory.md) - Provide an interface for creating families of related or dependent objects.
- [Builder](./patterns/Builder.md) - Separate the construction of a complex object from its representation.
- [Factory_Method](./patterns/Factory_Method.md) - Define an interface for creating an object, but let subclasses decide which class to instantiate.
- [Prototype](./patterns/Prototype.md) - Specify the kinds of objects to create using a prototypical instance.
- [Singleton](./patterns/Singleton.md) - Ensure a class only has one instance.

### Structural
- [Adapter](./patterns/Adapter.md) - Convert the interface of a class into another interface clients expect.
- [Bridge](./patterns/Bridge.md) - Decouple an abstraction from its implementation.
- [Composite](./patterns/Composite.md) - Compose objects into tree structures.
- [Decorator](./patterns/Decorator.md) - Attach additional responsibilities to an object dynamically.
- [Facade](./patterns/Facade.md) - Provide a unified interface to a set of interfaces in a subsystem.
- [Flyweight](./patterns/Flyweight.md) - Use sharing to support large numbers of fine-grained objects efficiently.
- [Proxy](./patterns/Proxy.md) - Provide a surrogate or placeholder for another object.

### Behavioral
- [Chain_of_Responsibility](./patterns/Chain_of_Responsibility.md) - Pass the request along a chain of handlers.
- [Command](./patterns/Command.md) - Encapsulate a request as an object.
- [Interpreter](./patterns/Interpreter.md) - Define a representation for a language's grammar.
- [Iterator](./patterns/Iterator.md) - Access elements of an aggregate object sequentially.
- [Mediator](./patterns/Mediator.md) - Encapsulate how a set of objects interact.
- [Memento](./patterns/Memento.md) - Capture and externalize an object's internal state.
- [Observer](./patterns/Observer.md) - Notify all dependents when an object changes state.
- [State](./patterns/State.md) - Alter behavior when internal state changes.
- [Strategy](./patterns/Strategy.md) - Define a family of algorithms and make them interchangeable.
- [Template_Method](./patterns/Template_Method.md) - Define the skeleton of an algorithm.
- [Visitor](./patterns/Visitor.md) - Represent an operation to be performed on elements of an object structure.

### Pythonic Idioms
Language-specific patterns that leverage Python's unique features.
- [Borg](./patterns/Borg.md) - Shared state among all instances (Monostate).
- [Context_Manager](./patterns/Context_Manager.md) - Resource management via the `with` statement.
- [Descriptor](./patterns/Descriptor.md) - Managed attribute access (get/set logic).
- [Dependency_Injection](./patterns/Dependency_Injection.md) - Decoupling via constructor or property injection.

## Testing Strategies

### Pytest Core Patterns
Structural and behavioral patterns for robust Python testing.
- [Factory_Fixture](./testing/Factory_Fixture.md) - Create multiple object instances with varying state.
- [Yield_Fixture](./testing/Yield_Fixture.md) - Manage resource lifecycles (RAII).
- [Parametrization](./testing/Parametrization.md) - Data-driven testing for interchangeable scenarios.
- [Mock_Injection](./testing/Mock_Injection.md) - Isolate units using Spies, Stubs, and Mocks.
- [Property_Based_Testing](./testing/Property_Based_Testing.md) - Generative testing for invariants and edge cases.
- [Snapshot_Testing](./testing/Snapshot_Testing.md) - Regression testing for complex data structures.
- [Object_Mother_Builder](./testing/Object_Mother_Builder.md) - Centralized and flexible test data generation.
- [BDD_GWT](./testing/BDD_GWT.md) - Behavioral structure mapping to business requirements.

### Design Pattern Harnesses
Specific Pytest implementation strategies for GoF and Pythonic patterns.
- [Abstract_Factory_Harness](./testing/Abstract_Factory_Harness.md), [Builder_Harness](./testing/Builder_Harness.md), [Factory_Method_Harness](./testing/Factory_Method_Harness.md), [Prototype_Harness](./testing/Prototype_Harness.md), [Singleton_Harness](./testing/Singleton_Harness.md)
- [Adapter_Harness](./testing/Adapter_Harness.md), [Bridge_Harness](./testing/Bridge_Harness.md), [Composite_Harness](./testing/Composite_Harness.md), [Decorator_Harness](./testing/Decorator_Harness.md), [Facade_Harness](./testing/Facade_Harness.md), [Flyweight_Harness](./testing/Flyweight_Harness.md), [Proxy_Harness](./testing/Proxy_Harness.md)
- [Chain_of_Responsibility_Harness](./testing/Chain_of_Responsibility_Harness.md), [Command_Harness](./testing/Command_Harness.md), [Interpreter_Harness](./testing/Interpreter_Harness.md), [Iterator_Harness](./testing/Iterator_Harness.md), [Mediator_Harness](./testing/Mediator_Harness.md), [Memento_Harness](./testing/Memento_Harness.md), [Observer_Harness](./testing/Observer_Harness.md), [State_Harness](./testing/State_Harness.md), [Strategy_Harness](./testing/Strategy_Harness.md), [Template_Method_Harness](./testing/Template_Method_Harness.md), [Visitor_Harness](./testing/Visitor_Harness.md)
- [Borg_Harness](./testing/Borg_Harness.md), [Context_Manager_Harness](./testing/Context_Manager_Harness.md), [Descriptor_Harness](./testing/Descriptor_Harness.md), [Dependency_Injection_Harness](./testing/Dependency_Injection_Harness.md)
