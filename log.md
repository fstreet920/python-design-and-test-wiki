# Research & Ingestion Log

## 2026-04-23
- **Pattern:** [Command](./patterns/Command.md)
- **Testing Strategy:** [Command_Harness](./testing/Command_Harness.md)
- **Summary:** Initial ingestion. Researched Command Pattern for Python 3.12+ using structural typing.

## 2026-04-24
- **Patterns:** Full GoF Suite (22 Patterns).
- **Testing Strategies:** Corresponding [Harnesses](./testing/) for all patterns.
- **Summary:** Mass ingestion of all GoF design patterns and their specific test harnesses.

## 2026-04-25
- **Pytest Core Patterns:** [Factory_Fixture](./testing/Factory_Fixture.md), [Yield_Fixture](./testing/Yield_Fixture.md), [Parametrization](./testing/Parametrization.md), [Mock_Injection](./testing/Mock_Injection.md), [Property_Based_Testing](./testing/Property_Based_Testing.md), [Snapshot_Testing](./testing/Snapshot_Testing.md), [Object_Mother_Builder](./testing/Object_Mother_Builder.md), [BDD_GWT](./testing/BDD_GWT.md).
- **Summary:** Expanded the wiki to include foundational Python testing patterns.

## 2026-04-26 (Current)
- **Pythonic Idioms:** [Borg](./patterns/Borg.md), [Context_Manager](./patterns/Context_Manager.md), [Descriptor](./patterns/Descriptor.md), [Dependency_Injection](./patterns/Dependency_Injection.md).
- **Architectural Patterns:** [Repository](./patterns/Repository.md), [Unit_of_Work](./patterns/Unit_of_Work.md), [Service_Layer](./patterns/Service_Layer.md), [Specification](./patterns/Specification.md).
- **Harnesses:** [Borg_Harness](./testing/Borg_Harness.md), [Context_Manager_Harness](./testing/Context_Manager_Harness.md), [Descriptor_Harness](./testing/Descriptor_Harness.md), [Dependency_Injection_Harness](./testing/Dependency_Injection_Harness.md), [Repository_Harness](./testing/Repository_Harness.md), [Unit_of_Work_Harness](./testing/Unit_of_Work_Harness.md), [Service_Layer_Harness](./testing/Service_Layer_Harness.md), [Specification_Harness](./testing/Specification_Harness.md).
- **Summary:** Added language-specific patterns and high-level architectural patterns. The architectural additions focus on domain-persistence decoupling (Repository, UoW) and business rule encapsulation (Specification), leveraging Python 3.12's advanced typing features.
