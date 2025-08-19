# Pachyterm Agent Guidelines

## Build/Test Commands
- **Build**: `cargo build --release`
- **Test**: `cargo test` 
- **Single test**: `cargo test test_name`
- **Benchmarks**: `cargo run --bin config_bench` or `cargo run --bin tty_bench`
- **Examples**: `cargo run --example config_demo` or `cargo run --example tty_demo`

## Code Style

### Imports
- Group std imports first, then external crates, then local modules
- Use explicit imports (avoid `use module::*`)
- Order: std, external crates (serde, thiserror, etc.), local modules

### Error Handling
- Use `thiserror::Error` for custom error types with `#[error()]` messages
- Return `Result<T, CustomError>` from fallible functions
- Use descriptive error variants with context (e.g., `PtyNotFound { id: u64 }`)

### Types & Naming
- Use `snake_case` for variables/functions, `PascalCase` for types/enums
- Prefer explicit types for public APIs (u64, u32, etc.)
- Use descriptive names: `ConfigManager`, `TtyEngine`, `PtySession`

### Formatting
- Use `rustfmt` defaults (4-space indentation, 100 char lines)
- No trailing commas in single-line items
- Prefer single-line closures when readable