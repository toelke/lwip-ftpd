FROM debian:10 AS builder

RUN apt update && apt install -y unzip libpcap-dev build-essential git cmake && rm -rf /var/lib/dpkg/lists/*

WORKDIR /src

RUN git clone https://git.savannah.nongnu.org/git/lwip.git

RUN git clone https://github.com/toelke/lwip-ftpd.git

COPY . lwip-ftpd

ADD http://elm-chan.org/fsw/ff/arc/ff14.zip .

RUN unzip -q ff14.zip -d ff

RUN sed -i -e 's/FF_FS_RPATH[	 ]*0/FF_FS_RPATH 2/;s/FF_USE_MKFS[	 ]*0/FF_USE_MKFS 1/;' ff/source/ffconf.h

WORKDIR /build

RUN cmake /src/lwip-ftpd/test-in-docker/src && make -j

FROM debian:10 AS runner

RUN apt update && apt install -y libpcap0.8 valgrind && rm -rf /var/lib/dpkg/lists/*

WORKDIR /app

COPY --from=builder /build/lwip-runner .

CMD valgrind /app/lwip-runner
