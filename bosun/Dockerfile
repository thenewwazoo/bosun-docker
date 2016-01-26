FROM golang:1.5.3-alpine
MAINTAINER "Brandon Matthews <bmatt@optimaltour.us>"

ENV BOSUN_REPO https://github.com/bosun-monitor/bosun.git
ENV BOSUN_TAG master

RUN adduser -S bosun
RUN mkdir -p /var/cache/bosun && chown bosun /var/cache/bosun

RUN apk update && apk add git
RUN go get bosun.org/cmd/bosun
RUN cd /go/src/bosun.org && \
    git remote set-url origin ${BOSUN_REPO} && \
    git fetch && \
    git checkout ${BOSUN_TAG}
RUN go build bosun.org/cmd/bosun

ADD bosun.conf /etc/bosun.conf

# net.Dial doesn't respect /etc/hosts by default
ENV GODEBUG netdns=cgo

USER bosun
WORKDIR /var/cache/bosun/
VOLUME /var/cache/bosun/
EXPOSE 8070
ENTRYPOINT ["/go/bin/bosun", "-c", "/etc/bosun.conf"]
