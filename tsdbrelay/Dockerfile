FROM golang:1.5.3-alpine
MAINTAINER "Brandon Matthews <bmatt@optimaltour.us>"

RUN adduser -S tsdbrelay

RUN apk update && apk add git
RUN go get bosun.org/cmd/tsdbrelay && go build bosun.org/cmd/tsdbrelay

EXPOSE 4242

# net.Dial doesn't respect /etc/hosts by default
ENV GODEBUG netdns=cgo

USER tsdbrelay
ENTRYPOINT /go/tsdbrelay -b bosun:8070 -t tsdb:4242
