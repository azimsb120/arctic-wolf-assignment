## About

Implementation of an in-memory data structure that acts as a cache.

Programming language: Java.

## README.md

This README.md provides comprehensive information on the project structure, implementation details, assumptions, imporvements, etc.

Quick access to source and tests:

1. Source code: [LFU Cache Implementation](https://github.com/azimsb120/arctic-wolf-assignment/tree/master/src/main/java/cache)
2. Unit tests: [Cache Tests](https://github.com/azimsb120/arctic-wolf-assignment/tree/master/src/test/java/cache)

Quick demo of functionality:

[CacheTestRunner.java](https://github.com/azimsb120/arctic-wolf-assignment/tree/master/src/main/java/cache/CacheTestRunner.java) offers a high-level demonstration of the functionalities of the LFU cache, showcasing how entries are managed and evicted based on access frequency.

## Project Structure

- `src/main/java/cache/`: Contains the source files for the LFU cache implementation.
- `src/test/java/cache/`: Contains all JUnit test cases for testing the cache functionality.
- `bin/`: Directory for compiled bytecode files.
- `lib/`: Contains JUnit libraries necessary for running the tests.
- `README.md`: Provides the project overview, setup, design decisions, assumptions, and usage/testing instructions.

## Implementation Details

- **Methods**:
  - `get(K key)`: Retrieves the value associated with the specified key. If the key does not exist in the cache, the method returns `null`. This method is designed to handle cache misses gracefully without throwing exceptions.
  - `put(K key, V value)`: Inserts a new key-value pair into the cache. If the key already exists, the existing value is replaced with the new value. If inserting a new key-value pair causes the cache to exceed its capacity, the least frequently used item will be evicted.
  - `remove(K key)`: Removes the entry associated with the specified key from the cache. If the key does not exist, the operation has no effect.
  - `size()`: Returns the current number of entries in the cache.

### Eviction Policy

The eviction policy of the LFU cache we implemented is to evict entries based on their frequency of access. When there is a tie — i.e., multiple entries have the same frequency — the cache evicts the least recently added entry first. This behavior is achieved with the help of the `LinkedHashSet<K>` data structure, which maintains the insertion order of keys. This ensures that when there are entries with same frequency, the oldest is evicted first.

### Time-based Invalidation

The LFUCache implementates a time-based invalidation mechanism to automatically remove cache entries that have not been accessed or updated within a specified period of time. This time can be set in the constructor, or a default of 1 hour is used.

## Assumptions

- The cache is designed for situations where access frequency is one of the main factors in data relevance

## Configuration and Setup

To set up and run this project:

1. Ensure Java 11 or higher is installed.
2. Compile the source code using `javac` or an IDE that supports Java.
3. Run tests using the command: `java -cp lib/junit.jar:. org.junit.runner.JUnitCore cache.CacheTest`

## Library Management

Due to the time constraints of this assignment and the need for a straightforward project setup for evaluation, libraries required by the project (JUnit for testing) are currently included directly in the `lib/` directory. Under normal circumstances, it would be preferable to manage these dependencies through a build tool like Maven or Gradle.

## Future Improvements

**LFU Cache Limitations**

While LFU caching effectively prioritizes frequently accessed data, it still has drawbacks. For example, data accessed intensively over a short period may not be needed again soon, yet its high access frequency prevents its eviction.

Additionally, new items that were just added to the cache may be removed too soon, because the initial frequency count is low, even though they may be potentially accessed more frequently in the future. This situation might cause data that isn't accessed often but is used regularly over time to be removed from the cache too soon.

These challenges suggest that a more effective cache implementation may be integrating hybrid strategies that combine LFU principles with other caching mechanisms to optimize both frequency and recency of data access.

**Refined Tie-Breaking in Eviction Policy**

The current implementation of the eviction policy in case of a tie looks at the order of insertion. However, this approach may not accurately reflect the recency of usage of keys with the same frequency. As an improvement, we could implement an explicit tie-breaker using Least Recently Used (LRU) principles; i.e., the key with the least recent usage would be removed first. We would track access timestamps to accomplish this, effectively ensuring that the cache consistently retains the most relevant data.

**Concurrency**

The LFUCache we implemented assumes non-concurrent usage patterns. Multi-threading can be implemented for for high-concurrency usage.

- **Implement Dependency Management**: Post-assignment, plan to transition to using Maven or Gradle for dependency management. This will not only automate the management of library versions and dependencies but also align the project with standard Java project practices.
- **Remove Direct Library Inclusions**: Once a proper dependency management tool is configured, the libraries will be removed from the `lib/` directory and managed exclusively through the build tool's configuration files.
