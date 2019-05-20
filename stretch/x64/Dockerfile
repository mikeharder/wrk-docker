FROM buildpack-deps:stretch as build

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
# wrk dependencies
        build-essential \
        libssl-dev \
    && rm -rf /var/lib/apt/lists/*

# Build wrk
ENV WRK_VERSION 4.1.0
RUN cd /tmp \
    && wget https://github.com/wg/wrk/archive/$WRK_VERSION.tar.gz -O wrk.tar.gz \
    && tar xvzf wrk.tar.gz \
    && cd wrk-$WRK_VERSION \
    && make

# Build wrk2
RUN cd /tmp \
    && git clone https://github.com/giltene/wrk2 \
    && cd wrk2 \
    && make


FROM debian:stretch-slim

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
# wrk2 dependencies
        libssl1.1 \
    && rm -rf /var/lib/apt/lists/*

ENV WRK_VERSION 4.1.0
COPY --from=build /tmp/wrk-$WRK_VERSION/wrk /usr/local/bin
COPY --from=build /tmp/wrk2/wrk /usr/local/bin/wrk2
ENTRYPOINT ["wrk"]
