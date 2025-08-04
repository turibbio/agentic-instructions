---
description: 'Guidelines for building [LANGUAGE] applications'
applyTo: '**/*.[LANGUAGE_EXT]'
---

# [LANGUAGE] Development

## [LANGUAGE] Instructions
- Always use the latest version [LANGUAGE], currently [LANGUAGE] [VERSION] features.
- Write clear and concise comments for each function.

## General Instructions
- Make only high confidence suggestions when reviewing code changes.
- Write code with good maintainability practices, including comments on why certain design decisions were made.
- Handle edge cases and write clear exception handling.
- For libraries or external dependencies, mention their usage and purpose in comments.

## Naming Conventions

- Follow PascalCase for component names, method names, and public members.
- Use camelCase for private fields and local variables.
- Prefix interface names with "I" (e.g., I[Entity]Service).

## Formatting

- Apply code-formatting style defined in `.editorconfig`.
- Prefer file-scoped namespace declarations and single-line using directives.
- Insert a newline before the opening curly brace of any code block (e.g., after `if`, `for`, `while`, `foreach`, `using`, `try`, etc.).
- Ensure that the final return statement of a method is on its own line.
- Use pattern matching and switch expressions wherever possible.
- Use `nameof` instead of string literals when referring to member names.
- Ensure that XML doc comments are created for any public APIs. When applicable, include `<example>` and `<code>` documentation in the comments.

## Project Setup and Structure

- Guide users through creating a new [FRAMEWORK] project with the appropriate templates.
- Explain the purpose of each generated file and folder to build understanding of the project structure.
- Demonstrate how to organize code using feature folders or domain-driven design principles.
- Show proper separation of concerns with models, services, and data access layers.
- Explain the Program.[LANGUAGE_EXT] and configuration system in [FRAMEWORK] [VERSION] including environment-specific settings.

## Nullable Reference Types

- Declare variables non-nullable, and check for `null` at entry points.
- Always use `is null` or `is not null` instead of `== null` or `!= null`.
- Trust the [LANGUAGE] null annotations and don't add null checks when the type system says a value cannot be null.

## Advanced [LANGUAGE] Features

- Leverage modern [LANGUAGE] [VERSION] features for cleaner, more efficient code
- Use records for immutable data transfer objects
- Implement pattern matching for complex conditional logic
- Utilize nullable reference types for better null safety
- Apply async streams for large data processing

## Testing

- Always include test cases for critical paths of the application.
- Guide users through creating unit tests.
- Do not emit "Act", "Arrange" or "Assert" comments.
- Copy existing style in nearby files for test method names and capitalization.
- Explain integration testing approaches for API endpoints.
- Demonstrate how to mock dependencies for effective testing.
- Show how to test authentication and authorization logic.
- Explain test-driven development principles as applied to API development.

## Memory Management and Performance

- Use `Span<T>` and `Memory<T>` for high-performance scenarios
- Implement object pooling for frequently allocated objects
- Leverage `IAsyncEnumerable<T>` for streaming large datasets
- Use `ValueTask<T>` when appropriate for hot paths
- Apply source generators for compile-time optimizations