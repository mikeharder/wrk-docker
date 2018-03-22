FROM arm32v7/buildpack-deps:stretch-scm as build

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
# wrk dependencies
        build-essential \
        libssl-dev \
    && rm -rf /var/lib/apt/lists/*

# Build wrk
ENV WRK_VERSION 4.0.2
RUN cd /tmp \
    && wget https://github.com/wg/wrk/archive/$WRK_VERSION.tar.gz -O wrk.tar.gz \
    && tar xvzf wrk.tar.gz \
    && cd wrk-$WRK_VERSION \
    && make

FROM arm32v7/debian:stretch-slim
ENV WRK_VERSION 4.0.2
COPY --from=build /tmp/wrk-$WRK_VERSION/wrk /usr/local/bin
ENTRYPOINT ["wrk"]
