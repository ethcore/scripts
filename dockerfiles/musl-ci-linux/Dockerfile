ARG VCS_REF=master
ARG BUILD_DATE=""
ARG REGISTRY_PATH=paritytech

FROM ${REGISTRY_PATH}/ci-linux:production

# metadata
LABEL summary="MUSL Image for Substrate-based projects." \
	name="${REGISTRY_PATH}/ci-linux-musl" \
	maintainer="devops-team@parity.io" \
	version="1.0" \
	description="Inherits from ci-linux" \
	io.parity.image.vendor="Parity Technologies" \
	io.parity.image.source="https://github.com/paritytech/scripts/blob/${VCS_REF}/\
dockerfiles/ci-linux-musl/Dockerfile" \
	io.parity.image.documentation="https://github.com/paritytech/scripts/blob/${VCS_REF}/\
dockerfiles/ci-linux-musl/README.md" \
	io.parity.image.revision="${VCS_REF}" \
	io.parity.image.created="${BUILD_DATE}"

ENV CC=musl-gcc \
	CXX=musl-gcc \
	TARGET_CC=musl-gcc \
	TARGET_CXX=musl-gcc \
	PKG_CONFIG_ALLOW_CROSS=1 \
	PKG_CONFIG_ALL_STATIC=1 \
	PORTABLE=1 \
	RUST_BACKTRACE=1 \
	RUSTC_WRAPPER=

# install tools and dependencies
RUN set -eux && \
	apt-get update -y && \
	apt-get install -y musl musl-dev musl-tools && \
	rustup target add x86_64-unknown-linux-musl && \
	rustup target add x86_64-unknown-linux-musl --toolchain=nightly && \
  update-alternatives --remove cc /usr/bin/clang && \
  update-alternatives --remove c++ /usr/bin/clang++ && \
# Cheeky hack to make openssl work...
  ln -s /usr/include/x86_64-linux-gnu/openssl/ /usr/include/x86_64-linux-musl/openssl && \
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
