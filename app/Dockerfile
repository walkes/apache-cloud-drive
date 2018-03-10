FROM debian:jessie

ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_RUN_DIR /var/run/apache2

RUN apt-get update && \
    apt-get install -y apache2 && \
    a2enmod dav dav_fs autoindex && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir /var/lock/apache2 && \
    mkdir -p /mnt/storage && \
    chown -R $APACHE_RUN_USER:$APACHE_RUN_GROUP /var/www /mnt/storage && \
    chmod -R a+rw /var/www /mnt/storage

ADD https://github.com/ramlmn/Apache-Directory-Listing/archive/v2.0.tar.gz /tmp/directory-listing.tar.gz
WORKDIR /tmp
RUN tar xvzf /tmp/directory-listing.tar.gz && \
    mv /tmp/Apache-Directory-Listing-2.0/directory-listing /var/www/html/directory-listing && \
    rm /tmp/directory-listing.tar.gz && \
    rm -rf /tmp/Apache-Directory-Listing-2.0 && \
    sed -i 's/{SOME_FOLDER}/directory-listing/g' /var/www/html/directory-listing/.htaccess && \
    sed -i 's/{VIEW}/table/g' /var/www/html/directory-listing/.htaccess

COPY webdav.conf /etc/apache2/sites-available/

RUN a2ensite webdav && \
    a2dissite 000-default

VOLUME /mnt/storage

EXPOSE 80
CMD cp -n /var/www/html/directory-listing/.htaccess /mnt/storage && apache2 -DFOREGROUND
