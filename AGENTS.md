# Compliance Rules

This file contains the compliance and code quality rules for this repository.

## 1. All C# Source Files Must Include MIT License Header

**Objective:** Ensure legal compliance and proper attribution by requiring the standardized .NET Foundation MIT license header at the beginning of every C# source file

**Success Criteria:** Every .cs file starts with the exact two-line comment header: '// Licensed to the .NET Foundation under one or more agreements.' followed by '// The .NET Foundation licenses this file to you under the MIT license.'

**Failure Criteria:** C# files are missing the license header, have an incorrect or modified license header, or have the header in the wrong location (not at the start of the file)

---

## 2. Use File-Scoped Namespace Declarations

**Objective:** Maintain consistency with modern C# conventions and reduce indentation levels by using file-scoped namespace declarations instead of traditional block-scoped namespaces

**Success Criteria:** All C# files use file-scoped namespace syntax (namespace declaration ending with semicolon without braces) as in 'namespace Microsoft.AspNetCore.Feature;'

**Failure Criteria:** C# files use traditional namespace declarations with opening and closing braces

---

## 3. Private and Internal Fields Must Use Underscore Prefix with camelCase

**Objective:** Maintain consistent code style and improve readability by enforcing the underscore prefix naming convention for private and internal instance fields

**Success Criteria:** All private and internal instance fields are named with an underscore prefix followed by camelCase (e.g., '_antiforgery', '_tokenStore', '_dbOperations')

**Failure Criteria:** Private or internal instance fields lack the underscore prefix, use PascalCase, or use other naming patterns inconsistent with the convention

---

## 4. Use ArgumentNullException Throw Helpers for Parameter Validation

**Objective:** Standardize null parameter validation and improve code clarity by using modern throw helper methods instead of traditional null checks with manual exception throwing

**Success Criteria:** Null parameter validation uses ArgumentNullException.ThrowIfNull(parameter) or similar throw helper methods (ThrowIfNullOrEmpty, ThrowIfNullOrWhiteSpace) at the beginning of public methods

**Failure Criteria:** Code uses manual null checks with 'if (parameter == null) throw new ArgumentNullException()' pattern or lacks null parameter validation where appropriate

---

## 5. Test Classes Must Follow Naming Convention

**Objective:** Maintain consistency and discoverability of test files by enforcing standardized test class naming patterns that clearly identify their purpose

**Success Criteria:** Test class names end with 'Test' or 'Tests' suffix (e.g., 'DefaultAntiforgeryTest', 'SqlServerCacheTests') and are located in directories named 'test'

**Failure Criteria:** Test classes lack the 'Test' or 'Tests' suffix, use inconsistent naming patterns, or are not clearly identifiable as test classes

---

## 6. Test Methods Must Use Arrange-Act-Assert Pattern with Comments

**Objective:** Improve test readability and maintainability by enforcing explicit structural comments that separate the test setup, execution, and verification phases

**Success Criteria:** Test methods include explicit '// Arrange', '// Act', and '// Assert' comments (or combined '// Act & Assert' when appropriate) that clearly delineate the three phases of the test

**Failure Criteria:** Test methods lack the AAA pattern comments, use inconsistent comment formatting, or mix phases without clear separation

---

## 7. Public APIs Must Have XML Documentation Comments

**Objective:** Ensure comprehensive API documentation for consumers by requiring XML documentation on all public types, methods, properties, and events

**Success Criteria:** All public APIs include XML documentation comments with at minimum <summary> tags, and <param>, <returns>, <exception>, and <remarks> tags where applicable

**Failure Criteria:** Public types, methods, properties, or events lack XML documentation comments, have incomplete documentation, or have missing required tags like <param> for parameters

---

## 8. Async Methods Must Be Named with Async Suffix

**Objective:** Improve code clarity and follow .NET naming conventions by ensuring all asynchronous methods are clearly identified with the 'Async' suffix

**Success Criteria:** All methods returning Task or Task<T> are named with the 'Async' suffix (e.g., 'GetAsync', 'ValidateRequestAsync', 'SaveAsync')

**Failure Criteria:** Methods returning Task or Task<T> lack the 'Async' suffix or methods not returning Task inappropriately use the suffix

---

## 9. Async Methods Must Use ConfigureAwait(false) in Library Code

**Objective:** Prevent deadlocks and improve performance in library code by ensuring async operations do not capture the synchronization context unnecessarily

**Success Criteria:** All await expressions in library code (non-test, non-sample projects) use .ConfigureAwait(false) when awaiting tasks

**Failure Criteria:** Await expressions in library code omit ConfigureAwait(false) or use ConfigureAwait(true), potentially causing synchronization context capture

---

## 10. Async Methods Must Accept CancellationToken Parameter

**Objective:** Enable cooperative cancellation and improve responsiveness by ensuring all async methods provide a mechanism for callers to cancel long-running operations

