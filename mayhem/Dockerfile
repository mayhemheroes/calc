FROM --platform=linux/amd64 ubuntu:20.04 as builder

RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y build-essential libreadline-dev bsdmainutils clang

COPY . /repo
WORKDIR /repo
RUN make -j8
RUN make install LIBDIR=/install BINDIR=/install
RUN clang++ /repo/mayhem/fuzz_convhex2z.cpp -fsanitize=fuzzer,address -L /install/ -lcalc -lcustcalc -o /fuzz

FROM ubuntu:20.04 as package
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y libreadline-dev
COPY --from=builder /install /install
COPY --from=builder /fuzz /fuzz_convhex2z
ENV LD_LIBRARY_PATH=/install
