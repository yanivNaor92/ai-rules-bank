# Go Performance Optimization

## Go-Specific Performance Guidelines

Optimize Go code while maintaining readability and Go idioms:

### Memory Allocation Optimization
- **Avoid unnecessary allocations**: Reuse slices and maps when possible
- **Pre-allocate slices**: Use `make([]T, 0, capacity)` when size is known
- **String builder**: Use `strings.Builder` instead of string concatenation in loops
- **Byte slices**: Prefer `[]byte` over `string` for mutable text operations
- **Struct field ordering**: Order fields by size (largest first) to minimize padding

### Slice and Map Performance
- **Slice capacity**: Set appropriate initial capacity to avoid reallocations
- **Map pre-sizing**: Use `make(map[K]V, size)` when approximate size is known
- **Slice vs array**: Use arrays for fixed-size data to avoid heap allocation
- **Copy vs append**: Use `copy()` for known-size slice operations
- **Map key types**: Use comparable types as map keys for better performance

### Goroutine and Channel Optimization
- **Goroutine pooling**: Limit goroutine creation for high-frequency operations
- **Buffered channels**: Use buffered channels to reduce goroutine blocking
- **Channel direction**: Use directional channels to clarify intent and enable optimizations
- **Select with default**: Use default case in select to avoid blocking
- **Worker pools**: Implement worker pools for CPU-intensive tasks

### Interface and Method Performance
- **Interface assertions**: Cache type assertions when used repeatedly
- **Method receivers**: Use pointer receivers for large structs or when mutation is needed
- **Interface size**: Keep interfaces small and focused
- **Avoid boxing**: Be aware of interface{} boxing overhead
- **Method inlining**: Write small methods that can be inlined by the compiler

### I/O and Network Performance
- **Buffered I/O**: Use `bufio` package for frequent small reads/writes
- **Connection pooling**: Reuse HTTP connections with proper client configuration
- **Context timeouts**: Set appropriate timeouts for network operations
- **Streaming**: Process large data sets in streams rather than loading entirely
- **Compression**: Use compression for network data when appropriate

### Profiling and Benchmarking
- **pprof usage**: Use `go tool pprof` to identify performance bottlenecks
- **Benchmark tests**: Write benchmark tests using `testing.B`
- **CPU profiling**: Profile CPU usage to find hot paths
- **Memory profiling**: Identify memory allocation patterns and leaks
- **Trace analysis**: Use `go tool trace` for detailed execution analysis

### Compiler Optimizations
- **Build tags**: Use build tags for platform-specific optimizations
- **Escape analysis**: Understand when variables escape to the heap
- **Inlining**: Write functions that can be inlined by the compiler
- **Dead code elimination**: Remove unused code and imports
- **Link-time optimization**: Use appropriate build flags for production builds
