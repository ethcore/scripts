# base-ci

Docker image based on [official Ubuntu 20.04 image](https://hub.docker.com/_/ubuntu) ubuntu:20.04.

Used as a base for our CI images.

Our base CI image `<base-ci:latest>`.

Used to build and test Substrate-based projects.

**Dependencies and Tools:**

- `libssl-dev`
- `clang-10`
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

[Click here](https://hub.docker.com/repository/docker/paritytech/base-ci) for the registry.

**Rust tools & toolchains:**

- stable (default)
- `sccache`

## Usage

```Dockerfile
FROM docker.io/paritytech/base-ci:latest
```
