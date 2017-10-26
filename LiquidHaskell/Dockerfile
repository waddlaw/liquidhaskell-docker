FROM ubuntu:16.04 AS build-lh
RUN apt-get update && apt-get install -y \
      curl \
      libtinfo-dev
RUN curl -sSL https://get.haskellstack.org/ | sh
RUN git clone --recursive https://github.com/ucsd-progsys/liquidhaskell.git
WORKDIR liquidhaskell
RUN stack setup
RUN stack build
RUN stack install

FROM ubuntu:16.04
RUN apt-get update && apt-get install -y --no-install-recommends \
      libgmp-dev \
      z3
COPY --from=build-lh /root/.local/bin/liquid /usr/local/bin/liquid
COPY --from=build-lh /root/.local/bin/fixpoint /usr/local/bin/fixpoint
COPY --from=build-lh /root/.local/bin/gradual /usr/local/bin/gradual
COPY --from=build-lh /root/.local/bin/target /usr/local/bin/target
