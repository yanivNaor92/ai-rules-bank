# Documentation and Comments

## Documentation Philosophy

Good documentation explains the "why" behind code decisions, not just the "what":

### When to Comment
- **Complex algorithms**: Explain the approach and reasoning
- **Business logic**: Document domain-specific rules and requirements
- **Non-obvious code**: Clarify anything that might confuse future developers
- **Workarounds**: Explain why unusual solutions were necessary
- **API interfaces**: Document parameters, return values, and side effects

### When NOT to Comment
- **Obvious code**: Don't comment what the code clearly shows
- **Bad code**: Fix the code instead of explaining why it's confusing
- **Outdated information**: Remove or update stale comments immediately

### Comment Quality Guidelines
- Write comments in clear, grammatically correct language
- Keep comments concise but complete
- Update comments when code changes
- Use consistent formatting and style
- Avoid redundant comments that just restate the code

### Function and Method Documentation
- Document the purpose and behavior
- Specify parameters and their types/constraints
- Describe return values and possible exceptions
- Include usage examples for complex functions
- Document side effects and dependencies

### Code Organization Documentation
- Use header comments to explain file/module purpose
- Document class responsibilities and relationships
- Explain architectural decisions and trade-offs
- Include TODO comments for known improvements needed

### README and Project Documentation
- Provide clear setup and installation instructions
- Explain the project's purpose and scope
- Document configuration options and environment variables
- Include examples of common usage patterns
- Maintain a changelog for significant updates

### Documentation Maintenance
- Review and update documentation during code reviews
- Remove obsolete documentation promptly
- Keep documentation close to the code it describes
- Use automated tools to validate documentation when possible
