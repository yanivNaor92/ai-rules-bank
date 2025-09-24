# Performance and Optimization

## Performance Guidelines

Write efficient code while maintaining readability and maintainability:

### General Performance Principles
- **Measure before optimizing**: Use profiling tools to identify actual bottlenecks
- **Optimize the right things**: Focus on code that runs frequently or processes large data
- **Consider algorithmic complexity**: Choose appropriate data structures and algorithms
- **Cache expensive operations**: Store results of costly computations when appropriate
- **Lazy loading**: Load resources only when needed

### Data Structure Selection
- **Arrays vs Lists**: Use arrays for fixed-size, indexed access; lists for dynamic sizing
- **Hash tables**: Excellent for key-value lookups with O(1) average performance
- **Trees**: Use for sorted data and range queries
- **Sets**: Efficient for membership testing and eliminating duplicates
- **Queues/Stacks**: Choose based on access patterns (FIFO vs LIFO)

### Memory Management
- **Avoid memory leaks**: Clean up resources, remove event listeners, close connections
- **Object pooling**: Reuse objects for frequently created/destroyed instances
- **Minimize allocations**: Reduce garbage collection pressure in managed languages
- **Use appropriate data types**: Choose the smallest type that meets requirements
- **Stream processing**: Process large datasets in chunks rather than loading entirely

### Database and I/O Optimization
- **Use indexes effectively**: Ensure queries can leverage database indexes
- **Batch operations**: Group multiple database operations when possible
- **Connection pooling**: Reuse database connections rather than creating new ones
- **Async I/O**: Use non-blocking I/O for network and file operations
- **Pagination**: Limit result sets for large data queries

### Code-Level Optimizations
- **Avoid premature optimization**: Write clear code first, optimize when needed
- **Minimize function calls in loops**: Move invariant calculations outside loops
- **Use built-in functions**: Language/library implementations are usually optimized
- **Short-circuit evaluation**: Structure boolean expressions for early termination
- **String operations**: Use StringBuilder/StringBuffer for multiple concatenations

### Caching Strategies
- **Application-level caching**: Cache computed results in memory
- **HTTP caching**: Use appropriate cache headers for web applications
- **Database query caching**: Cache frequently executed query results
- **CDN usage**: Serve static assets from content delivery networks
- **Cache invalidation**: Implement proper cache expiration and invalidation strategies

### Monitoring and Profiling
- **Performance metrics**: Track response times, throughput, and resource usage
- **APM tools**: Use Application Performance Monitoring for production systems
- **Load testing**: Test performance under expected and peak loads
- **Memory profiling**: Identify memory usage patterns and potential leaks
- **Database query analysis**: Monitor slow queries and execution plans
