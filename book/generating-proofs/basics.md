# Generating Proofs: Basics

An end-to-end flow of proving `f(x) = y` with the SP1 zkVM involves the following steps:

- Define `f` using normal Rust code and compile it to an ELF (covered in the [writing programs](../writing-programs/basics.md) section). 
- Generate a proof `π` using the SP1 zkVM that `f(x) = y` with `prove(ELF, x)`.
- Verify the proof `π` using `verify(ELF, x, y, π)`.

To make this more concrete, let's walk through a simple example of generating a proof for a Fibonacci program inside the zkVM.

## Fibonacci

```rust,noplayground
{{#include ../../examples/fibonacci-io/script/src/main.rs}}
```

You can run the above script with `RUST_LOG=info cargo run --release`.

## Build Script

If you want your program crate to be built automatically whenever you build/run your script crate, you can add a `build.rs` file inside of `script/` (at the same level as `Cargo.toml`):

```rust,noplayground
{{#include ../../examples/fibonacci-io/script/build.rs}}
```

Make sure to also add `sp1-helper` as a build dependency in `script/Cargo.toml`:

```toml
[build-dependencies]
sp1-helper = { git = "https://github.com/succinctlabs/sp1.git" }
```

If you run `RUST_LOG=info cargo run --release -vv`, you will see the following output from the build script if the program has changed, indicating that the program was rebuilt:
```
[fibonacci-script 0.1.0] cargo:rerun-if-changed=../program/src
[fibonacci-script 0.1.0] cargo:rerun-if-changed=../program/Cargo.toml
[fibonacci-script 0.1.0] cargo:rerun-if-changed=../program/Cargo.lock
[fibonacci-script 0.1.0] cargo:warning=fibonacci-program built at 2024-03-02 22:01:26
[fibonacci-script 0.1.0] [sp1]    Compiling fibonacci-program v0.1.0 (/Users/umaroy/Documents/fibonacci/program)
[fibonacci-script 0.1.0] [sp1]     Finished release [optimized] target(s) in 0.15s
warning: fibonacci-script@0.1.0: fibonacci-program built at 2024-03-02 22:01:26```
``````
