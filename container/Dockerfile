FROM alpine:latest

# Install latest version of bind and upgrade container packages with no cache.
RUN apk --update --no-cache upgrade && \
    apk --update --no-cache add bind


# Add entrypoint script

COPY entrypoint.sh /
RUN chmod 750 /entrypoint.sh

VOLUME ["/etc/bind", "/var/named", "/var/log/named"]

EXPOSE 53 53/udp
WORKDIR /etc/bind

ENTRYPOINT ["/entrypoint.sh"]
