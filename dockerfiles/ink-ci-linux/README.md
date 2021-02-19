# ink! CI for Linux Distributions

Docker image based on our base CI image `<base-ci-linux:latest>`.

Used to build and test ink!.

## Dependencies and Tools

**Inherited from `<base-ci-linux:latest>`:**

- `libssl-dev`
- `clang-7`
- `lld`
- `libclang-dev`
- `make`
- `cmake`
- `git`
- `pkg-config`
- `curl`
- `time`
- `rhash`
- `ca-certificates`

**Rust versions:**

We always try to use the [latest possible](https://rust-lang.github.io/rustup-components-history/) `nightly` version that supports our required `rustup` components:

- `clippy`: The Rust linter.
- `rust-src`: The Rust sources of the standard library.
- `miri`: The Rust MIR interpreter that interprets the test suite with additional checks.
- `rustfmt`: The Rust code formatter.
- `llvm-tools-preview`: LLVM tools.

**Rust tools & toolchains:**

- `grcov`: Required for general Rust code coverage reports.
- `rust-covfix`: Required to polish the coverage reports by `grcov`.
- `cargo-contract`: Required to build ink! Wasm smart contracts.
- `xargo`: Required so that `miri` runs properly.
- `wasm32-unknown-unknown`: The toolchain to compile Rust codebases for Wasm.

[Click here](https://hub.docker.com/repository/docker/paritytech/ink-ci-linux) for the registry.

## Usage

```yaml
test-ink:
    stage: test
        image: paritytech/ink-ci-linux:latest
        script:
            - cargo build ...
```
