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

FROM debian:stretch-slim
ENV WRK_VERSION 4.1.0
COPY --from=build /tmp/wrk-$WRK_VERSION/wrk /usr/local/bin
ENTRYPOINT ["wrk"]
