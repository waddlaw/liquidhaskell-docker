# z3
FROM haskell:8.2.1 AS build-z3
RUN apt-get update
RUN apt-get install -y make python
RUN git clone https://github.com/Z3Prover/z3.git
WORKDIR z3
RUN python scripts/mk_make.py
WORKDIR build
RUN make
RUN make install

# COMPRESS WITH UPX
RUN apt-get install -y upx-ucl
RUN upx --best --ultra-brute /usr/bin/z3

# liquidhaskell
FROM haskell:8.2.1 AS build-lh
WORKDIR /root/.stack/global-project/
COPY stack.yaml .
RUN git clone --recursive https://github.com/ucsd-progsys/liquidhaskell.git
WORKDIR liquidhaskell
# ver 0.8.2.0
RUN git checkout -b 0f122cbd9983ce9fe65389474be9b36f9ea86b59
RUN stack install

FROM haskell:8.2.1
COPY --from=build-z3 /usr/bin/z3 /usr/bin/z3
COPY --from=build-lh /root/.local/bin/liquid /usr/local/bin/liquid
COPY --from=build-lh /root/.local/bin/fixpoint /usr/local/bin/fixpoint
COPY --from=build-lh /root/.stack/global-project/liquidhaskell/.stack-work/install/x86_64-linux/nightly-2017-10-06/8.2.1/share/x86_64-linux-ghc-8.2.1/liquidhaskell-0.8.2.0/ /root/.stack/global-project/liquidhaskell/.stack-work/install/x86_64-linux/nightly-2017-10-06/8.2.1/share/x86_64-linux-ghc-8.2.1/liquidhaskell-0.8.2.0/
RUN cabal update
RUN cabal install vector