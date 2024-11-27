ARG BUILD_FROM
FROM $BUILD_FROM as builder

RUN apk update && apk add --no-cache \
	g++ \
	gcc \
	git \
	libpcap-dev

WORKDIR /tmp
ADD https://api.github.com/repos/remarkov/phev2mqtt/git/refs/heads/debug version.json
RUN git clone -b debug https://github.com/remarkov/phev2mqtt.git
COPY --from=golang:alpine /usr/local/go/ /usr/local/go/
RUN cd /tmp/phev2mqtt && \
    /usr/local/go/bin/go build

FROM $BUILD_FROM
RUN apk update && apk add --no-cache \
	libpcap-dev

COPY --from=builder /tmp/phev2mqtt/phev2mqtt /opt/phev2mqtt

COPY run.sh /opt
RUN chmod a+x /opt/run.sh

CMD [ "/opt/run.sh" ]
