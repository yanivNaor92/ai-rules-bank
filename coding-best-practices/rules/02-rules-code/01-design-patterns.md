# Design Patterns and Architecture

## Architectural Principles

Apply these fundamental design principles when writing code:

### SOLID Principles
- **Single Responsibility**: Each class should have one reason to change
- **Open/Closed**: Open for extension, closed for modification
- **Liskov Substitution**: Subtypes must be substitutable for their base types
- **Interface Segregation**: Clients shouldn't depend on interfaces they don't use
- **Dependency Inversion**: Depend on abstractions, not concretions

### Common Design Patterns
- **Factory Pattern**: Use for object creation when the exact type isn't known until runtime
- **Observer Pattern**: Implement for event-driven architectures and loose coupling
- **Strategy Pattern**: Encapsulate algorithms and make them interchangeable
- **Decorator Pattern**: Add behavior to objects without altering their structure
- **Repository Pattern**: Abstract data access logic from business logic

### Code Organization
- **Separation of Concerns**: Keep different aspects of functionality separate
- **Layered Architecture**: Organize code into logical layers (presentation, business, data)
- **Modular Design**: Create self-contained modules with clear interfaces
- **Dependency Injection**: Inject dependencies rather than creating them internally
- **Configuration Management**: Externalize configuration from code

### Function and Method Design
- Keep functions small and focused on a single task
- Limit function parameters (prefer objects for multiple parameters)
- Use pure functions when possible (no side effects)
- Return early to reduce nesting and improve readability
- Prefer composition over inheritance

### Class Design Guidelines
- Favor composition over inheritance
- Use interfaces to define contracts
- Keep class hierarchies shallow
- Encapsulate data and provide controlled access
- Make classes immutable when possible

### API Design
- Design APIs that are intuitive and consistent
- Use meaningful HTTP status codes for REST APIs
- Version your APIs from the beginning
- Provide comprehensive error responses
- Follow established conventions for your platform/language
