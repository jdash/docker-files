# jdash/nginx Docker File
# Built on jdash/supervisor:latest (built on ubuntu:latest)
# Version 1.0

# Define the initial base image
FROM jdash/supervisor:latest

# Maintainer Information
MAINTAINER James M. Dasher, @jdash, james@jdasher.com

# Install necessary software
RUN apt-get install -qq wget tar build-essential apache2-threaded-dev libxml2-dev libcurl4-openssl-dev zlib1g-dev libpcre3 libpcre3-dev 1>/dev/null

# Install ModSecurity for Nginx
RUN \
  wget https://www.modsecurity.org/tarball/2.8.0/modsecurity-2.8.0.tar.gz && \
  tar -xf modsecurity-2.8.0.tar.gz && \
  cd modsecurity-2.8.0 && \
  ./configure --enable-standalone-module 1>/dev/null && \
  make 1>/dev/null && \
  make install 1>/dev/null

# Install PageSpeed for Nginx
RUN \
  wget https://github.com/pagespeed/ngx_pagespeed/archive/v1.8.31.2-beta.tar.gz && \
  tar -xf v1.8.31.2-beta.tar.gz && \
  cd ngx_pagespeed-1.8.31.2-beta/ && \
  wget https://dl.google.com/dl/page-speed/psol/1.8.31.2.tar.gz && \
  tar -xzf 1.8.31.2.tar.gz

# Install Nginx
RUN \
  wget http://nginx.org/download/nginx-1.7.0.tar.gz && \
  tar -xf nginx-1.7.0.tar.gz && \
  cd nginx-1.7.0 && \
  ./configure --add-module=../modsecurity-2.8.0/nginx/modsecurity --add-module=../ngx_pagespeed-1.8.31.2-beta --user=www-data --group=www-data --with-http_ssl_module --with-http_realip_module \
	--with-http_gzip_static_module --with-http_spdy_module && \
  make 1>/dev/null && \ 
  make install 1>/dev/null && \ 
  make clean 1>/dev/null

# Clean up image and shrink size
RUN \
  cd modsecurity-2.8.0 && \
  make clean 1>/dev/null && \
  cd .. && \
  apt-get remove -qq build-essential && \ 
  apt-get clean && \
  rm -f -r /var/lib/apt/lists/archive* modsecurity-2.8.0* nginx-1.7.0* ngx_pagespeed* v1.8.31.2-beta.tar.gz

# Expose HTTP & HTTPS ports
EXPOSE 80
EXPOSE 443

# Add the supervisord configuration file for hhvm
ADD nginx.sv.conf /etc/supervisor/conf.d/

# Set supervisord to launch upon the start of the container
