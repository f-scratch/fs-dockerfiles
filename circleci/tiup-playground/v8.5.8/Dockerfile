FROM ubuntu:22.04

ENV TZ=UTC
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
  && apt-get -y install curl tzdata \
  && dpkg-reconfigure --frontend noninteractive tzdata \
  && rm -rf /var/lib/apt/lists/*

# 起動時に配布サーバーへ取りに行かないよう、playground と全部品を版を固定して入れておく
RUN curl --proto '=https' --tlsv1.2 -sSf https://tiup-mirrors.pingcap.com/install.sh -O && sh install.sh && rm install.sh \
  && /root/.tiup/bin/tiup install playground:v1.17.1 tidb:v8.5.8 tikv:v8.5.8 pd:v8.5.8

ENV PATH="/root/.tiup/bin:${PATH}"

EXPOSE 2379 4000 10080

CMD ["tiup", "playground:v1.17.1", "v8.5.8", "--host", "0.0.0.0", "--db", "1", "--kv", "1", "--pd", "1", "--tiflash", "0", "--without-monitor"]
