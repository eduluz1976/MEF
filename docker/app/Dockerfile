FROM alpine:edge


WORKDIR /app
# Add repos
RUN echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories


RUN apk update \
    && apk upgrade \
    && apk add --update util-linux  \
    && apk add bash apache2 php7-apache2 curl ca-certificates \
               openssl openssh git php7 php7-phar php7-json   \
               php7-iconv php7-openssl tzdata openntpd unzip  \
               zip php7-zip php7-fileinfo php7-dom php7-xml   \
               php7-xmlwriter php7-tokenizer php7-session     \
               php7-mbstring php7-sqlite3 php7-xdebug php7-curl


# Add apache to run and configure
RUN sed -i "s/#LoadModule\ rewrite_module/LoadModule\ rewrite_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ session_module/LoadModule\ session_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ session_cookie_module/LoadModule\ session_cookie_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ session_crypto_module/LoadModule\ session_crypto_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ deflate_module/LoadModule\ deflate_module/" /etc/apache2/httpd.conf \
    && sed -i "s#^DocumentRoot \".*#DocumentRoot \"/app/public\"#g" /etc/apache2/httpd.conf \
    && sed -i "s#/var/www/localhost/htdocs#/app/public#" /etc/apache2/httpd.conf \
    && printf "\n<Directory \"/app/public\">\n\tAllowOverride All\n</Directory>\n" >> /etc/apache2/httpd.conf


RUN cp /usr/bin/php7 /usr/bin/php \
    && rm -f /var/cache/apk/*


# Add Composer
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer


ADD ./docker/app/run.sh /
ADD ./app/ /app/


RUN cd /app && composer update && chown -R apache:apache /app


RUN chmod +x /run.sh

EXPOSE 80
ENTRYPOINT ["/run.sh"]
