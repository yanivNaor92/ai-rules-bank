# Naming Conventions

## General Naming Guidelines

Use clear, descriptive names that communicate intent and purpose:

### Variables and Functions
- Use descriptive names that explain what the variable holds or what the function does
- Prefer `calculateTotalPrice()` over `calc()` or `doStuff()`
- Use `userEmail` instead of `e` or `data`
- Boolean variables should be questions: `isValid`, `hasPermission`, `canEdit`

### Constants
- Use UPPER_SNAKE_CASE for constants: `MAX_RETRY_ATTEMPTS`, `DEFAULT_TIMEOUT`
- Group related constants in enums or constant objects when appropriate

### Classes and Types
- Use PascalCase for class names: `UserManager`, `PaymentProcessor`
- Choose names that represent the entity or concept clearly
- Avoid generic names like `Manager`, `Handler`, `Utility` unless truly appropriate

### Files and Directories
- Use consistent naming patterns within the project
- Prefer kebab-case or snake_case for file names depending on project conventions
- Make file names descriptive of their contents: `user-authentication.js`, `payment_processor.py`

### Avoid These Common Mistakes
- Single letter variables (except for short loop counters)
- Abbreviations that aren't universally understood
- Misleading names that don't match the actual functionality
- Names with numbers unless they have semantic meaning
- Hungarian notation or other outdated naming schemes

### Context-Specific Guidelines
- Use domain-specific terminology that your team understands
- Be consistent with existing codebase conventions
- Consider the scope - shorter names are acceptable for smaller scopes
- Use searchable names for important concepts
