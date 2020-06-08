ARG VCS_REF=master
ARG BUILD_DATE=""
ARG REGISTRY_PATH=paritytech

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
RUN set -eux; \
	apt-get -y update; \
	apt-get install -y --no-install-recommends \
		chromium-driver; \
# use minimum components
	rustup set profile minimal; \
# install Rust nightly, default is stable
	rustup install nightly; \
# install cargo tools
	cargo install cargo-audit cargo-web wasm-pack cargo-deny wasm-bindgen-cli; \
# install wasm toolchain
	rustup target add wasm32-unknown-unknown; \
	rustup target add wasm32-unknown-unknown --toolchain nightly; \
# install wasm-gc. It's useful for stripping slimming down wasm binaries (polkadot)
	cargo +nightly install wasm-gc; \
# versions
	rustup show; \
	cargo --version; \
# apt clean up
	apt-get autoremove -y; \
	apt-get clean; \
	rm -rf /var/lib/apt/lists/*; \
# cargo clean up
# removes compilation artifacts cargo install creates (>250M)
	rm -rf $CARGO_HOME/registry; \
# removes toolchain's html docs and autocompletions (>300M for each toolchain)
	rm -rf /usr/local/rustup/toolchains/*/share