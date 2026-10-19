FROM alpine:3.24

RUN apk add --no-cache \
    curl \
    jq

ENV SLEEP_SECONDS=30

CMD exec /bin/sh -c "sleep $SLEEP_SECONDS"