FROM registry.parity.io/parity/infrastructure/scripts/base-ci-linux:latest

# metadata
ARG VCS_REF
ARG BUILD_DATE

LABEL io.parity.image.authors="devops-team@parity.io" \
	io.parity.image.vendor="Parity Technologies" \
	io.parity.image.title="registry.parity.io/parity/infrastructure/scripts/contracts-ci-linux" \
	io.parity.image.description="Inherits from base-ci-linux:latest. rust nightly" \
	io.parity.image.source="https://github.com/paritytech/scripts/blob/${VCS_REF}/\
dockerfiles/contracts-ci-linux/Dockerfile" \
	io.parity.image.documentation="https://github.com/paritytech/scripts/blob/${VCS_REF}/\
dockerfiles/contracts-ci-linux/README.md" \
	io.parity.image.revision="${VCS_REF}" \
	io.parity.image.created="${BUILD_DATE}"

WORKDIR /builds

# copy yarn repo key
COPY utility/yarn.key /etc/apt/trusted.gpg.d/debian-yarn.gpg

# install tools and dependencies
RUN set -eux; \
	echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list; \
	DEBIAN_FRONTEND=noninteractive apt-get -y update; \
	DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        npm yarn wabt unzip; \
# install Rust nightly, default is stable
    rustup toolchain install nightly; \
# install wasm toolchain
    rustup target add wasm32-unknown-unknown --toolchain nightly; \
# install cargo tools
    cargo install sccache --features redis; \
    cargo install pwasm-utils-cli --bin wasm-prune; \
    cargo install --git https://github.com/paritytech/cargo-contract; \
# versions
    yarn --version; \
	rustup show; \
	cargo --version; \
# cargo clean up
# removes compilation artifacts cargo install creates (>250M)
	rm -rf $CARGO_HOME/registry; \
# removes toolchain's html docs and autocompletions (>300M for each toolchain)
	rm -rf /usr/local/rustup/toolchains/*/share; \
# apt clean up
	apt-get autoremove -y; \
	apt-get clean; \
	rm -rf /var/lib/apt/lists/*

# compiler ENV
ENV CC=gcc \
	CXX=g++ \
# cache handler
	RUSTC_WRAPPER=sccache \
# show backtraces
	RUST_BACKTRACE=1