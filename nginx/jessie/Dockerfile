FROM debian:jessie

ENV NGINX_VERSION 1.10.1-1~jessie

RUN apt-get update && \
    apt-get install \
        --no-install-recommends \
        --no-install-suggests \
        --yes \
            locales \
            ntp && \
    cat /dev/null > /etc/locale.gen && \
    echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen && \
    echo "ru_RU.UTF-8 UTF-8" >> /etc/locale.gen && \
    /usr/sbin/locale-gen && \
    cp /usr/share/zoneinfo/Europe/Kiev /etc/localtime

RUN apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62 && \
    echo "deb http://nginx.org/packages/debian/ jessie nginx" >> /etc/apt/sources.list && \
    apt-get update && \
    apt-get install \
        --no-install-recommends \
        --no-install-suggests \
        --yes \
            ca-certificates \
            iptables \
            supervisor \
            nginx=${NGINX_VERSION} \
            nginx-module-xslt \
            nginx-module-geoip \
            nginx-module-image-filter \
            nginx-module-perl \
            nginx-module-njs \
            gettext-base

RUN ln -sf /dev/stdout /var/log/nginx/access.log && \
    ln -sf /dev/stderr /var/log/nginx/error.log

# COPY ./include/iptables.conf /etc/iptables.conf
# RUN chmod +x /etc/iptables.conf

COPY ./include/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

RUN rm -rf /var/lib/apt/lists/*

EXPOSE 80 443

CMD ["/usr/bin/supervisord"]
