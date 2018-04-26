###########################################################################
#
#--------------------------------------------------------------------------
# Image Setup
#--------------------------------------------------------------------------
#
# To change its version, see the available Tags on the Docker Hub:
#    https://hub.docker.com/r/fromscratch/java/tags/
#
# Note: Base Image name format fromscratch/java:{java-version}u{java-update}
#
###########################################################################

FROM frolvlad/alpine-glibc:alpine-3.7

LABEL maintainer "from scratch Co.Ltd."

###########################################################################
# variables:
###########################################################################

ENV JAVA_VERSION=8 \
    JAVA_UPDATE=171 \
    JAVA_BUILD=11 \
    JAVA_PATH=512cd62ec5174c3487ac17c61aaa89e8 \
    JAVA_HOME="/usr/lib/jvm/default-jvm"

###########################################################################
# awscli and socat:
###########################################################################

RUN apk add --no-cache --virtual=build-dependencies wget ca-certificates unzip && \
    cd "/tmp" && \
    wget --header "Cookie: oraclelicense=accept-securebackup-cookie;" \
        "http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION}u${JAVA_UPDATE}-b${JAVA_BUILD}/${JAVA_PATH}/jdk-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz" && \
    tar -xzf "jdk-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz" && \
    mkdir -p "/usr/lib/jvm" && \
    mv "/tmp/jdk1.${JAVA_VERSION}.0_${JAVA_UPDATE}" "/usr/lib/jvm/java-${JAVA_VERSION}-oracle" && \
    ln -s "java-${JAVA_VERSION}-oracle" "$JAVA_HOME" && \
    ln -s "$JAVA_HOME/bin/"* "/usr/bin/" && \
    rm -rf "$JAVA_HOME/"*src.zip && \
    wget --header "Cookie: oraclelicense=accept-securebackup-cookie;" \
        "http://download.oracle.com/otn-pub/java/jce/${JAVA_VERSION}/jce_policy-${JAVA_VERSION}.zip" && \
    unzip -jo -d "${JAVA_HOME}/jre/lib/security" "jce_policy-${JAVA_VERSION}.zip" && \
    rm "${JAVA_HOME}/jre/lib/security/README.txt" && \
    apk del build-dependencies && \
    rm "/tmp/"*

###########################################################################
# add user 'circleci':
# cf. circleci/ruby:2.3.1-node
# https://hub.docker.com/r/circleci/ruby/tags/
###########################################################################

RUN addgroup -g 3434 circleci \
  && adduser -D -u 3434 -G circleci -s /bin/bash circleci \
  && echo 'circleci ALL=NOPASSWD: ALL' >> /etc/sudoers.d/50-circleci

USER circleci

WORKDIR /target

CMD ["/bin/sh"]