**Success Criteria:** Public async methods include a CancellationToken parameter, typically with a default value of 'default(CancellationToken)' or 'default', positioned as the last parameter

**Failure Criteria:** Async methods that perform I/O or long-running operations lack a CancellationToken parameter or position it incorrectly in the parameter list

---

## 11. Use Primary Constructor Syntax Where Appropriate

**Objective:** Reduce boilerplate code and improve readability by using C# 12 primary constructor syntax for simple dependency injection scenarios

**Success Criteria:** Classes with simple constructor-to-field assignment patterns use primary constructor syntax (parameters in parentheses after class name) instead of traditional constructor with field assignments

**Failure Criteria:** Classes with simple dependency injection patterns use traditional constructors with repetitive parameter-to-field assignments when primary constructor syntax would be more concise

---

## 12. Service Registration Extension Methods Must Return IServiceCollection

**Objective:** Enable method chaining and follow the fluent API pattern by ensuring all service registration extension methods return the service collection for further configuration

**Success Criteria:** Extension methods on IServiceCollection that register services return the IServiceCollection instance to enable fluent chaining

**Failure Criteria:** Service registration extension methods return void or other types, breaking the fluent API pattern and preventing method chaining

---

## 13. Use TryAdd Methods for Optional Service Registration

**Objective:** Allow consumers to override default service registrations by using TryAdd methods that only register services if no implementation already exists

**Success Criteria:** Service registration for overridable services uses TryAddSingleton, TryAddScoped, TryAddTransient, or TryAddEnumerable instead of direct Add methods

**Failure Criteria:** Service registration uses Add methods (AddSingleton, AddScoped, AddTransient) for services that should be overridable, preventing consumer customization

---

## 14. Sealed Keyword Required for Internal Implementation Classes

**Objective:** Improve performance by preventing virtual dispatch overhead and clarify design intent by sealing internal classes not intended for inheritance

**Success Criteria:** Internal classes that are not explicitly designed for inheritance are marked with the 'sealed' keyword (e.g., 'internal sealed class DefaultAntiforgery')

**Failure Criteria:** Internal implementation classes lack the sealed keyword even though they are not designed for inheritance, potentially allowing unintended subclassing

---

## 15. Use Source-Generated Logging with LoggerMessage Attribute

**Objective:** Improve logging performance and type safety by using compile-time generated logging methods instead of runtime string formatting

**Success Criteria:** Logging code uses partial methods with [LoggerMessage] attribute that specify log level, event ID, and message template for compile-time generation

**Failure Criteria:** Logging uses traditional ILogger methods (_logger.LogInformation, _logger.LogWarning) with runtime string formatting instead of source-generated logging

---

## 16. Middleware Must Implement Fast-Path Synchronous Invoke Method

**Objective:** Optimize middleware pipeline performance by providing a synchronous code path for common scenarios that do not require awaiting async operations

**Success Criteria:** Middleware classes implement a synchronous Invoke method that returns Task and only calls a separate async method (e.g., InvokeAwaited) when async operations are actually needed

**Failure Criteria:** Middleware implements only async Invoke methods or uses async/await unnecessarily in the hot path when synchronous execution would suffice

---

## 17. Use Curly Braces for All Control Flow Statements

**Objective:** Prevent subtle bugs and improve code consistency by requiring braces around all control flow statement bodies, even single-line statements

**Success Criteria:** All if, else, for, foreach, while, and do-while statements use curly braces to enclose their bodies, even for single statements

**Failure Criteria:** Control flow statements omit curly braces for single-line bodies, making the code prone to errors when modifications are made

---

## 18. Opening Braces Must Be on New Line (Allman Style)

**Objective:** Maintain visual consistency and improve code readability by enforcing the Allman brace style where opening braces appear on their own line

**Success Criteria:** All opening braces for methods, classes, namespaces, control flow statements, and other blocks appear on a new line after the declaration

**Failure Criteria:** Opening braces appear on the same line as the declaration (K&R style) instead of on a new line

---

## 19. Test Projects Must Use xUnit Framework

**Objective:** Maintain consistency across the codebase and leverage xUnit's features by standardizing on xUnit as the test framework for all test projects

**Success Criteria:** Test methods use xUnit attributes ([Fact], [Theory], [InlineData]) and assertions (Assert.Equal, Assert.Throws, etc.)

**Failure Criteria:** Test projects use alternative testing frameworks (MSTest, NUnit) inconsistent with the repository standard

---

## 20. Unit Test Projects Must Follow Directory Structure Convention

**Objective:** Maintain consistent project organization by placing test projects in 'test' directories parallel to their corresponding 'src' directories

**Success Criteria:** Test projects are located in directories named 'test' that are siblings to 'src' directories containing the code under test

**Failure Criteria:** Test projects are placed in inconsistent locations such as within src directories, in misnamed directories, or without clear correspondence to source projects

---
