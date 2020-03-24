FROM google/cloud-sdk:alpine

LABEL maintainer "from scratch Co.Ltd."

ENV GOPATH /go
ENV PATH $GOPATH/bin:/usr/local/bin:$PATH

# Install Go
ENV GO_VERSION 1.11.13
RUN set -eux && \
	apk --no-cache add \
		bash \
		curl \
		g++ \
		gcc \
		git \
		libc6-compat \
		make \
		musl-dev \
		openssh-client \
		py-crcmod \
		python && \
	\
    curl -Lso go.tar.gz "https://dl.google.com/go/go${GO_VERSION}.linux-amd64.tar.gz" \
    && tar -C /usr/local -xzf go.tar.gz \
    && rm go.tar.gz
ENV PATH /usr/local/go/bin:$PATH

# Install dep
RUN go get -u github.com/golang/dep/cmd/dep
