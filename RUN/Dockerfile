FROM alpine:latest

RUN echo "http://dl-cdn.alpinelinux.org/alpine/latest-stable/main" > /etc/apk/repositories && \
    apk update && apk add python3

RUN apk update && apk add python3 

RUN python3 --version

CMD [ "google.com" ]

ENTRYPOINT [ "ping" ,"-c", "5" ]


