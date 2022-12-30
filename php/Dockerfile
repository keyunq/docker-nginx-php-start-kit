FROM php:7.3-fpm
 
# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 替换源
RUN sed -i "s@http://deb.debian.org@http://mirrors.aliyun.com@g" /etc/apt/sources.list && rm -Rf /var/lib/apt/lists/* &&  cat /etc/apt/sources.list
 
# 更新安装依赖包和PHP核心拓展
RUN apt-get update && apt-get install -y \
        --no-install-recommends libfreetype6-dev libjpeg62-turbo-dev libpng-dev curl libzip-dev libpq-dev \
        && rm -r /var/lib/apt/lists/* \
        && docker-php-ext-configure gd \
        && docker-php-ext-install -j$(nproc) gd bcmath opcache pdo_mysql pdo_pgsql gettext sockets zip mysqli dba pcntl calendar shmop exif sysvmsg sysvsem sysvshm 
 
# 安装 PECL 拓展，安装Redis，mongodb，amqp
RUN pecl install redis \
    && docker-php-ext-enable redis

RUN pecl install mongodb \
   && docker-php-ext-enable mongodb
 
# 安装 Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH

RUN cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini
 
WORKDIR /data