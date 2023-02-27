FROM alpine:3.17.0

RUN apk add wireguard-tools curl

ENTRYPOINT wg-quick up /etc/wireguard/clients/wg0.conf && while true; do echo 'Hit CTRL+C'; sleep 1; done
