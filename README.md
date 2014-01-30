compile-nginx-master-all-modules Centos 6.x
================================

This module compile latest nginx version and complementary modules on centos 6.x


## Installation dependence

    cd /usr/local/src
    yum -y groupinstall "Development Tools"
    yum -y install gcc gcc-c++ kernel-devel
    yum -y install zlib-devel
    yum -y install cpio
    yum -y install expat-devel
    yum -y install gettext-devel
    yum -y install libxslt
    yum -y install libxslt-devel
    yum -y install gd gd-devel
    yum -y install perl-ExtUtils-Embed
    yum -y install openssl
    yum -y install openssl-devel
    
## Download latest Pcre 8.34

    cd /usr/local/src
    wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.34.tar.gz
    tar -xzvf pcre-8.34.tar.gz
    cd pcre-8.34/
    ./configure # /usr/local is the default so no need to prefix
    make
    sudo make install
    sudo ldconfig # this is important otherwise nginx will compile but fail to load
    
## Download latest stable Luajit

    cd /usr/local/src
    git clone http://luajit.org/git/luajit-2.0.git

Update luajit

    cd luajit-2.0/
    git pull
    
Install

    cd /usr/local/src/luajit-2.0/
    make
    make install
    lua
    ln -sf luajit-2.0.0-beta10 /usr/local/bin/luajit
    export LUA_LIB=/usr/local/lib/
    export LUA_INC=/usr/local/include/luajit-2.0/
    ln -s /usr/local/lib/libluajit-5.1.so.2.0.0 /usr/local/lib/liblua.so
    
##Install MaxMind C API
Type the following commands to install MaxMind C API:

    yum -y install zlib-devel
    cd /usr/local/src
    wget http://geolite.maxmind.com/download/geoip/api/c/GeoIP.tar.gz
    tar -zxvf GeoIP.tar.gz
    cd GeoIP-1.4.8
    ./configure
    make
    make install

You need to configure dynamic linker run time bindings as follows:

    echo '/usr/local/lib' > /etc/ld.so.conf.d/geoip.conf

Run ldconfig to activate configuration:

    ldconfig
    ldconfig -v | less

## Create master nginx-master directory and nginx-modules

    mkdir /usr/local/src/nginx-master
    mkdir /usr/local/src/nginx-master/nginx-modules
    
## Git latest nginx-master

    cd /usr/local/src/nginx-master
    git clone --depth 1 https://github.com/nginx/nginx
    
Update nginx-master

    cd /usr/local/src/nginx-master/nginx
    git pull

## Git memc-nginx-module

    cd /usr/local/src/nginx-master/nginx-modules
    git clone --depth 1 https://github.com/agentzh/memc-nginx-module
    
Update memc-nginx-module

    cd /usr/local/src/nginx-master/nginx-modules/memc-nginx-module
    git pull

## Git nginx-rtmp-module

    cd /usr/local/src/nginx-master/nginx-modules/
    git clone --depth 1 https://github.com/arut/nginx-rtmp-module
    
Update nginx-rtmp-module

    cd /usr/local/src/nginx-master/nginx-modules/nginx-rtmp-module
    git pull

## Git ngx_devel_kit

    cd /usr/local/src/nginx-master/nginx-modules
    git clone --depth 1 https://github.com/simpl/ngx_devel_kit
    
Update ngx_devel_kit

    cd /usr/local/src/nginx-master/nginx-modules/ngx_devel_kit
    git pull

## Git nginx-upload-progress-module

    cd /usr/local/src/nginx-master/nginx-modules
    git clone --depth 1 https://github.com/masterzen/nginx-upload-progress-module
    
Update nginx-upload-progress-module

    cd /usr/local/src/nginx-master/nginx-modules/nginx-upload-progress-module
    git pull

## Git lua-nginx-module

    cd /usr/local/src/nginx-master/nginx-modules
    git clone --depth 1 https://github.com/chaoslawful/lua-nginx-module
    
Update lua-nginx-module

    cd /usr/local/src/nginx-master/nginx-modules/lua-nginx-module
    git pull
    
## Git latest ngx_pagespeed

https://github.com/pagespeed/ngx_pagespeed

    cd /usr/local/src/nginx-master/nginx-modules
    git clone --depth 1  https://github.com/pagespeed/ngx_pagespeed
    
