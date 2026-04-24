# Python Design & Test Wiki

An autonomous, interlinked knowledge base of modern Python design patterns and robust testing strategies. This repository follows the **LLM-Wiki** pattern, serving as a persistent reference for architectural excellence and verification patterns.

## 🚀 Core Mission
To provide a comprehensive library of design patterns (Classic GoF & Pythonic Idioms) paired with complementary Pytest harnesses, all implemented using strictly modern Python 3.12+ standards.

## 📚 Contents

### [Design Patterns](./index.md#design-patterns)
- **Creational (5)**: Abstract Factory, Builder, Factory Method, Prototype, Singleton.
- **Structural (7)**: Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy.
- **Behavioral (11)**: Command, Observer, Strategy, State, Visitor, and more.
- **Pythonic Idioms (4)**: Borg (Monostate), Context Manager, Descriptor, Dependency Injection.

### [Testing Strategies](./index.md#testing-strategies)
- **Pytest Core Patterns (8)**: Factory Fixture, Yield Fixture (RAII), Parametrization, Mock Injection, Property-Based Testing (Hypothesis), Snapshot Testing (Syrupy), Object Mother/Builder, BDD (Given-When-Then).
- **Design Pattern Harnesses (27)**: Specific implementation-level test strategies for every design pattern in the library.

## 🛠 Technical Standards
- **Python 3.12+**: Leveraging `typing.Protocol` for structural typing and PEP 695 for simplified generics.
- **Type Safety**: 100% type-hinted code following PEP 8 conventions.
- **Verification**: Every design pattern is cross-linked to a validated Pytest harness.
- **Architecture**: Decoupled, modular, and focused on maintainability.

## 📂 Structure
- `/patterns`: Structural, Behavioral, Creational, and Idiomatic pattern implementations.
- `/testing`: Pytest core patterns and design pattern-specific harnesses.
- `index.md`: The central navigational hub for the entire wiki.
- `log.md`: Append-only history of research, ingestion, and architectural decisions.
- `GEMINI.md`: Core mandates and foundational instructions for the librarian.

---
*Maintained by the Autonomous Python Librarian.*
