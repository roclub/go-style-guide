# Go Style Guide

Explains how roclub writes Golang code.

## Table of Contents

- [Semantic](#semantic)
- [Error](#error)
- [Error-Handling](#Error-Handling)
- [Logging](#logging)
- [Anti-Patterns](#anti-patterns)

## Semantic

### Formatting

All Go source files must conform to the format outputted by the gofmt tool.

### Naming

Naming is more art than science. In Go, names tend to be somewhat shorter than in many other languages,
but the same general guidelines apply.

Names should:

- Not feel repetitive when they are used
- Take the context into consideration
- Not repeat concepts that are already clear

### MixedCaps

Go source code uses MixedCaps or mixedCaps (camel case) rather than underscores (snake case) when writing multi-word names.

This applies even when it breaks conventions in other languages. For example, a constant is MaxLength (not MAX_LENGTH) if exported and maxLength (not max_length) if unexported.

### Line length

Lines should be no longer than 85 characters. Ideally, they should be shorter.

Do not split a line:

- Before an indentation change (e.g., function declaration, conditional)
- To make a long string (e.g., a URL) fit into multiple shorter lines

```go
// Good
if longCondition1 && longCondition2 && longCondition3 && longCondition4 {
    log.Info("all conditions met")
}

// Bad
if longCondition1 && longCondition2 &&
    // Conditions 3 and 4 have the same indentation as the code within the if.
    longCondition3 && longCondition4 {
    log.Info("all conditions met")
}
```

### References

- [General Naming](https://testing.googleblog.com/2017/10/code-health-identifiernamingpostforworl.html)

## Error

All errors should be wrapped.

### Examples

```go
// Good
if err := exampleFunction(ctx); err != nil {
  return fmt.Errorf("could not execute example function: %w", err)
}

// Bad
if err := exampleFunction(ctx); err != nil {
  return err
}
```

### References

## Error-Handling

Return errors as early as possible.

### Check errors and handle them gracefully
Rule 1: Comparing error contents with matching strings is a code smell. Contents of an error belong to stdout or logfiles for humans to read it. Check the error type instead.
Rule 2: Your package should not export custom error values.

Check: https://dave.cheney.net/2016/04/27/dont-just-check-errors-handle-them-gracefully


## Anti-Patterns

- reflections are not clear, don't use them (ref. https://www.youtube.com/watch?v=PAAkCSZUG1c&t=922s)

## Logging

At roclub we use the Zap logger.
A logline should start lowercase. A log line should contain a message and a context.
The context can be an error or information/debug fields.

### Usage

### Examples
  
```go
// Good
if err := exampleFunction(ctx); err != nil {
  logger.Error("failed to execute example function", err)
}

// Bad
if err := exampleFunction(ctx); err != nil {
  logger.Error("Failed to execute example function")
}
```

### References

- [Zap]()
