FROM alpine:latest

RUN apk add --update git python2 py2-pip py2-impacket py2-crypto py2-asn1 \
    && git clone https://github.com/zerosum0x0/koadic.git /koadic \
    && pip2 install --no-cache-dir -r /koadic/requirements.txt \
    && apk del --purge py2-pip

WORKDIR /koadic
ENTRYPOINT ["python", "koadic"]