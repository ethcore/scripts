FROM registry.parity.io/parity/infrastructure/scripts/parity-ci-linux:latest as builder

# metadata
ARG VCS_REF
ARG BUILD_DATE

LABEL io.parity.image.authors="devops-team@parity.io" \
	io.parity.image.vendor="Parity Technologies" \
	io.parity.image.title="registry.parity.io/parity/infrastructure/scripts/parity-ci-armhf" \
	io.parity.image.description="g++-arm-linux-gnueabihf gcc-arm-linux-gnueabihf libudev-dev libc6-dev-i386 \
libudev-dev:armhf; cargo target armv7-unknown-linux-gnueabihf" \
	io.parity.image.source="https://github.com/paritytech/scripts/blob/${VCS_REF}/\
dockerfiles/parity-ci-armhf/Dockerfile" \
	io.parity.image.documentation="https://github.com/paritytech/scripts/blob/${VCS_REF}/\
dockerfiles/parity-ci-armhf/README.md" \
	io.parity.image.revision="${VCS_REF}" \
	io.parity.image.created="${BUILD_DATE}"

# install armv7 dependencies and tools
RUN set -eux; \
	dpkg --add-architecture armhf; \
	echo '# source urls for armhf \n\
		deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ xenial main \n\
		deb-src [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ xenial main \n\
		deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ xenial-updates main \n\
		deb-src [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ xenial-updates main \n\
		deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ xenial-security main \n\
		deb-src [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ xenial-security main \
		\n # end armhf section' >> /etc/apt/sources.list; \
	sed -r 's/deb h/deb \[arch=amd64\] h/g' /etc/apt/sources.list > /tmp/sources-tmp.list; \
	cp /tmp/sources-tmp.list /etc/apt/sources.list; \
	sed -r 's/deb-src h/deb-src \[arch=amd64\] h/g' /etc/apt/sources.list > /tmp/sources-tmp.list; \
	cp /tmp/sources-tmp.list /etc/apt/sources.list; \
	apt-get -y update; \
	apt-get install -y --no-install-recommends \
		g++-arm-linux-gnueabihf gcc-arm-linux-gnueabihf libc6-dev-i386 \
		libudev-dev libudev-dev:armhf; \
# install armv7 toolchain
	rustup target add armv7-unknown-linux-gnueabihf; \
# apt clean up
	apt-get autoremove -y; \
	apt-get clean; \
	rm -rf /var/lib/apt/lists/*; \
# cargo clean up
# removes compilation artifacts cargo install creates (>250M)
	rm -rf $CARGO_HOME/registry; \
# removes toolchain's html docs and autocompletions (>300M for each toolchain)
	rm -rf /usr/local/rustup/toolchains/*/share;

# config for armv7-unknown-linux-gnueabihf
COPY utility/parity-ci-armhf-config /.cargo/config

# set cross-compiler ENV
ENV CC=arm-linux-gnueabihf-gcc \
	CXX=arm-linux-gnueabihf-g++ \
	CARGO_TARGET=armv7-unknown-linux-gnueabihf \
	HOST_CC=clang \
	HOST_CXX=clang \
	RUSTFLAGS="" \
	LDFLAGS="-L/usr/lib/arm-linix-gnueabihf" \
	CPPFLAGS="-I/usr/include" \
# cache handler
	RUSTC_WRAPPER=sccache \
# show backtraces
	RUST_BACKTRACE=1
