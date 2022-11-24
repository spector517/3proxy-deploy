FROM alpine:3.17.0

RUN apk add openvpn curl

ENTRYPOINT [ "openvpn", "--config", "/etc/openvpn/clients/client.ovpn" ]
