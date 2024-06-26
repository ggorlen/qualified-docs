---
summary: Information for unit testing Rust code
tags:
  - rust
  - testing
---

# Unit Testing Rust

## Overview

Qualified supports writing tests for Rust using `tests` mod with `#[cfg(test)]` attibute.

## Basic Example

### Solution

```rust
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

### Tests

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_add() {
        assert_eq!(add(1, 1), 2);
    }
}
```

## Preloaded Example

### Preloaded

```rust
pub fn optional_helper_func() -> i32 {
    42
}
```

### Solution

```rust
use preloaded::optional_helper_func;

pub fn add(a: i32, b: i32) -> i32 {
    optional_helper_func();
    a + b
}
```

### Tests

```rust
mod preloaded;

#[cfg(test)]
mod tests {
    use super::*;
    use preloaded::optional_helper_func;

    #[test]
    fn test_add() {
        optional_helper_func();
        assert_eq!(add(1, 1), 2);
    }
}
```

# Learn More

You can learn more on the [unit testing chapter][1] in [Rust by Example][0].

[0]: https://doc.rust-lang.org/rust-by-example/index.html
[1]: https://doc.rust-lang.org/rust-by-example/testing/unit_testing.html
