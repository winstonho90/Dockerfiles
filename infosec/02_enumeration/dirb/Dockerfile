FROM alpine:latest

WORKDIR /root
RUN apk add --no-cache curl g++ make curl-dev wget \
    # Download the latest version from Github 
    && wget --no-check-certificate https://nchc.dl.sourceforge.net/project/dirb/dirb/2.22/dirb222.tar.gz -O - | tar xz \
    # Install dirb and do post-install
    && cd dirb222/ \
    && chmod +x -R . \
    && ./configure \
    && make -j \
    && cp dirb /bin \
    && cp -r wordlists/ /usr/share/dirb/wordlists \
    # Cleanup unneeded packages
    && apk del --purge g++ make curl-dev wget \
    && rm -rf ../dirb222/

ENTRYPOINT ["dirb"]
