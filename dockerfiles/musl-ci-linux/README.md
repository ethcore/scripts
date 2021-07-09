# musl-ci-linux

Docker image based on our Linux CI image `<ci-linux:production>`.

Used to build and test Substrate-based projects using the MUSL toolchain.

## Dependencies and Tools

**Inherited from `<ci-linux:production>`:**

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
- `chromium-driver`
- `cargo-audit`
- `cargo-web`
- `sccache`
- `wasm-pack`
- `wasm-bindgen`
- `cargo-deny`
- `mdbook`

**Rust versions:**

- stable (default)
- nightly

**Rust tools & toolchains:**

- `musl`
- `musl-dev`
- `musl-tools`
- `wasm32-unknown-unknown` toolchain

[Click here](https://hub.docker.com/repository/docker/paritytech/musl-ci-linux) for the registry.

## Usage

```yaml
test-substrate:
    stage: test
        image: paritytech/musl-ci-linux:latest
        script:
            - cargo build ... --target=x86_64-unknown-linux-musl
```
