FROM golang:1.13.6-alpine3.11 as buildergo
RUN mkdir -p /fluent-bit
RUN mkdir -p /code
COPY . /code/
WORKDIR /code
RUN echo $(ls -al /code)
RUN CGO_ENABLED=0 go build -i -ldflags '-w -s' -o /fluent-bit/fluent-bit /code/cmd/fluent-bit-watcher/main.go

FROM fluent/fluent-bit:1.8.3
LABEL Description="Fluent Bit docker image" Vendor="KubeSphere" Version="1.0"

COPY conf/fluent-bit.conf conf/parsers.conf /fluent-bit/etc/
COPY --from=buildergo /fluent-bit/fluent-bit /fluent-bit/bin/fluent-bit-watcher

# Entry point
ENTRYPOINT ["/fluent-bit/bin/fluent-bit-watcher"]
