FROM ubuntu:latest

ENV TZ UTC
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update \
  && apt-get -y install curl tzdata \
  && dpkg-reconfigure --frontend noninteractive tzdata

RUN curl --proto '=https' --tlsv1.2 -sSf https://tiup-mirrors.pingcap.com/install.sh -O && sh install.sh \
    && /root/.tiup/bin/tiup install playground pd tikv tiup grafana tiflash prometheus

ENV PATH="/root/.tiup/bin:${PATH}"

EXPOSE 2379 3000 4000 9090 10080

CMD ["/root/.tiup/bin/tiup", "playground", "--host", "0.0.0.0", "--db", "1", "--kv", "1", "--pd", "1", "--tiflash", "1"]
