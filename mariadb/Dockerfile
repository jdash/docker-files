# jdash/mariadb Docker File
# Built on jdash/supervisor:latest (built on ubuntu:latest)
# Version 1.0

# Define the initial base image
FROM jdash/supervisor:latest

# Maintainer Information
MAINTAINER James M. Dasher, @jdash, james@jdasher.com

# Add the MariaDB Repo
RUN apt-get install -qq software-properties-common
RUN apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xcbcb082a1bb943db
RUN add-apt-repository 'deb http://mirrors.syringanetworks.net/mariadb/repo/10.0/ubuntu trusty main'

# Install MariaDB and pwgen
RUN apt-get update && apt-get install -qq mariadb-server pwgen

# Update MariaDB bind-address to accept connections from all interfaces
RUN sed -i -r 's/bind-address.*$/bind-address = 0.0.0.0/' /etc/mysql/my.cnf

# Add VOLUMEs to allow backup of config and databases
VOLUME  ["/etc/mysql", "/var/lib/mysql"]

# Expose the MariaDB port
EXPOSE 3306

# Add the supervisord configuration file for MariaDB
ADD mariadb.sv.conf /etc/supervisor/conf.d/

# Set supervisord to launch upon the start of the container
CMD supervisord -c /etc/supervisor.conf
