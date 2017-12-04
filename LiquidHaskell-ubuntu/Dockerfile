# z3
FROM ubuntu:16.04 AS build-z3
RUN apt-get update
RUN apt-get install -y z3

# COMPRESS WITH UPX
RUN apt-get install -y upx-ucl
RUN upx --best --ultra-brute /usr/bin/z3

# liquidhaskell
FROM ubuntu:16.04 AS build-lh
RUN apt-get update
RUN apt-get install -y git curl libtinfo-dev

## stack
RUN curl -sSL https://get.haskellstack.org/ | sh

## ghc
RUN apt-get install -y software-properties-common
RUN add-apt-repository ppa:hvr/ghc
RUN apt-get update
RUN apt-get install -y ghc-8.2.1

## liquidhaskell
WORKDIR /root/.stack/global-project/
COPY stack.yaml .
RUN git clone --recursive https://github.com/ucsd-progsys/liquidhaskell.git
WORKDIR liquidhaskell

### ver 0.8.2.0
RUN git checkout -b 0f122cbd9983ce9fe65389474be9b36f9ea86b59
ENV PATH /opt/ghc/bin:$PATH
RUN stack build --system-ghc
RUN stack install --system-ghc

### cabal
RUN apt-get install -y cabal-install-2.0
RUN cabal update
RUN cabal install vector

# COMPRESS WITH UPX
RUN apt-get install -y upx-ucl
# RUN upx --best --ultra-brute /root/.local/bin/liquid
# RUN upx --best --ultra-brute /root/.local/bin/fixpoint
# RUN upx --best --ultra-brute /opt/ghc/bin/ghc
# RUN upx --best --ultra-brute /opt/ghc/bin/cabal
# RUN upx --best --ultra-brute /usr/bin/gcc

# FROM fpco/haskell-scratch
FROM ubuntu:16.04
ENV PATH /opt/ghc/bin/:/opt/cabal/2.0/bin/:$PATH

COPY --from=build-z3 /usr/bin/z3 /usr/bin/z3
COPY --from=build-lh /root/.local/bin/liquid /usr/local/bin/liquid
COPY --from=build-lh /root/.local/bin/fixpoint /usr/local/bin/fixpoint
COPY --from=build-lh /root/.cabal/ /root/.cabal/
COPY --from=build-lh /etc/alternatives/ /etc/alternatives/
COPY --from=build-lh /opt/ghc/ /opt/ghc/
COPY --from=build-lh /opt/cabal/ /opt/cabal/
COPY --from=build-lh /usr/bin/gcc /usr/bin/gcc

# include
COPY --from=build-lh /root/.stack/global-project/liquidhaskell/.stack-work/install/x86_64-linux/nightly-2017-10-06/8.2.1/share/ /root/.stack/global-project/liquidhaskell/.stack-work/install/x86_64-linux/nightly-2017-10-06/8.2.1/share/

# lib
COPY --from=build-lh /lib/x86_64-linux-gnu/libpthread.so.0 /lib/x86_64-linux-gnu/libpthread.so.0
COPY --from=build-lh /lib/x86_64-linux-gnu/librt.so.1 /lib/x86_64-linux-gnu/librt.so.1
COPY --from=build-lh /usr/lib/x86_64-linux-gnu/libstdc++.so.6 /usr/lib/x86_64-linux-gnu/libstdc++.so.6
COPY --from=build-lh /lib/x86_64-linux-gnu/libm.so.6 /lib/x86_64-linux-gnu/libm.so.6
COPY --from=build-lh /usr/lib/x86_64-linux-gnu/libgomp.so.1 /usr/lib/x86_64-linux-gnu/libgomp.so.1
COPY --from=build-lh /lib/x86_64-linux-gnu/libgcc_s.so.1 /lib/x86_64-linux-gnu/libgcc_s.so.1
COPY --from=build-lh /lib/x86_64-linux-gnu/libc.so.6 /lib/x86_64-linux-gnu/libc.so.6
COPY --from=build-lh /lib64/ld-linux-x86-64.so.2 /lib64/ld-linux-x86-64.so.2
COPY --from=build-lh /lib/x86_64-linux-gnu/libdl.so.2 /lib/x86_64-linux-gnu/libdl.so.2

COPY --from=build-lh /lib/x86_64-linux-gnu/libtinfo.so.5 /lib/x86_64-linux-gnu/libtinfo.so.5
COPY --from=build-lh /lib/x86_64-linux-gnu/libutil.so.1 /lib/x86_64-linux-gnu/libutil.so.1
COPY --from=build-lh /usr/lib/x86_64-linux-gnu/libgmp.so.10 /usr/lib/x86_64-linux-gnu/libgmp.so.10

COPY --from=build-lh /usr/lib/x86_64-linux-gnu/libgmp.so /usr/lib/x86_64-linux-gnu/libgmp.so

WORKDIR /tmp/liquid12