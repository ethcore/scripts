ARG VCS_REF=master
ARG BUILD_DATE
ARG REGISTRY_PATH=paritytech

FROM ${REGISTRY_PATH}/base-ci:latest

# metadata
LABEL io.parity.image.authors="devops-team@parity.io" \
	io.parity.image.vendor="Parity Technologies" \
	io.parity.image.title="${REGISTRY_PATH}/sccache-ci-linux" \
	io.parity.image.description="CI image with all dependencies for sccache compilation. \
Inherits from paritytech/base-ci. libssl-dev, clang, libclang-dev, lld, cmake, make, \
git, pkg-config curl, time, rhash, rust stable, rust nightly, sccache" \
	io.parity.image.source="https://github.com/paritytech/scripts/blob/${VCS_REF}/\
dockerfiles/sccache-ci-linux/Dockerfile" \
	io.parity.image.documentation="https://github.com/paritytech/scripts/blob/${VCS_REF}/\
dockerfiles/sccache-ci-linux/README.md" \
	io.parity.image.revision="${VCS_REF}" \
	io.parity.image.created="${BUILD_DATE}"

WORKDIR /builds

ENV SHELL /bin/bash
ENV DEBIAN_FRONTEND=noninteractive
ENV TERM=xterm-256color
ENV LD_LIBRARY_PATH=/usr/local/cuda-11.1/lib64
ENV PATH=/usr/local/cargo/bin:/usr/local/cuda-11.1/bin:$PATH
ENV CC=clang
ENV CXX=clang

# config for wasm32-unknown-unknown & clang
COPY utility/base-ci-linux-config /root/.cargo/config

# install tools and dependencies
RUN set -eux && \
	apt-get -y update && \
	apt-get install -y --no-install-recommends \
		linux-headers-5.8.0-29 gcc binutils coreutils gdbserver \
		zlib1g-dev librust-zip+deflate-miniz-dev gnupg2 docker && \
# setup nvcc
    curl -fsSL https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/7fa2af80.pub | apt-key add - && \
    echo "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64 /" > /etc/apt/sources.list.d/cuda.list && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        cuda-command-line-tools-11-1=11.1.1-1 && \
    ln -s cuda-11.1 /usr/local/cuda && \
# install rustup, use minimum components
	rustup toolchain install stable --profile minimal --component clippy rustfmt && \
	rustup default stable && \
    rustup toolchain install nightly --profile minimal && \
    cargo install broot ripgrep sd && \
# versions
	rustup show && \
	cargo --version && \
    nvcc --version && \
	gcc --version && \
	clang --version && \
	gdbserver --version && \
	openssl version && \
# cargo clean up
# removes compilation artifacts cargo install creates (>250M)
	rm -rf "${CARGO_HOME}/registry" "${CARGO_HOME}/git" /root/.cache/sccache && \
# apt clean up
	apt-get autoremove -y && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/*
# cache handler
ENV	RUSTC_WRAPPER=sccache \
# show backtraces
  	RUST_BACKTRACE=1

CMD ["bash", "-l"]
