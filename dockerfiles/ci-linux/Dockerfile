ARG VCS_REF=master
ARG BUILD_DATE=""
ARG REGISTRY_PATH=docker.io/paritytech

FROM ${REGISTRY_PATH}/base-ci-linux:latest

# metadata
LABEL summary="Image for Substrate-based projects." \
	name="${REGISTRY_PATH}/ci-linux" \
	maintainer="devops-team@parity.io" \
	version="1.0" \
	description="Inherits from base-ci-linux; chromium-driver, \
wasm-gc, wasm-bindgen-cli, wasm-pack, cargo-audit, cargo-web, cargo-deny " \
	io.parity.image.vendor="Parity Technologies" \
	io.parity.image.source="https://github.com/paritytech/scripts/blob/${VCS_REF}/\
dockerfiles/ci-linux/Dockerfile" \
	io.parity.image.documentation="https://github.com/paritytech/scripts/blob/${VCS_REF}/\
dockerfiles/ci-linux/README.md" \
	io.parity.image.revision="${VCS_REF}" \
	io.parity.image.created="${BUILD_DATE}"

# install tools and dependencies
RUN set -eux && \
	apt-get -y update && \
	apt-get install -y --no-install-recommends \
		chromium-driver && \
# install `rust-src` component for ui test
	rustup component add rust-src && \
# install specific Rust nightly, default is stable, use minimum components
	rustup toolchain install nightly-2021-06-29 --profile minimal && \
# "alias" pinned nightly-2021-06-29 toolchain as nightly
	ln -s /usr/local/rustup/toolchains/nightly-2021-06-29-x86_64-unknown-linux-gnu /usr/local/rustup/toolchains/nightly-x86_64-unknown-linux-gnu && \
# install wasm toolchain
	rustup target add wasm32-unknown-unknown && \
	rustup target add wasm32-unknown-unknown --toolchain nightly && \
# install cargo tools
	cargo install cargo-web wasm-pack cargo-deny cargo-spellcheck && \
	cargo install --version 0.4.0 diener && \
# wasm-bindgen-cli version should match the one pinned in substrate
# https://github.com/paritytech/substrate/blob/master/bin/node/browser-testing/Cargo.toml#L15
	cargo install --version 0.2.73 wasm-bindgen-cli && \
# install wasm-gc. It's useful for stripping slimming down wasm binaries (polkadot)
	cargo +nightly install wasm-gc && \
# versions
	rustup show && \
	cargo --version && \
# apt clean up
	apt-get autoremove -y && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/* && \
# cargo clean up
# removes compilation artifacts cargo install creates (>250M)
	rm -rf "${CARGO_HOME}/registry" "${CARGO_HOME}/git" /root/.cache/sccache

# TODO: switch to non-root here when substrate will work with latest rust nightlies again
# USER nonroot:nonroot
