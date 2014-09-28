# jdash/hhvm Docker File
# Built on jdash/supervisor:latest (built on ubuntu:latest)
# Version 1.0

# Define the initial base image
FROM jdash/supervisor:latest

# Maintainer Information
MAINTAINER @jdash

# Install wget to add the repo info
RUN apt-get install -qq wget

# Add the HHVM repo to apt-get
RUN wget -O - http://dl.hhvm.com/conf/hhvm.gpg.key | apt-key add -
RUN echo deb http://dl.hhvm.com/ubuntu trusty main | tee /etc/apt/sources.list.d/hhvm.list
RUN apt-get update && apt-get install -qq hhvm

# Expose the HHVM FastCGI port
EXPOSE 9000

# Add the supervisord configuration file for hhvm
ADD hhvm.sv.conf /etc/supervisor/conf.d/

# Set supervisord to launch upon the start of the container
ENTRYPOINT ["supervisord"]
CMD ["--configuration=/etc/supervisor.conf"]
