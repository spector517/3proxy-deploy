FROM alpine:latest

RUN apk add openvpn curl && \
    echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.d/ipv4.conf

ENTRYPOINT [ "openvpn", "--config", "/etc/openvpn/clients/client.ovpn" ]
