# Core Coding Principles

## Fundamental Guidelines

When writing or reviewing code, always prioritize these core principles:

### 1. Clarity Over Cleverness
- Write code that is immediately understandable to other developers
- Avoid overly complex one-liners or obscure language features
- Choose descriptive variable and function names over short, cryptic ones
- If code requires explanation, consider refactoring for clarity

### 2. Consistency is Key
- Follow established coding conventions within the project
- Maintain consistent indentation, spacing, and formatting
- Use consistent naming patterns throughout the codebase
- Apply the same architectural patterns across similar components

### 3. Single Responsibility Principle
- Each function should do one thing well
- Classes should have a single, well-defined purpose
- Modules should be cohesive and focused
- Avoid functions that perform multiple unrelated tasks

### 4. DRY (Don't Repeat Yourself)
- Extract common functionality into reusable functions or modules
- Use configuration files or constants for repeated values
- Create abstractions for repeated patterns
- However, avoid premature abstraction - some duplication is acceptable

### 5. KISS (Keep It Simple, Stupid)
- Choose the simplest solution that meets requirements
- Avoid over-engineering or unnecessary complexity
- Prefer straightforward algorithms over complex optimizations unless proven necessary
- Simple code is easier to debug, test, and maintain
