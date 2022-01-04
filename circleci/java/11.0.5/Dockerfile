###########################################################################
##
##--------------------------------------------------------------------------
## Image Setup
#--------------------------------------------------------------------------
#
# To change its version, see the available Tags on the Docker Hub:
#    https://hub.docker.com/r/fromscratch/java/tags/
#
# Note: Base Image name format fromscratch/java:{java-version}u{java-update}
#
###########################################################################

FROM frolvlad/alpine-glibc:alpine-3.11

LABEL maintainer "from scratch Co.Ltd."

###########################################################################
# variables:
###########################################################################

ENV JAVA_HOME="/usr/lib/jvm/default-jvm" \
    MYSQL_CONNECTOR_JAVA_VERSION=5.1.40

###########################################################################
# jdk:
###########################################################################

RUN apk add openjdk11

###########################################################################
## set PATH:
############################################################################

ENV PATH=$PATH:$JAVA_HOME/bin

###########################################################################
# python and awscli and curl:
###########################################################################

RUN apk update --no-cache && \
    apk add --no-cache --virtual=build-dependencies python py-pip py-setuptools ca-certificates && \
    pip install --no-cache-dir awscli && \
    apk del build-dependencies && \
    apk add --no-cache python curl && \
    rm -rf /var/cache/apk/*

###########################################################################
# add user 'circleci':
# cf. circleci/ruby:2.3.1-node
# https://hub.docker.com/r/circleci/ruby/tags/
# maven for executing maven
###########################################################################

RUN apk add --no-cache bash git openssh tar gzip sudo wget maven && \
  addgroup -g 3434 circleci && \
  adduser -D -u 3434 -G circleci -s /bin/bash circleci && \
  echo 'circleci ALL=NOPASSWD: ALL' > /etc/sudoers

USER circleci

WORKDIR /target

CMD ["/bin/sh"]
