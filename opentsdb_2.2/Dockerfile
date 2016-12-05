FROM opower/opentsdb

RUN cd /opt/opentsdb && \
    git fetch --tag && \
    git checkout tags/v2.2.0 && \
    ./build.sh
