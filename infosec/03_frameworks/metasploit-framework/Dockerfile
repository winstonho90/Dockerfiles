FROM alpine:latest

# Usage: docker run -it \
#   -e DATABASE_URL=postgres://postgres@db:5432/msf \
#   -v $HOME/.msf4:/home/msf/.msf4 \
#   --name msfconsole metasploit-framework

ENV PATH=$PATH:/usr/share/metasploit-framework 
ENV NOKOGIRI_USE_SYSTEM_LIBRARIES=1

ARG BUNDLER_ARGS="--jobs=8 --without development test coverage"
ENV MSF_USER msf
ENV NMAP_PRIVILEGED=""

RUN apk add --update \
        ruby \
        ruby-bigdecimal \
        ruby-bundler \
        libxslt \
        nmap \
        ncurses \
        # Added based on rapid7's repo
        sqlite-libs \
        nmap-scripts \
        nmap-nselibs \
        postgresql-libs \
        libpcap \
        # Build Dependencies
    && apk add --virtual .ruby-builddeps \
        autoconf \
        bison \
        build-base \
        ruby-dev \
        libressl-dev \
        readline-dev \
        sqlite-dev \
        postgresql-dev \
        libpcap-dev \
        libxml2-dev \
        libxslt-dev \
        yaml-dev \
        zlib-dev \
        ncurses-dev \
        git \
        # To fix missing ffi errors
        libffi-dev \
    # Clone the MSF repository
    && cd /usr/share \
    && git clone https://github.com/rapid7/metasploit-framework.git \
    && cd /usr/share/metasploit-framework \
    # Bundle install gem dependencies
    && echo "gem: --no-ri --no-rdoc" > /etc/gemrc \
    && bundle install --system $BUNDLER_ARGS \
    # Cleanup
    && apk del --purge .ruby-builddeps \
    && rm -rf /var/cache/apk/* \
    # Add msf user
    && adduser -g msfconsole -D $MSF_USER \
    && chown $MSF_USER:msfconsole /home/$MSF_USER/.msf4

USER $MSF_USER
WORKDIR /usr/share

CMD ["metasploit-framework/msfconsole", "-r", "metasploit-framework/docker/msfconsole.rc"]
