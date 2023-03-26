# Testing

Test function signature
- Must be in _test.go file
- func name must start with Test
- Must accept parameter whose type is *testing.T
- Don't return anything
- `t.Parallel()` is a standard statement/boilerplate for enabling test concurrency

want-and-got pattern

test cases, aka table tests, `for ... range` loop over a slice of structs with the test data

One behavior, one test
It’s tempting to try to extend our table test to check both the result and the error value at the same time. But that’s complicated, and we want tests to be simple. A useful way to think about this problem is to frame it in terms of behaviors, not functions.

This material may be protected by copyright.But now we’re saying there’s another behavior, too. If the input is invalid, Divide’s error result should be set to something that indicates a problem. For every different thing the function can do according to circumstances, we say it has a distinct behavior.