# go-style-guide
Explains how roclub writes Golang code.

## table of contents
[Error-Handling](#Error-Handling)
[Anti-Patterns](#Anti-Patterns)

## Error-Handling

Return errors as early as possible.

### Check errors and handle them gracefully
Rule 1: Comparing error contents with matching strings is a code smell. Contents of an error belong to stdout or logfiles for humans to read it. Check the error type instead.
Rule 2: Your package should not export custom error values.

Check: https://dave.cheney.net/2016/04/27/dont-just-check-errors-handle-them-gracefully


## Anti-Patterns
- reflections are not clear, don't use them (ref. https://www.youtube.com/watch?v=PAAkCSZUG1c&t=922s)
