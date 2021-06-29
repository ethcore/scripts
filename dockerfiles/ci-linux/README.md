# ci-linux

Docker image based on our base CI image `<base-ci:latest>`.

Used to build and test Substrate-based projects.

## Dependencies and Tools

- `chromium-driver`

**Inherited from `<base-ci:latest>`:**

- `libssl-dev`
- `clang-10`
- `clang++-10`
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
- `jq`

**Rust versions:**

- stable (default)
- nightly

**Rust tools & toolchains:**

- `sccache`
- `wasm-pack`
- `wasm-bindgen`
- `wasm-gc`
- `cargo-deny`
- `wasm32-unknown-unknown` toolchain

[Click here](https://hub.docker.com/repository/docker/paritytech/ci-linux) for the registry.

## Usage

```yaml
test-substrate:
    stage: test
        image: paritytech/ci-linux:production
        script:
            - cargo build ...
```
