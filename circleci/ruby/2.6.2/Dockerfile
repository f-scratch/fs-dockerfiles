###########################################################################
#
#--------------------------------------------------------------------------
# Image Setup
#--------------------------------------------------------------------------
#
# To change its version, see the available Tags on the Docker Hub:
#    https://hub.docker.com/r/fromscratch/ruby/tags/
#
# Note: Base Image name format fromscratch/ruby:{ruby-version}-amazonlinux{amazonlinux-version}-{ami-version}
#
###########################################################################

FROM amazonlinux:2.0.20181114

LABEL maintainer "from scratch Co.Ltd."

###########################################################################
# Versions:
###########################################################################

ENV RUBY_MAJOR 2.6
ENV RUBY_VERSION 2.6.2
ENV NODE_VERSION 10.15.3
ENV YARN_VERSION 1.17.3

###########################################################################
# Set Timezone
###########################################################################

ENV TZ Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

###########################################################################
# Common:
###########################################################################

RUN yum update -y && \
    yum install -y git gcc gcc-c++ openssl openssl-devel readline readline-devel mysql mysql-devel bzip2 postgresql postgresql-devel sudo tar

###########################################################################
# Ruby:
###########################################################################

RUN mkdir -p /build/ruby && \
    curl -s https://cache.ruby-lang.org/pub/ruby/$RUBY_MAJOR/ruby-$RUBY_VERSION.tar.gz | tar -C /build/ruby -xzf - && \
    cd /build/ruby/ruby-$RUBY_VERSION && \
    ./configure --disable-install-doc && \
    make && make install && \
    gem install bundler && \
    rm -fr /build/ruby

###########################################################################
# node:
###########################################################################

ENV NVM_DIR /root/.nvm

RUN echo $HOME && \
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.33.8/install.sh | bash && \
    . $NVM_DIR/nvm.sh && \
    nvm install ${NODE_VERSION} && \
    nvm use ${NODE_VERSION} && \
    nvm alias ${NODE_VERSION} && \
    npm install eslint --save-dev && \
    echo "" >> ~/.bashrc && \
    echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.bashrc && \
    echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm' >> ~/.bashrc

###########################################################################
# YARN:
###########################################################################

RUN [ -s "$NVM_DIR/nvm.sh" ] && \
    . $NVM_DIR/nvm.sh && \
    curl -o- -L https://yarnpkg.com/install.sh | bash -s -- --version ${YARN_VERSION} && \
    echo "" >> ~/.bashrc && \
    echo 'export PATH="$HOME/.yarn/bin:$PATH"' >> ~/.bashrc

###########################################################################
# awscli and socat:
###########################################################################

RUN curl -o- https://bootstrap.pypa.io/get-pip.py | python && \
    pip install pip --upgrade && \
    pip install awscli

WORKDIR /target

CMD ["/bin/bash"]
