FROM wunderseltsam/constraint-solvers AS build-z3

#FROM alpine:3.6 AS build-lh
FROM nnwww/haskell-stack-link:latest AS build-lh
# INSTALL BASIC DEV TOOLS, GHC, GMP & ZLIB
RUN apk update
RUN apk add cabal
ENV PATH ${PATH}:/root/.cabal/bin
#RUN apk add alpine-sdk git ca-certificates ghc gmp-dev zlib-dev
# GRAB A RECENT BINARY OF STACK
#RUN curl -sSL https://get.haskellstack.org/ | sh
# FIX https://bugs.launchpad.net/ubuntu/+source/gcc-4.4/+bug/640734
#WORKDIR /usr/lib/gcc/x86_64-alpine-linux-musl/6.3.0/
#RUN cp crtbeginT.o crtbeginT.o.orig
#RUN cp crtbeginS.o crtbeginT.o
# COMPILE
#WORKDIR /
RUN git clone --recursive https://github.com/ucsd-progsys/liquidhaskell.git
WORKDIR liquidhaskell
RUN git checkout master
COPY liquidhaskell.cabal ./
COPY liquid-fixpoint.cabal ./liquid-fixpoint/
RUN stack clean; stack install --split-objs --ghc-options="-fPIC"
#RUN cabal sandbox init
#RUN cabal update
#RUN cabal sandbox add-source ./liquid-fixpoint
#RUN cabal install --disable-shared
#RUN mkdir /usr/include/liquid
#RUN stack --local-bin-path /sbin install --system-ghc --ghc-options '-optl-static -optl-pthread -fPIC'
# COMPRESS WITH UPX
#RUN echo http://dl-cdn.alpinelinux.org/alpine/edge/community/ >> /etc/apk/repositories
#RUN apk --update add upx=3.94-r0
#RUN upx --best --ultra-brute /sbin/fixpoint
#RUN upx --best --ultra-brute /sbin/liquid
#RUN upx --best --ultra-brute /sbin/target
# SHOW INFORMATION
#RUN ldd /sbin/fixpoint || true
#RUN du -hs /sbin/fixpoint
#RUN ldd /sbin/liquid || true
#RUN du -hs /sbin/liquid
#RUN ldd /sbin/target || true
#RUN du -hs /sbin/target
COPY --from=build-z3 /usr/local/bin/z3 /sbin/z3



#FROM alpine:3.6
#RUN apk update
#RUN apk add alpine-sdk git ca-certificates ghc gmp-dev zlib-dev
#COPY --from=build-lh /root/.local/bin/liquid /sbin/liquid
#COPY --from=build-lh /root/.local/bin/fixpoint /sbin/fixpoint
#COPY --from=build-lh /root/.local/bin/target /sbin/target
#COPY --from=build-lh /liquidhaskell/include/* /liquidhaskell/.stack-work/install/x86_64-linux/lts-8.9/8.0.2/share/x86_64-linux-ghc-8.0.2/liquidhaskell-0.8.0.5/include/
#COPY --from=build-z3 /usr/local/bin/z3 /sbin/z3

#
#FROM ubuntu:16.04 AS build-lh
#RUN apt-get update && apt-get install -y \
#      curl \
#      libtinfo-dev
#RUN curl -sSL https://get.haskellstack.org/ | sh
#RUN git clone --recursive https://github.com/ucsd-progsys/liquidhaskell.git
#WORKDIR liquidhaskell
#RUN stack setup
#RUN stack build --ghc-options='-optl-static -optl-pthread -fPIC'
#RUN stack install
#
#FROM ubuntu:16.04
#RUN apt-get update && apt-get install -y --no-install-recommends \
#      libgmp-dev \
#      sudo \
#      z3 \
#    && apt-get clean
#COPY --from=build-lh /root/.local/bin/liquid /usr/local/bin/liquid
#COPY --from=build-lh /root/.local/bin/fixpoint /usr/local/bin/fixpoint
#COPY --from=build-lh /root/.local/bin/gradual /usr/local/bin/gradual
#COPY --from=build-lh /root/.local/bin/target /usr/local/bin/target
#
#RUN useradd -m -d /home/liquid -s /bin/bash liquid
#RUN echo "liquid ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/liquid && chmod 0440 /etc/sudoers.d/liquid
#ENV HOME /home/liquid
#WORKDIR /home/liquid
#USER liquid
