# Go Language Fundamentals

## Go-Specific Best Practices

Follow these Go-specific guidelines for idiomatic and efficient Go code:

### Go Code Organization
- **Package naming**: Use short, lowercase package names without underscores
- **File naming**: Use lowercase with underscores for multi-word file names
- **Directory structure**: Organize packages in a logical hierarchy
- **Internal packages**: Use `internal/` directories to restrict package access
- **Cmd directory**: Place main packages in `cmd/` directory for executables

### Variable and Function Naming
- **Exported names**: Start with uppercase letter for public APIs
- **Unexported names**: Start with lowercase letter for internal use
- **Receiver names**: Use short, consistent receiver names (1-2 characters)
- **Interface naming**: Use `-er` suffix for single-method interfaces (Reader, Writer)
- **Acronyms**: Keep acronyms uppercase (HTTP, JSON, API)

### Go Idioms and Patterns
- **Error handling**: Always check errors explicitly, don't ignore them
- **Zero values**: Design types to be useful with their zero values
- **Composition over inheritance**: Use embedding and interfaces
- **Accept interfaces, return structs**: Make functions flexible with interface parameters
- **Goroutine management**: Always ensure goroutines can exit cleanly

### Memory Management
- **Pointer usage**: Use pointers for large structs and when mutation is needed
- **Slice operations**: Understand slice capacity and potential memory leaks
- **String concatenation**: Use strings.Builder for multiple concatenations
- **Defer statements**: Use defer for cleanup, but be aware of performance implications
- **Pool usage**: Use sync.Pool for frequently allocated objects

### Concurrency Best Practices
- **Channel usage**: Prefer channels for communication between goroutines
- **Select statements**: Use select for non-blocking channel operations
- **Context usage**: Use context.Context for cancellation and timeouts
- **Mutex vs channels**: Choose the right synchronization primitive
- **Race condition prevention**: Use go race detector during development

### Error Handling Patterns
- **Custom error types**: Implement error interface for domain-specific errors
- **Error wrapping**: Use fmt.Errorf with %w verb for error chains
- **Sentinel errors**: Define package-level error variables for common errors
- **Error checking**: Check errors immediately after function calls
- **Panic usage**: Reserve panic for truly exceptional circumstances
