env_flags
=========

This library provides a convenient macro for declaring environment variables.

```toml
[dependencies]
env-flags = "0.1"
```

_Compiler support: requires rustc 1.80+_

## Example

```rust
use env_flags::env_flags;

use std::time::Duration;

env_flags! {
    /// Required env var, panics if missing.
    AUTH_TOKEN: &str;
    /// Env var with a default value if not specified.
    pub(crate) PORT: u16 = 8080;
    /// An optional env var.
    pub OVERRIDE_HOSTNAME: Option<&str> = None;

    /// `Duration` by default is parsed as `f64` seconds.
    TIMEOUT: Duration = Duration::from_secs(5);
    /// Custom parsing function, takes a `String` and returns a `Result<Duration>`.
    TIMEOUT_MS: Duration = Duration::from_millis(30), |value| {
        value.parse().map(Duration::from_millis)
    };

    /// `bool` can be true, false, 1, or 0 (case insensitive)
    /// eg. export ENABLE_FEATURE="true"
    pub ENABLE_FEATURE: bool = true;

    /// `Vec<T>` by default is parsed as a comma-seprated string
    /// eg. export VALID_PORTS="80,443,9121"
    pub VALID_PORTS: Vec<u16> = vec![80, 443, 9121];
}
```
