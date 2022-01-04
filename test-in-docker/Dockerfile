FROM debian:10 AS builder

RUN apt-get update && apt-get install -y \
   unzip \
   libpcap-dev \
   build-essential \
   git \
   cmake \
   wget \
   && rm -rf /var/lib/dpkg/lists/*

WORKDIR /src

RUN git clone https://git.savannah.nongnu.org/git/lwip.git

RUN wget http://elm-chan.org/fsw/ff/arc/ff14.zip && unzip -q ff14.zip -d ff && rm ff14.zip
RUN sed -i -e 's/FF_FS_RPATH[	 ]*0/FF_FS_RPATH 2/;s/FF_USE_MKFS[	 ]*0/FF_USE_MKFS 1/;' ff/source/ffconf.h

# sources
COPY ["*.c", "*.h", "lwip-ftpd/"]
# test sources
COPY ./test-in-docker/src/ lwip-ftpd/test-in-docker/src/
COPY ./test-in-docker/lwip-include/ lwip-ftpd/test-in-docker/lwip-include/

WORKDIR /build

RUN cmake /src/lwip-ftpd/test-in-docker/src && make -j

FROM debian:10 AS runner

RUN apt-get update && apt-get install -y \
   libpcap0.8 \
   valgrind \
   && rm -rf /var/lib/dpkg/lists/*

WORKDIR /app

COPY --from=builder /build/lwip-runner .

CMD valgrind /app/lwip-runner
