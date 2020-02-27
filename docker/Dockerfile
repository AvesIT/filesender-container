FROM php:7.4-fpm-buster
LABEL version="0.1" \
      license="GPL2" \
      maintainer="Nils Vogels <n.vogels@aves-it.nl>" \
      description="Filesender and simplesamlphp based on php7-fpm" \
      documentation="Currently lacking" 

ENV FILESENDER_VERSION=2.15 \
    FILESENDER_SUM="111a630d778a5670ded8fc6d4bddff021375beaf160d1204e928937c0182444c" \
    SSP_VERSION=1.18.4 \
    SSP_SUM="7530dec7290ba5efaac08cb17042819a96dc530e217c3810cdde9be76d57b2ca" 

RUN cd /opt && mkdir filesender && \
    curl -kL https://github.com/filesender/filesender/archive/master-filesender-${FILESENDER_VERSION}.tar.gz | tar xz && \
    ln -s filesender-master-filesender-${FILESENDER_VERSION} filesender && \
    curl -L https://github.com/simplesamlphp/simplesamlphp/releases/download/v${SSP_VERSION}/simplesamlphp-${SSP_VERSION}.tar.gz | tar xz && \
    ln -s simplesamlphp-${SSP_VERSION} simplesamlphp

RUN apt-get update && \
    apt-get dist-upgrade -y && \
    apt-get autoremove -y && \
    apt-get clean && \ 
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN chown -R www-data:www-data /opt/filesender/*

VOLUME ["/opt/filesender"]

CMD ["/entrypoint.sh"]
