FROM alpine:latest

RUN apk add openvpn curl

ENTRYPOINT [ "openvpn", "--config", "/etc/openvpn/clients/client.ovpn" ]
