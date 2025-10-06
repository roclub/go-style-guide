# Go Style Guide

Explains how roclub writes Golang code.

## Table of Contents

- [Semantic](#semantic)
- [Error](#error)
- [Logging Style Guide](#logging-style-guide)
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

## Logging Style Guide

At roclub we use a custom Zap-based logger that provides global access through the `logger` package.

### Message Format

Log messages should:
- Start with lowercase
- Be concise and descriptive  
- Use present tense for actions ("processing user" not "processed user")
- Include relevant context as additional arguments

### Error Handling

When logging errors, pass the error as a separate argument to preserve error details:

```go
// Good
if err := processUser(userID); err != nil {
    logger.Error("failed to process user", err, userID)
    return fmt.Errorf("could not process user: %w", err)
}

// Bad - Missing context
if err := processUser(userID); err != nil {
    logger.Error("process failed")
    return err
}
```

### Context Information

Provide relevant context through additional arguments:

```go
// Good - Multiple context arguments
logger.Info("user authentication successful", userID, method, duration)

// Good - Single context argument
logger.Debug("processing request", requestID)

// Bad - No context
logger.Info("authentication successful")
```

### Log Levels

Use appropriate log levels:
- **Error**: For errors that require attention
- **Warn**: For unexpected but recoverable situations
- **Info**: For general operational messages
- **Debug**: For detailed debugging information
- **Fatal**: For unrecoverable errors (stops execution)

### Examples

**Error Logging:**
```go
// Input
logger.Error("failed to connect to database", err, "user123", 30)

// Output
failed to connect to database connection refused user123 30
```

**Info Logging:**
```go
// Input  
logger.Info("user logged in", userID, "oauth", time.Since(start))

// Output
user logged in user456 oauth 1.2s
```

**Debug Logging:**
```go
// Input
logger.Debug("processing webhook payload", webhookID, len(payload))

// Output  
processing webhook payload wh_abc123 1024
```

### Best Practices

- Always call `logger.Sync()` before application exit
- Pass errors as separate arguments, not embedded in strings
- Include identifiers (userID, requestID, etc.) for traceability
- Use consistent terminology across log messages
- Avoid logging sensitive information (passwords, tokens, etc.)

### References

- [Zap Documentation](https://pkg.go.dev/go.uber.org/zap)
- [Effective Go](https://go.dev/doc/effective_go)

## Anti-Patterns

- reflections are not clear, don't use them (ref. https://www.youtube.com/watch?v=PAAkCSZUG1c&t=922s)
