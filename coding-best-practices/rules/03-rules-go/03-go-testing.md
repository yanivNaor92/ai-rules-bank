# Go Testing Best Practices

## Go Testing Guidelines

Follow Go-specific testing patterns and conventions:

### Test Organization
- **Test file naming**: Use `_test.go` suffix for test files
- **Test function naming**: Start with `Test` followed by the function name being tested
- **Package testing**: Use same package name with `_test` suffix for white-box testing
- **Separate test packages**: Use different package for black-box testing
- **Table-driven tests**: Use table-driven tests for multiple test cases

### Writing Effective Tests
- **Test function signature**: Use `func TestXxx(t *testing.T)` pattern
- **Subtests**: Use `t.Run()` for organizing related test cases
- **Test helpers**: Create helper functions and mark them with `t.Helper()`
- **Test data**: Use testdata directory for test fixtures and data files
- **Golden files**: Use golden file pattern for complex output testing

### Benchmark Testing
- **Benchmark naming**: Start benchmark functions with `Benchmark`
- **Benchmark signature**: Use `func BenchmarkXxx(b *testing.B)` pattern
- **Reset timer**: Use `b.ResetTimer()` after setup code
- **Parallel benchmarks**: Use `b.RunParallel()` for concurrent benchmarks
- **Memory benchmarks**: Use `b.ReportAllocs()` to track allocations

### Test Utilities and Mocking
- **Testify package**: Consider using testify for assertions and mocks
- **Interface mocking**: Create mock implementations of interfaces for testing
- **Dependency injection**: Design code to accept interfaces for easy mocking
- **Test doubles**: Use stubs, mocks, and fakes appropriately
- **HTTP testing**: Use `httptest` package for HTTP handler testing

### Integration and End-to-End Testing
- **Build tags**: Use build tags to separate unit and integration tests
- **Test containers**: Use Docker containers for integration testing
- **Database testing**: Use test databases or in-memory alternatives
- **External dependencies**: Mock or stub external services
- **Environment setup**: Use `TestMain` for test environment setup/teardown

### Test Coverage and Quality
- **Coverage analysis**: Use `go test -cover` to measure test coverage
- **Coverage reports**: Generate HTML coverage reports for detailed analysis
- **Race detection**: Run tests with `-race` flag to detect race conditions
- **Fuzzing**: Use Go's built-in fuzzing for property-based testing
- **Test documentation**: Document complex test scenarios and edge cases

### Common Testing Patterns
- **Setup and teardown**: Use defer for cleanup in tests
- **Temporary files**: Use `t.TempDir()` for temporary test directories
- **Parallel tests**: Use `t.Parallel()` for independent tests
- **Skip tests**: Use `t.Skip()` for conditional test skipping
- **Error testing**: Test both success and error paths thoroughly

### Testing Anti-patterns to Avoid
- **Testing implementation details**: Focus on behavior, not implementation
- **Brittle tests**: Avoid tests that break with minor code changes
- **Slow tests**: Keep unit tests fast and move slow tests to integration suite
- **Flaky tests**: Eliminate non-deterministic test behavior
- **Over-mocking**: Don't mock everything; test real interactions when possible
