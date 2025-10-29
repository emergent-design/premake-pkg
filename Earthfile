VERSION 0.6

image:
	FROM ubuntu:22.04
	WORKDIR /code
	RUN apt-get update && apt-get install -y --no-install-recommends ca-certificates build-essential debhelper curl unzip uuid-dev

build:
	ARG VERSION=5.0.0-beta7
	FROM +image

	RUN curl -L https://github.com/premake/premake-core/releases/download/v$VERSION/premake-$VERSION-src.zip -so premake.zip \
		&& unzip premake.zip
	#RUN cd premake-$VERSION-src/build/gmake2.unix/ && make -j$(nproc) config=release
	#RUN mv premake-$VERSION-src/bin/release/premake5 ./
	RUN cd build/gmake.unix/ && make -j$(nproc) config=release
	RUN mv bin/release/premake5 ./

package:
	FROM +build
	COPY --dir package .
	RUN cd package && dpkg-buildpackage -b -uc -us
	SAVE ARTIFACT *.deb AS LOCAL build/
	# SAVE ARTIFACT *.deb

all:
	BUILD --platform=linux/amd64 --platform=linux/arm64 +package