Update ngx_pagespeed

    cd /usr/local/src/nginx-master/nginx-modules/ngx_pagespeed
    git pull
    wget https://dl.google.com/dl/page-speed/psol/1.7.30.2.tar.gz
    tar -xzvf 1.7.30.2.tar.gz # expands to psol/

## Compile Nginx with :
lua-nginx-module

memc-nginx-module

nginx-rtmp-module-master

nginx-upload-progress-module-master

ngx_devel_kit

ngx_pagespeed

    cd /usr/local/src/nginx-master/nginx
    ./configure \
    --prefix=/etc/nginx \
    --conf-path=/etc/nginx/nginx.conf \
    --sbin-path=/usr/sbin/nginx \
    --lock-path=/var/lock/nginx.lock \
    --pid-path=/var/run/nginx.pid \
    --user=nobody \
    --group=nobody \
    --http-log-path=/var/log/nginx/access.log \
    --error-log-path=/var/log/nginx/error.log \
    --http-client-body-temp-path=/var/lib/nginx/body \
    --http-fastcgi-temp-path=/var/lib/nginx/fastcgi \
    --http-log-path=/var/log/nginx/access.log \
    --http-proxy-temp-path=/var/lib/nginx/proxy \
    --http-scgi-temp-path=/var/lib/nginx/scgi \
    --http-uwsgi-temp-path=/var/lib/nginx/uwsgi \
    --with-pcre=/usr/local/src/pcre-8.34 \
    --with-debug \
    --with-http_addition_module \
    --with-http_dav_module \
    --with-http_geoip_module \
    --with-http_gzip_static_module \
    --with-http_image_filter_module \
    --with-http_realip_module \
    --with-http_stub_status_module \
    --with-http_ssl_module \
    --with-http_sub_module \
    --with-http_xslt_module \
    --with-ipv6 \
    --with-sha1=/usr/include/openssl \
    --with-md5=/usr/include/openssl \
    --with-mail \
    --with-file-aio \
    --with-mail_ssl_module \
    --with-http_perl_module \
    --with-http_flv_module \
    --with-http_gunzip_module \
    --with-http_image_filter_module \
    --with-http_mp4_module \
    --with-http_random_index_module \
    --with-http_secure_link_module \
    --with-http_stub_status_module \
    --with-http_sub_module \
    --with-http_dav_module \
    --with-http_xslt_module \
    --with-mail \
    --without-mail_pop3_module \
    --without-mail_imap_module \
    --without-mail_smtp_module \
    --with-mail_ssl_module \
    --add-module=/usr/local/src/nginx-master/nginx-modules/lua-nginx-module \
    --add-module=/usr/local/src/nginx-master/nginx-modules/memc-nginx-module \
    --add-module=/usr/local/src/nginx-master/nginx-modules/nginx-rtmp-module \
    --add-module=/usr/local/src/nginx-master/nginx-modules/nginx-upload-progress-module \
    --add-module=/usr/local/src/nginx-master/nginx-modules/ngx_pagespeed \
    --add-module=/usr/local/src/nginx-master/nginx-modules/ngx_devel_kit
    make
    make install

## Add init file

## Install php55W

    rpm -Uvh http://mirror.webtatic.com/yum/el6/latest.rpm
    yum install -y libfbclient
    yum install -y libfbclient-devel
    yum install -y php-embedded-devel
    yum install -y php55w
    yum install -y php55w-opcache
    yum install -y php55w-common
    yum install -y php55w-fpm
    yum install -y php55w-cli
    yum install -y php55w-dba
    yum install -y php55w-devel
    yum install -y php55w-embedded
    yum install -y php55w-enchant
    yum install -y php55w-gd
    yum install -y php55w-imap
    yum install -y php55w-mbstring
    yum install -y php55w-mcrypt
    yum install -y php55w-mysql
    yum install -y php55w-odbc
    yum install -y php55w-pdo
    yum install -y php55w-pecl-memcache
    yum install -y php55w-pecl-xdebug
    yum install -y php55w-process
    yum install -y php55w-pspell 
    yum install -y php55w-recode
    yum install -y php55w-snmp
    yum install -y php55w-soap
    yum install -y php55w-tidy
    yum install -y php55w-xml
    yum install -y php55w-xmlrpc
    yum install -y php-firebird
    yum install -y php_database

## Install drush

    pear channel-discover pear.drush.org
    pear install drush/drush

