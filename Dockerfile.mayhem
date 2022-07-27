FROM --platform=linux/amd64 ubuntu:20.04 as builder

RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y build-essential libreadline-dev bsdmainutils

COPY . /repo
WORKDIR /repo
RUN make -j8
RUN make install LIBDIR=/install BINDIR=/install

FROM ubuntu:20.04 as package
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y libreadline-dev
COPY --from=builder /install /install
ENV LD_LIBRARY_PATH=/install
