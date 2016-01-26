FROM golang:1.5.3-alpine
MAINTAINER "Brandon Matthews <bmatt@optimaltour.us>"

RUN adduser -S tsdbrelay

ENV TSDBRELAY_REPO https://github.com/bosun-monitor/bosun.git
ENV TSDBRELAY_TAG master

RUN apk update && apk add git
RUN go get bosun.org/cmd/tsdbrelay
RUN cd /go/src/bosun.org && \
    git remote set-url origin ${TSDBRELAY_REPO} && \
    git fetch && \
    git checkout ${TSDBRELAY_TAG}
RUN go build bosun.org/cmd/tsdbrelay

EXPOSE 4242

# net.Dial doesn't respect /etc/hosts by default
ENV GODEBUG netdns=cgo

USER tsdbrelay
ENTRYPOINT /go/tsdbrelay -b bosun:8070 -t tsdb:4242
