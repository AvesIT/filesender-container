FROM php:7.4-fpm-buster
LABEL version="0.2" \
      license="GPL2" \
      maintainer="Nils Vogels <n.vogels@aves-it.nl>" \
      description="Filesender and simplesamlphp based on php7-fpm" \
      documentation="Currently lacking" 

ENV FILESENDER_VERSION=2.15 \
    FILESENDER_SUM="111a630d778a5670ded8fc6d4bddff021375beaf160d1204e928937c0182444c" \
    SSP_VERSION=1.18.4 \
    SSP_SUM="7530dec7290ba5efaac08cb17042819a96dc530e217c3810cdde9be76d57b2ca" 

RUN cd /opt && mkdir filesender && \
    cd /opt/filesender && \
    curl -kL https://github.com/filesender/filesender/archive/master-filesender-${FILESENDER_VERSION}.tar.gz | tar xz && \
    ln -s filesender-master-filesender-${FILESENDER_VERSION} filesender && \
    curl -L https://github.com/simplesamlphp/simplesamlphp/releases/download/v${SSP_VERSION}/simplesamlphp-${SSP_VERSION}.tar.gz | tar xz && \
    ln -s simplesamlphp-${SSP_VERSION} simplesamlphp

RUN cd /opt/filesender/filesender && \
    cp config/config_sample.php config/config.php && \
    mkdir -p tmp files log && \
    chmod o-rwx tmp files log config/config.php && \
    chown www-data:www-data tmp files log && \
    chgrp www-data config/config.php && \
    cd /opt/filesender/simplesamlphp && \
    cp -r config-templates/*.php config/ && \
    cp -r metadata-templates/*.php metadata/

RUN mkdir -p /config/fpm /config/filesender /config/simplesamlphp/config /config/simplesamlphp/metadata && \
    rm -f /usr/local/etc/php-fpm.d/filesender.conf && \
    rm -f /opt/filesender/filesender/config/config.php && \
    rm -f /opt/filesender/simplesamlphp/config/config.php && \
    rm -f /opt/filesender/simplesamlphp/config/acl.php && \
    rm -f /opt/filesender/simplesamlphp/config/authsource.php && \
    rm -f /opt/filesender/simplesamlphp/metadata/active.php && \
    ln -s /config/fpm/www.conf /usr/local/etc/php-fpm.d/filesender.conf && \
    ln -s /config/filesender/config.php /opt/filesender/filesender/config/config.php && \
    ln -s /config/simplesamlphp/config/acl.php /opt/filesender/simplesamlphp/config/acl.php && \
    ln -s /config/simplesamlphp/config/authsource.php /opt/filesender/simplesamlphp/config/authsource.php && \
    ln -s /config/simplesamlphp/config/config.php /opt/filesender/simplesamlphp/config/config.php && \
    ln -s /config/simplesamlphp/metadata/active.php /opt/filesender/simplesamlphp/metadata/active.php
 
RUN apt-get update && \
    apt-get dist-upgrade -y && \
    apt-get autoremove -y && \
    apt-get clean && \ 
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
    
VOLUME ["/opt/filesender"]
VOLUME ["/config/fpm"]
VOLUME ["/config/filesender/config"]
VOLUME ["/config/simplesamlphp/config"]
VOLUME ["/config/simplesamlphp/metadata"]

# We keep entrypoint, expose and cmd as they were in base image, they are listed below for completeness
# ENTRYPOINT ["docker-php-entrypoint"]
# EXPOSE 9000
# CMD ["php-fpm"]
